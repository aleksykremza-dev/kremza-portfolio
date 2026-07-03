# Oleksii Kremza — Applied AI & Automation Engineering

I design exhibition stands for a living — and automate every process around me that deserves it. Everything below runs in production and is used daily: CAD plugins that cut modeling time by ~80%, a payroll/time-tracking system my company relies on, an LLM-driven recruitment pipeline that handled correspondence and scheduling on its own.

Poznań, Poland · [GitHub](https://github.com/aleksykremza-dev) · aleksykremza-dev

---

## Case studies

### 1. CAD Automation Plugin Suite — Ruby / Python

**Problem.** In peak season one designer runs 5–6 exhibition stands in parallel, deadlines fixed by fair dates. The hardest part is engineering calculation: rigging (suspended structures) alone took a full working day, sometimes two — with high error risk.

**Solution.** A set of plugins for SketchUp (Ruby API) and Fusion 360 (Python API) that work like configurators: enter dimensions and material — get a production-ready stand element with documentation.

- **Floors** — three layers (structure, base, facing); stand and ramp outlines merged with boolean operations, panels laid out with edge-cut tiling
- **Walls** — free space computed by coordinates, filled with standard warehouse frames plus one custom closing frame; greedy layout with merging of small offcuts (design-for-manufacturing)
- **Cladding** — parametric panel layout with deduplication of identical elements for the BOM
- **Rigging** — load distributed to nearest suspension points (Voronoi), trusses treated as a graph, segment lengths picked from warehouse stock (bin-packing / subset-sum), per-point weight checked against hall limits (25–200 kg), exported to Excel

**Stack:** Ruby (SketchUp API), Python (Fusion 360 API), computational geometry, graph algorithms, custom XLSX export in pure Ruby (OOXML + ZIP, no external libraries).

**Result: stand modeling takes ~80% less time.**

### 2. Time Tracking & Payroll System — Microsoft 365

**Problem.** Hours were logged manually, payroll counted in Excel. The real pain: a project runs for months, payroll is monthly — attributing each employee's hours to specific projects was nearly impossible.

**Solution.** A system on Excel Online + Office Scripts (TypeScript) + Power Automate + SharePoint:

- one entry screen; dropdowns (employees, active projects, holidays) refresh themselves on schedule
- each day auto-archives to the monthly ledger with write verification — a row leaves the entry sheet only after a confirmed archive write
- payroll computes itself: raw hours split into regular / overtime / holiday, individual rates and multipliers applied
- reports per project and month; a personal signable hours file generated for every employee
- master data (people, projects, rates) lives in one place, separate from daily entry
- deployment done by editing `.osts` files directly via PowerShell, bypassing the unreliable web editor

**Stack:** Microsoft 365, Office Scripts (TypeScript/ExcelScript), Power Automate, Power Query, SharePoint/OneDrive, PowerShell.

**Result: exact hours-per-project visibility across 40+ projects; zero manual data transfer.**

### 3. LLM-Driven Recruitment Pipeline — Node.js

**Problem.** Recruiting recruiters: thousands of applications per posting. The drowning wasn't in interviews — it was in routine: create a folder and sheet for each one, grant access, aggregate everything.

**Solution.** A Node.js backend that runs the routine end-to-end:

- applications from a form land in a sheet and are auto-filtered by criteria (country, language, age)
- the system messages matching candidates on WhatsApp and proposes interview slots
- candidates reply in natural language — the LLM parses the time and books it in Google Calendar with collision control
- Telegram bot sends approve/reject buttons; automatic reminders 10 minutes before the call
- after the interview: automatic onboarding — folder, personal sheet, access grants
- an aggregator pulls every recruiter's sheet into one master view; status changes flow back to the right sheet via UUID matching
- the whole thing is driven from a Telegram bot: applications, schedule, stats, automation mode, blacklist

**Stack:** Node.js, Google API (Sheets / Drive / Calendar / Forms), whatsapp-web.js, Telegram bot, LLM API.

**Result: filtering, correspondence, scheduling, onboarding and aggregation ran themselves — only interviews and decisions remained.**

### 4. Company Database + Hot-Lead Engine — Node.js + SQLite

**Problem.** Find companies that hire foreign workers. Paid services charge for this and forbid scraping; official open data is free but raw.

**Solution.** A local database of ~4M Romanian legal entities from official open data (ONRC registry + ANAF tax authority), joined with mandatory job postings from the state employment agency (ANOFM) — postings are an exact signal of who is hiring right now.

- finds and downloads the latest registry dump (~685 MB), loads 4M companies in ~25 minutes
- scrapes ~10k job postings, matches them against the base
- enriches: VAT status and financials from the tax API, phones and emails from company websites
- GDPR-clean: natural persons filtered out, do-not-call list honored (incl. Romanian law 190/2018)
- the same architecture was ported to a second market (Slovenia) with code reuse: recruiting-intensity scoring over public ads, agencies filtered out, TOP-50 SME employers returned

**Stack:** Node.js 24, node:sqlite + FTS5, cheerio, p-queue, ONRC/ANAF APIs.

**Result: from 4M companies to ~1480 verified employers actively hiring, with contacts — at $0/month.**

---

## Open repositories

- **[invisible-prompt-lab](https://github.com/aleksykremza-dev/invisible-prompt-lab)** — LLM security research: hidden Unicode Tags instructions (prompt injection), encoder/decoder + detection, 17/17 tests (Python, pytest)
- **[ai-automation-lab](https://github.com/aleksykremza-dev/ai-automation-lab)** — public learning lab: closing my skill gaps (RAG, agents, n8n) with working artifacts, in public

## Certificates

Google AI Essentials · Google Cybersecurity Certificate · ISC2 CC (exam scheduled)
