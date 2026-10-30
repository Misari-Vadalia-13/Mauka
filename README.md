# Mauka: hackathons and internships that fit you

> Find opportunities. See your fit. Stay ahead of deadlines.

A web app that collects hackathons, contests and internships from multiple platforms into one feed, tells each student whether they are eligible, and sends reminders only for the opportunities they have saved.

**Status:** 🚧 In development (initial commit). Nothing below is claimed as finished until its checkbox is ticked in the [Roadmap](#roadmap).

---

## The problem

Opportunities for students are scattered across many platforms and group chats. Each platform has its own filters and alerts, but none of them looks across platforms, and none of them knows your situation: your year, your branch, your college's exam weeks, or what you have already applied to. The result is missed deadlines and time wasted on events you cannot enter.

## What this project does

- Pulls listings from more than one platform on a schedule and merges duplicates
- Shows an **eligibility verdict with a reason** for each listing (Eligible / Not eligible / Unclear)
- Lets you save listings and get reminders only for those
- Warns you when deadlines clash with each other or with your exam dates

## Features

Planned features are listed here and tracked in the [Roadmap](#roadmap).

**Core**
- [ ] Collect listings from a first source and store them in a database
- [ ] Add a second source and remove duplicate listings
- [ ] Filter by type, mode (online/offline) and deadline
- [ ] Save listings
- [ ] Scheduled updates, with a "last updated" time on every listing

**Signature**
- [ ] Per-user eligibility verdict with a reason
- [ ] Deadline clash detection (exam dates and other saved deadlines)
- [ ] Telegram bot with reminders at 7 days, 2 days and 6 hours before a deadline

**Maybe later**
- [ ] Flags for paid events or unverified organizers (heuristic only)
- [ ] Application tracker (saved → registered → submitted → result)
- [ ] Weekly "top 5 for you" digest

## How it works

```
Sources (platform listings)
        │  scheduled collection
        ▼
  Collector (Python)  ──►  Database
                              │
                              ▼
                    API (FastAPI) ──► Web app (React)
                              │
                              └────► Telegram bot (reminders)
```

1. **Collect:** a scheduled job fetches listings. It prefers an official API or the JSON a page already loads, and falls back to parsing HTML only if needed.
2. **Normalise:** each listing is converted to one common format (title, type, mode, deadline, eligibility text, link, source).
3. **Decide:** eligibility rules compare a listing to the user's profile and return a verdict with a reason. If the text is ambiguous, the verdict is "Unclear" rather than a guess.
4. **Serve and notify:** the API feeds the web app and the Telegram bot.

## Tech stack

| Layer | Choice |
|---|---|
| Collection | Python (requests, BeautifulSoup; Playwright only if a source needs it) |
| Backend | FastAPI |
| Database | SQLite first, PostgreSQL if needed |
| Frontend | React |
| Alerts | Telegram bot |
| Deployment | To be decided (Docker + a free cloud tier is the goal) |

## Project structure (planned)

```
.
├── backend/        # API, database models, eligibility logic
├── collector/      # scrapers / API clients, one module per source
├── frontend/       # React app
├── bot/            # Telegram bot
└── README.md
```

## Getting started

Setup steps will be added as each part is built. The intended flow is:

```bash
# backend
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload

# frontend
cd frontend
npm install
npm run dev
```

Configuration (API keys, bot token) will go in a `.env` file. A `.env.example` will be provided and real secrets will never be committed.

## Roadmap

- [ ] **Week 1:** collect listings from one source into a database
- [ ] **Week 2:** API with filtering and the first version of eligibility rules
- [ ] **Week 3:** React frontend, scheduled updates, one alert channel, deployment
- [ ] **After:** add a second source, then explore an LLM step for extracting details from messy descriptions, security hardening, and containerised deployment

## Limitations and responsible use

- **Data can be wrong or late.** Listings come from third-party pages that can change without notice. Always confirm details and deadlines on the official page before applying.
- **Eligibility verdicts are automated.** They can be incorrect, which is why uncertain cases show "Unclear". Accuracy will be measured on a test set and reported here once available.
- **Respect the sources.** Collection should follow each site's terms of service and robots.txt, use official APIs where they exist, and keep request rates low.
- This project is independent and **not affiliated with or endorsed by** any platform it collects from.

## Contributing

This is a personal learning project and is not yet open for contributions. Ideas and bug reports are welcome as issues.

## License

To be decided. A `LICENSE` file will be added.

## Author

Built by **[Your Name]** as a hands-on project to learn backend development, data collection and deployment.
