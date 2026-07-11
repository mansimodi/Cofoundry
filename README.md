# CoFoundry

A searchable, categorized directory of every project submitted in the **FWA AI Launchpad — Part 2** cohort (Weeks 6–8), built so classmates can actually find each other's work and find collaborators — instead of scrolling a Teams channel one post at a time.

Built by **Mansi Modi** and **Sri Annamraju**, using **Claude** (Anthropic) for extraction, categorization, and the frontend, and **n8n** for the AI-powered search backend.

---

## Why this is being built

Microsoft Teams (free/consumer tier) has no admin export, no API access, and no way to browse "all project submissions" as a set — just a long, un-filterable scroll of individual posts. Finding a specific classmate's project, or finding everyone working on something similar to your own idea, meant manually scrolling and opening post after post. It's tiresome, and it's not an efficient way for a cohort to see each other's work or find collaborators.

## What it does

CoFoundry collects all the projects submitted across Weeks 6, 7, and 8 of the AI Launchpad Part 2 course, automatically categorizes them into 16 topic areas, and makes them browsable and searchable in one place:

- **Browse by category** — 16 categories (Business & Analytics, Healthcare & Medical, Career & Job Search, and more), each showing every project filed under it, with author, tool used, and a link back to the original Teams post.
- **Authors and collaboration** — see every author, how many projects they've built, which categories they've worked across, and a "Cross-domain builder" badge for anyone spanning 3+ categories — the fastest way to spot a potential collaborator.
- **Ask CoFoundry** — a natural-language search box, backed by an n8n workflow, so you can ask things like "who's building something with LangChain?" instead of filtering manually.
- **Clickable stats and category legend** — the project/author/category counts at the top double as filters; category badges are clickable everywhere they appear.

By the numbers: **97 projects**, **53 authors**, **16 categories**, **96 of 97 projects** linked directly back to their original Teams post.

## Who it's for

- **Cohort members (the ~53 authors and everyone else in the course)** — to find what classmates built, avoid duplicating effort, and find people to team up with on similar ideas.
- **Course organizers (FWA / World Alliance)** — as a lightweight, zero-cost way to surface the full body of work from a cohort without needing Teams admin access.

## Where it lives

CoFoundry is a single self-contained HTML page (no server, no build step) hosted for free on **Vercel**. Search is powered by an **n8n** webhook; the page and all the extraction/categorization tooling behind it were built with **Claude**.

---

## How it was built

1. **Extraction** — Since this Teams workspace is a free/consumer tier with no Graph API or admin export, all 97 posts were extracted via browser automation (Claude in Chrome), reading each post's DOM structure directly (`data-mid` / `data-reply-chain-id` attributes) to distinguish top-level submissions from replies.
2. **Categorization** — Each project was read and classified into one of 16 mutually-exclusive categories based on its primary theme.
3. **Teams-post linking** — Using Teams' own "Copy Link" action on each message (the only reliable way to get a working deep link without API access), 96 of 97 projects were matched back to their original post.
4. **Directory + search** — A single HTML/CSS/JS file renders the browsable directory; a connected n8n webhook (`Ask CoFoundry`) handles natural-language search over the same dataset.
5. **Testing** — An automated test suite (147 checks) validates category/data integrity and the search-rendering logic; a separate live link audit (37 checks) verified every project demo link actually resolves to the right content.

## Pain points & known limitations

- **Teams' free tier made extraction slow and brittle.** With no export or API access, every post had to be opened and parsed individually through browser automation — this repeatedly hit runtime errors and required multiple recovery passes (scroll-based capture, then a targeted search-based fallback) to get from 0 → 96 of 97 Teams links successfully captured.
- **Attachment-only posts were hard to parse.** 16 of the 97 submissions leaned on a screenshot or attached file for the actual project explanation, with little to no inline text — these are flagged separately (see the "Flagged - Attachment Heavy" sheet in the companion Excel export) since the automated read of them is less reliable.
- **Some data is genuinely missing or unverified as a result:** one project ("Library Inventory Agent") has no recoverable Teams link after two search attempts; a small number of project demo links (the third-party app URLs authors shared, not the Teams links) turned out to be dead, mismatched to a different app, or duplicated across two unrelated projects during a follow-up link audit.

## Next steps

To make this process seamless going forward — and to support **real-time classification** as new projects are submitted, instead of periodic manual extraction passes — the next step would be getting **admin rights to the Teams channel** (or upgrading off the free tier). That would unlock the Graph API, letting extraction and categorization run automatically instead of through browser automation. This is a natural next step for **Future World Alliance** to take on if there's appetite to keep this running beyond one cohort.

---

## Tech stack

| Layer | Tool |
|---|---|
| Extraction | Claude in Chrome (browser automation) |
| Categorization & tooling | Claude |
| Frontend | Static HTML/CSS/JS (single file, no build step) |
| Search backend | n8n (webhook) |
| Hosting | Vercel (free tier) |
| Testing | Custom Node.js test suite (184 automated checks) |

## Credits

Built for the FWA AI Launchpad — Part 2 cohort by **Mansi Modi** and **Sri Annamraju**.
