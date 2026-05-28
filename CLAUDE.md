# Foot Solutions Bot — Claude Code Briefing

## What This Is

An AI-powered sales and customer service assistant for Foot Solutions — the #1 foot wellness franchise in the US. Built by Aravind Murari (Kepram LLC) as a demo pitch to Bryan Scott, co-owner and EVP of Strategic Growth at Foot Solutions.

## Client Context

- **Client:** Foot Solutions (footsolutions.com) — franchise HQ in Atlanta, GA
- **Key contact:** Bryan Scott, co-owner and EVP of Strategic Growth
  - Former NFL player (Atlanta Falcons, 2003 draft; also played Saints and Bills)
  - Shark Tank winner (Mark Cuban + Daymond John deal for his shock-absorbing insole product)
  - Based in Atlanta. Known from BNI.
- **Georgia locations (5 stores):** Brookhaven/Atlanta, East Cobb/Marietta, Acworth, Cumming, Peachtree City
- **Primary product:** Custom 3D-printed orthotics ($400–$500) via laser scanning and gait analysis
- **Other products:** 10+ orthopedic shoe brands, OTC insoles ($119.99)

## Bot Purpose

Sales assistant + foot health expert. Unlike a real estate bot where the bot just qualifies leads, this bot is expected to:
1. Answer detailed, scientific foot health questions
2. Recommend specific products by name and price
3. Upsell toward custom orthotics naturally
4. Serve customers at off-hours when stores are closed
5. Direct customers to the nearest Georgia store when ready for in-person help

## Tech Stack

| Layer | Tool |
|-------|------|
| LLM | Claude API (`claude-sonnet-4-6`) |
| RAG | LlamaIndex via Voyage AI embeddings + Pinecone |
| Embeddings | Voyage AI `voyage-3-lite` (512 dims) |
| Vector DB | Pinecone Serverless (`foot-solutions-knowledge`) |
| Backend | Python 3.11 + FastAPI |
| Frontend | Vanilla HTML/CSS/JS |
| Hosting | Railway (backend) + GitHub Pages (frontend) |

## Project Structure

```
foot-solutions-bot/
├── CLAUDE.md                    ← you are here
├── .env                         ← never commit
├── .env.example                 ← template
├── context/
│   └── strategy.md              ← pitch strategy and business context
├── knowledge/                   ← RAG source files
│   ├── overview.md              ← company overview, Bryan Scott, franchise stats
│   ├── services.md              ← custom orthotics process, gait analysis
│   ├── products.md              ← full product catalog with prices
│   ├── conditions.md            ← foot conditions: science, symptoms, solutions
│   ├── faq.md                   ← common questions and answers
│   └── locations.md             ← all 57 store locations (Georgia highlighted)
├── backend/
│   ├── requirements.txt
│   ├── config.py                ← system prompt + model settings (edit per client)
│   ├── main.py                  ← FastAPI app (copy from template)
│   ├── rag.py                   ← Voyage AI + Pinecone + Claude (copy from template)
│   ├── ingest.py                ← one-time ingestion script (copy from template)
│   └── logger.py                ← optional Supabase chat logging
└── frontend/
    ├── index.html               ← chat widget
    ├── style.css                ← Navy/blue Foot Solutions color palette
    └── config.js                ← bot name, tagline, opening message, API URL
```

## Running Locally

```bash
# 1. Setup — do this once
cd /Users/aravindmurari/Projects/foot-solutions-bot/backend
/opt/homebrew/bin/python3.11 -m venv venv
venv/bin/python3 -m pip install -r requirements.txt

# 2. Create .env from .env.example and fill in API keys
cp ../.env.example ../.env

# 3. Ingest knowledge base into Pinecone
venv/bin/python3 ingest.py

# 4. Start the server
venv/bin/python3 -m uvicorn main:app --port 8003

# 5. Open the frontend
open ../frontend/index.html
```

## Port Allocation

| App | Port |
|-----|------|
| Isha MoM | 8000 |
| Kepram bot | 8001 |
| Trinity CRE bot | 8002 |
| **Foot Solutions bot** | **8003** |

## Environment Variables Required

```
ANTHROPIC_API_KEY    — console.anthropic.com
PINECONE_API_KEY     — app.pinecone.io
PINECONE_INDEX_NAME  — foot-solutions-knowledge
VOYAGE_API_KEY       — voyageai.com
SUPABASE_URL         — optional (chat logging)
SUPABASE_SERVICE_KEY — optional (chat logging)
```

## Files That Need Editing Per Deployment

- `backend/config.py` — system prompt, bot persona (already done)
- `frontend/config.js` — bot name, tagline, opening message, API URL
- `frontend/style.css` — brand colors (already done — navy/gold Foot Solutions palette)

## Deployment Checklist

- [ ] Copy .env.example to .env and fill in keys
- [ ] Run `venv/bin/python3 ingest.py` to build Pinecone index
- [ ] Test locally on port 8003
- [ ] Deploy backend to Railway
- [ ] Update `apiUrl` in `frontend/config.js` to Railway URL
- [ ] Deploy frontend to GitHub Pages (repo: foot-solutions-bot)
- [ ] Send link to Bryan Scott
