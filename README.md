# OpenSEO (Self-Hosted Community Edition)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Customized by](https://img.shields.io/badge/Customized%20by-Md.%20Redwan%20Ahmed-green.svg)](https://github.com/redwan-cse)
[![Upstream](https://img.shields.io/badge/Upstream-every--app%2Fopen--seo-orange.svg)](https://github.com/every-app/open-seo)
[![Architecture](https://img.shields.io/badge/Stack-Docker%20%7C%20SearXNG%20%7C%20Browserless%20%7C%20Manifest-purple.svg)](#architecture)
[![Autonomous Sync](https://img.shields.io/badge/Sync-AGY%20CLI%20Watchdog-brightgreen.svg)](#automated-upstream-synchronization)

> **A 100% self-hosted, unmetered, privacy-first distribution of OpenSEO.**  
> Built and maintained by [Md. Redwan Ahmed](https://github.com/redwan-cse) based on the foundational work by [Ben Senescu](https://github.com/bensenescu) and [every-app/open-seo](https://github.com/every-app/open-seo).

---

## ?? Upstream Credits & Licensing

This project is a customized, self-hosted distribution of the excellent open-source [OpenSEO](https://github.com/every-app/open-seo) created by **[Ben Senescu](https://github.com/bensenescu)** and the [EveryApp](https://github.com/every-app) team.

- **Original Project:** [every-app/open-seo](https://github.com/every-app/open-seo)
- **Hosted Platform:** [openseo.so](https://openseo.so)
- **License:** [MIT License](./LICENSE) ? All original copyrights, license terms, and author notices are strictly preserved.

---

## ?? Why This Custom Edition?

While upstream OpenSEO is an incredible open-source alternative to Semrush and Ahrefs, standard self-hosting still mandates third-party paid commercial APIs:
1. **DataForSEO** (paid account required for search volume, organic SERP, keyword ideas, and LLM mentions).
2. **Context.dev** (paid per-page API required to unlock the **"Render JavaScript"** toggle in Site Audits).

### Key Enhancements in this Distribution:

| Capability | Upstream OpenSEO | Redwan's Custom Self-Hosted Edition |
| :--- | :--- | :--- |
| **Search Engine Data** | Requires paid DataForSEO API balance | **100% Self-Hosted SearXNG Cluster** (unmetered Google/Bing organic scraping) |
| **AI LLM Visibility** | Commercial OpenRouter API keys | **Local Manifest AI Gateway** + Auto-fallback prompt relevance pipeline |
| **JavaScript Rendering** | Requires paid Context.dev API subscription | **Local Headless Chrome Engine (`browserless/chrome`)** with zero per-page fees |
| **Network Support** | Strict loopback / HTTPS requirements | **Full RFC1918 LAN & Private Network Access** (automatic CORS & CSRF bypass) |
| **Lifecycle & Updates** | Manual container upgrades | **Autonomous AGY CLI Sync Watchdog** (retains self-hosted adapters across updates) |
| **Ongoing Costs** | Monthly API billing + usage markups | **$0 / Month Ongoing API Costs** |

---

## ??? Architecture

```
                                  ?????????????????????????????????
                                  ?      Client Browser / UI      ?
                                  ?????????????????????????????????
                                                  ? (LAN / Localhost :3009)
                                                  ?
                                  ?????????????????????????????????
                                  ?        Nginx Web Proxy        ?
                                  ?????????????????????????????????
                                                  ?
                                                  ?
                                  ?????????????????????????????????
                                  ?     OpenSEO Core Runtime      ?
                                  ?       (celld / Workers)       ?
                                  ?????????????????????????????????
                                          ?               ?
                     (Scrape / Context)   ?               ?   (SERP / LLM Requests)
                                          ?               ?
                        ?????????????????????????????????????????????
                        ?       Self-Hosted Bridge Middleware       ?
                        ?               (Node.js)                   ?
                        ?????????????????????????????????????????????
                                  ?                       ?
                                  ?                       ?
                    ??????????????????????????  ??????????????????????????
                    ?   Browserless Engine   ?  ?   SearXNG Metasearch   ?
                    ?   (Headless Chromium)  ?  ?   + Manifest AI Gateway?
                    ??????????????????????????  ??????????????????????????
```

---

## ? Features & Workflows

### 1. Keyword Research & Organic SERP
- Discover high-intent queries and search volumes powered by local **SearXNG** aggregation.
- Real-time ranking previews, title tags, descriptions, and snippet extraction with zero API throttling.

### 2. AI Visibility & Prompt Research
- Track how AI models (ChatGPT, Claude, Gemini, Perplexity) perceive and cite your domain.
- Generates natural, context-aware prompt variations and brand mentions using **Manifest AI Gateway**.
- Resilient JSON normalization and fallback heuristics to eliminate blank results or schema errors.

### 3. Site Audit with Client-Side JavaScript Execution
- Full SPA crawling (React, Next.js, Vue, Angular, Svelte) via integrated **Headless Chrome**.
- Evaluates dynamically injected headings, links, internal architectures, and Lighthouse performance scores.
- Automatic DOM state validation (`loaded`) with direct HTML fallback resilience.

### 4. Agent Skills & MCP Integration
OpenSEO exposes a native Model Context Protocol (MCP) server so autonomous coding agents can orchestrate your SEO pipeline:
- Compatible with **Antigravity (AGY)**, **Claude Code**, **OpenClaw**, **Hermes**, and **Codex**.
- Reusable agent skills for keyword auditing, content briefs, and ranking checks.

---

## ??? Self-Hosting Guide

### Prerequisites
- Docker Engine & Docker Compose (v2.20+)
- 4 GB+ RAM recommended (for running the headless browser engine)
- Linux VPS (Ubuntu 22.04 / 24.04 LTS recommended)

### Quick Start

1. **Clone this repository**:
   ```bash
   git clone git@github.com:redwan-cse/open-seo.git
   cd open-seo/deploy/standalone
   ```

2. **Configure your environment**:
   ```bash
   cp .env.example .env
   ```
   Set your local keys:
   ```env
   AUTH_MODE=password
   BETTER_AUTH_URL=http://192.168.10.40:3009
   PORT=3009
   STORAGE_ACCESS_KEY=openseo
   STORAGE_SECRET_KEY=<generate-32-byte-hex>

   # Self-hosted adapters enable full features without paid external accounts:
   DATAFORSEO_API_KEY=selfhosted-local-bridge
   CONTEXT_API_KEY=selfhosted-browser-bridge
   OPENROUTER_API_KEY=local-manifest-key
   ```

3. **Deploy the stack**:
   ```bash
   docker compose up -d
   ```

4. **Access OpenSEO**:
   Open your browser at `http://<your-vps-ip>:3009`. The first visitor creates the administrator account!

---

## ?? Automated Upstream Synchronization

This repository uses an autonomous DevOps pipeline powered by the **Google Antigravity (AGY) CLI** and a persistent `systemd` watchdog:

- **Schedule:** Runs every 3 days at 03:00 UTC.
- **Offline Catch-Up (`Persistent=true`):** If your server is offline or rebooting at the scheduled time, the watchdog automatically executes immediately once the server comes back online.
- **Permanent Adapter Preservation:** Core self-hosted configurations, bridges, and patches are managed in `seo-platform-infra` and applied dynamically on container boot?so upstream git updates will **never overwrite your self-hosted modules**.
- **Comprehensive Health Audit:** Inspects OpenSEO API health, SearXNG, Browserless, Bridge, and Manifest AI, recording audit results to `/logs/last_sync.json`.

---

## ?? License & Attribution

- **Base Project:** Copyright (c) 2026 Ben Senescu under the [MIT License](./LICENSE).
- **Self-Hosted Enhancements:** Maintained by [Md. Redwan Ahmed](https://github.com/redwan-cse).
- Feel free to fork, adapt, and deploy your own private SEO platform!
