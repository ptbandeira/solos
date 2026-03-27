---
name: web-scraper
description: >
  Web scraping and crawling powered by a self-hosted Firecrawl instance.
  Use when the user says "scrape", "crawl", "extract from", "get content from",
  "research this site", "pull data from", "map this website", "what's on this page",
  or any request to retrieve structured data from a URL.
  Also triggers on "set up scraper", "configure firecrawl", or "scraping setup".
version: 1.0.0
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch
---

# Web Scraper

A self-hosted web scraping engine built on [Firecrawl](https://github.com/mendableai/firecrawl) (AGPL-3.0). Runs locally via Docker — no API keys, no rate limits, no per-page billing. Shared across all projects in the workspace.

---

## SETUP (first-time only)

**Trigger:** User asks to set up the scraper, or a scrape command fails because Firecrawl isn't running.

### Prerequisites

- Docker installed and running
- ~2GB disk space for images

### Step 1 — Clone Firecrawl

```bash
git clone https://github.com/mendableai/firecrawl.git ~/.firecrawl
cd ~/.firecrawl
```

### Step 2 — Configure environment

```bash
cp apps/api/.env.example apps/api/.env
```

Edit `apps/api/.env` — set these minimal values:

```env
PORT=3002
HOST=0.0.0.0
USE_DB_AUTHENTICATION=false
BULL_AUTH_KEY=solos-scraper
NUM_WORKERS_PER_QUEUE=4
REDIS_URL=redis://redis:6379
```

Optional — add if you want AI-powered extraction:
```env
OPENAI_API_KEY=sk-...
```

### Step 3 — Start

```bash
cd ~/.firecrawl && docker compose up -d
```

Services started:
- `api` → `http://localhost:3002` (main scraping API)
- `redis` → queue and cache
- `playwright-service` → headless browser for JS-heavy sites

### Step 4 — Verify

```bash
curl http://localhost:3002/test
```

Returns `OK` when ready.

### Stop / restart

```bash
docker compose -f ~/.firecrawl/docker-compose.yml stop
docker compose -f ~/.firecrawl/docker-compose.yml up -d
```

---

## USAGE

All requests go to `http://localhost:3002`. No auth header needed when `USE_DB_AUTHENTICATION=false`.

---

### SCRAPE — single page

Returns clean markdown (or HTML/JSON) from one URL.

```bash
curl -s -X POST http://localhost:3002/v1/scrape \
  -H 'Content-Type: application/json' \
  -d '{
    "url": "https://example.com",
    "formats": ["markdown"],
    "onlyMainContent": true
  }'
```

**Key options:**

| Option | Default | Notes |
|--------|---------|-------|
| `formats` | `["markdown"]` | Also: `html`, `links`, `screenshot`, `json` |
| `onlyMainContent` | `true` | Strips nav, footer, ads |
| `waitFor` | `0` | ms to wait before scraping (for JS pages) |
| `timeout` | `30000` | Max ms per request |
| `mobile` | `false` | Emulate mobile viewport |
| `actions` | `[]` | Pre-scrape interactions (click, scroll, type) |

**Response:**
```json
{
  "success": true,
  "data": {
    "markdown": "# Page Title\n\nContent...",
    "metadata": {
      "title": "Page Title",
      "description": "...",
      "url": "https://example.com",
      "statusCode": 200
    }
  }
}
```

---

### CRAWL — entire site

Crawls a domain and returns all pages as markdown. Returns a job ID — poll for results.

```bash
# Start crawl
curl -s -X POST http://localhost:3002/v1/crawl \
  -H 'Content-Type: application/json' \
  -d '{
    "url": "https://example.com",
    "limit": 50,
    "scrapeOptions": {
      "formats": ["markdown"],
      "onlyMainContent": true
    }
  }'
# Returns: { "success": true, "id": "job-id-here" }

# Check status + get results
curl -s http://localhost:3002/v1/crawl/{job-id}
```

**Key options:**

| Option | Default | Notes |
|--------|---------|-------|
| `limit` | `10` | Max pages to crawl |
| `maxDepth` | unlimited | Max link depth from start URL |
| `includePaths` | `[]` | Only crawl URLs matching these patterns |
| `excludePaths` | `[]` | Skip URLs matching these patterns |
| `allowBackwardLinks` | `false` | Follow links to parent paths |

**Poll until `status: "completed"`.** Response includes `data[]` array with one entry per page.

---

### MAP — discover all URLs

Returns a list of all URLs on a site without scraping content. Fast — uses sitemap when available.

```bash
curl -s -X POST http://localhost:3002/v1/map \
  -H 'Content-Type: application/json' \
  -d '{
    "url": "https://example.com",
    "limit": 1000
  }'
```

Use `search` param to filter results by relevance:
```json
{ "url": "https://example.com", "search": "pricing" }
```

**Response:** `{ "success": true, "links": ["https://...", ...] }`

---

### EXTRACT — structured data with schema

Pull specific fields from one or more pages using a JSON schema. Requires `OPENAI_API_KEY` in `.env`.

```bash
curl -s -X POST http://localhost:3002/v1/extract \
  -H 'Content-Type: application/json' \
  -d '{
    "urls": ["https://example.com/pricing"],
    "prompt": "Extract all pricing tiers with name, price, and features",
    "schema": {
      "type": "object",
      "properties": {
        "tiers": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "name":     { "type": "string" },
              "price":    { "type": "string" },
              "features": { "type": "array", "items": { "type": "string" } }
            }
          }
        }
      }
    }
  }'
```

---

## COMMON TASKS

### Scrape and save to second-brain

```bash
curl -s -X POST http://localhost:3002/v1/scrape \
  -H 'Content-Type: application/json' \
  -d '{"url": "URL_HERE", "formats": ["markdown"], "onlyMainContent": true}' \
  | python3 -c "import sys,json; d=json.load(sys.stdin); print(d['data']['markdown'])" \
  > second-brain/knowledge/FILENAME.md
```

### Research a competitor

1. `MAP` the site to see all pages
2. `SCRAPE` the homepage, pricing, and about pages
3. Save to `second-brain/knowledge/competitors/[name].md`
4. Add key findings to `ROUTER.md` under a new Competitors table

### Monitor a page for changes

Run scrape on a schedule (via cron or `/schedule` skill) and diff against the last saved version:
```bash
diff second-brain/knowledge/[page].md <(curl -s -X POST http://localhost:3002/v1/scrape ...)
```

### Scrape a JS-heavy page

Add `waitFor` to let JavaScript render before capture:
```json
{ "url": "https://spa-site.com", "waitFor": 3000, "formats": ["markdown"] }
```

---

## INTEGRATION WITH SOLOS MEMORY

| Output type | Save to |
|-------------|---------|
| Competitor research | `second-brain/knowledge/competitors/[name].md` |
| Industry/market research | `second-brain/knowledge/[topic].md` |
| Person/company profile | `second-brain/people/[name].md` |
| Project research | `second-brain/projects/[project]/research.md` |
| Raw crawl dumps | `outputs/scrapes/[domain]-[date].md` |

After saving, add an entry to `ROUTER.md` so the file is discoverable in future sessions.

---

## LIMITATIONS (self-hosted)

- No anti-bot bypass (Cloudflare, Akamai, etc.) — cloud proxies not available
- No `/agent` or `/browser` interactive endpoints
- Scraping rate limited only by your machine, not by external quotas
- `EXTRACT` endpoint requires OpenAI API key for LLM-powered schema extraction

If a scrape fails due to bot protection, try:
1. Add `"waitFor": 2000` to let JS load
2. Set `"mobile": true` (sometimes bypasses desktop-targeted blocks)
3. Use `WebFetch` tool as fallback for static pages

---

## HEALTH CHECK

```bash
# Is Firecrawl running?
curl -s http://localhost:3002/test

# Queue dashboard
open http://localhost:3002/admin/solos-scraper/queues
```
