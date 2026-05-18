# AtCoder AI Tagged Problems

> A topic-wise problem explorer for AtCoder - browse **problems** by **AI-generated topic tags**, filter by difficulty / contest, and track per-user solved status against any AtCoder handle.

[![Live](https://img.shields.io/badge/Live-Vercel-black?logo=vercel)](https://atcoder-ai-tagged-problem.vercel.app/)
[![Backend](https://img.shields.io/badge/Backend-Render-46E3B7?logo=render)](https://atcoder-tagged-backend.onrender.com)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript)](https://www.typescriptlang.org)
[![Vite](https://img.shields.io/badge/Vite-7-646CFF?logo=vite)](https://vitejs.dev)
[![Node.js](https://img.shields.io/badge/Node.js-20-339933?logo=node.js)](https://nodejs.org)

**Live App:** https://atcoder-ai-tagged-problem.vercel.app/
**API:** https://atcoder-tagged-backend.onrender.com

---

## Features

- **9,600+ Problems** - every public AtCoder problem
- **AI-Generated Tags** - auto-tagged using **Qwen LLM** (DP, Graph, Greedy, Math, DS, Strings, etc.).
- **Smart Search & Filters** - by problem ID, contest.
- **Per-User Solved Tracking** - enter any AtCoder handle, see solved status against problem list (powered by [kenkoooo API](https://kenkoooo.com/atcoder/))
- **Statistics Dashboard** - solve count by tag/difficulty
- **Dark Mode**

---

## Tech Stack

| Layer | Tools |
|---|---|
| **Frontend** | React 18, TypeScript, Vite 7, Bootstrap, React Router |
| **Backend** | Node.js 20, Express 5, sql.js (SQLite), CORS, rate-limit |
| **Edge / Proxy** | Vercel Edge Functions (TypeScript), CDN cache (`stale-while-revalidate`) |
| **Data Pipeline** | Python, Selenium, Qwen AI (tagging), pandas |
| **Hosting** | Frontend -> Vercel · Backend -> Render · Data source -> kenkoooo |
| **Observability** | Vercel Analytics, Vercel Speed Insights |
| **SEO** | sitemap.xml, robots.txt, JSON-LD (WebSite + WebApplication), OG/Twitter meta, Google Search Console |

---

## Performance & Scale (Engineering Highlights)

| Metric | Before | After | Improvement |
|---|---|---|---|
| **Backend offload (CDN cache hit)** | 0% | **99%** | Almost all traffic served at edge |
| **Live concurrent capacity** | ~25 users | **500+ users** | Verified via 1000-user ramp tests |
| **`/problems/all` latency** | 2500 ms | **17 ms** | **150x faster** (N+1 SQL fix) |
| **Server memory per request** | 512 MB OOM | **< 9 MB** | Stream-processed submission batches |
| **Initial JS payload (gzip)** | 93 KB | **15 KB** | Vite manual-chunking + `React.lazy` route splitting |
| **Client cache layers** | 1 | **3** | In-memory · sessionStorage · localStorage |

**Key engineering wins:**
1. **Vercel Edge Function reverse proxy** with per-route `s-maxage` + `stale-while-revalidate` + in-flight coalescing -> backend almost never gets hit
2. **N+1 SQL elimination** on `/problems/all` (one JOIN instead of N tag lookups)
3. **Stream-processed AtCoder submission batches** -> fixed OOM during user-solved sync
4. **Vite manual chunking** (`vendor-react`, `vendor-bootstrap`, route chunks) -> first-paint payload cut to 15 KB gz
5. **3-tier client cache** (memory -> sessionStorage -> localStorage) -> instant re-navigation
6. **SPA-aware reverse proxy** with regex rewrite (`/api/(.*) -> /api/proxy?p=$1`) preserving nested paths

---


---

## API Endpoints

Base URL: `https://atcoder-tagged-backend.onrender.com`

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Health check + DB problem count |
| `GET` | `/problems` | All problems (paginated) |
| `GET` | `/problems/all` | All problems (single payload, CDN-cached) |
| `GET` | `/problems/search?q=<query>` | Search by ID/contest/tag |
| `GET` | `/problems/tag/:tag` | Filter by tag |
| `POST` | `/problems/solved` | Bulk solved-status lookup |
| `GET` | `/problems/solved/user/:username` | User's solved problems |
| `GET` | `/tags` | All available tags |
| `GET` | `/contests` | All contests |
| `GET` | `/stats` | Aggregated statistics |


---

## Contributing

PRs welcome. For major changes, please open an issue first.


---

## License

[MIT](LICENSE) © 2025 [Lalit Verma](https://github.com/vlalit834)

---

## Credits

- [kenkoooo AtCoder API](https://kenkoooo.com/atcoder/) - user submission data
- [AtCoder](https://atcoder.jp/) - problem source
- [Qwen](https://qwenlm.github.io/) - AI tagging model
- Hosting: [Vercel](https://vercel.com) + [Render](https://render.com)

---

**Built for the competitive programming community.**
If this helped you, please star the repo!