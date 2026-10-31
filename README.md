# Mauka: Hackathons and internships that fit you

> Find opportunities. See your fit. Stay ahead of deadlines.

Mauka is a student-focused web app that brings hackathons, contests and internships from multiple platforms into one feed. It helps each student understand whether an opportunity fits their profile, keep track of deadlines, and receive reminders for opportunities they have saved.

**Status:** 🚧 In development. Features are planned unless their roadmap checkbox is marked as complete. No feature or performance metric is claimed as finished until it has been implemented and tested.

---

## The problem

Opportunities for students are scattered across different platforms and group chats. Each platform has its own filters and alerts, but students still have to search across multiple sources and manually check whether they meet each opportunity's requirements.

Most platforms also do not know a student's college exam dates or which opportunities they have already saved. This can lead to missed deadlines and time spent reviewing opportunities for which a student may not be eligible.

## What Mauka aims to do

- Collect listings from multiple opportunity platforms into one feed.
- Normalize listings and identify duplicates.
- Show a per-student eligibility verdict with a reason: **Eligible**, **Not eligible** or **Unclear**.
- Let students save opportunities and receive reminders for those saved listings.
- Detect deadline clashes between saved opportunities and a student's exam dates.

## Features

The checkboxes below represent the current plan. They should be checked only after the feature is implemented and tested.

### Core features

- [ ] Collect listings from a first source and store them in a database.
- [ ] Add a second source and identify duplicate listings.
- [ ] Filter opportunities by type, mode (online/offline) and deadline.
- [ ] Save opportunities.
- [ ] Refresh listings on a schedule and show a last-updated timestamp.

### Signature features

- [ ] Show a per-user eligibility verdict with a clear reason.
- [ ] Detect deadline clashes with exam dates and other saved deadlines.
- [ ] Send Telegram reminders for saved opportunities at 7 days, 2 days and 6 hours before the deadline.

### Possible later features

- [ ] Flag listings that appear to have registration fees or an unverified organizer. These flags will be presented as heuristics, not confirmed facts.
- [ ] Track an application through stages: saved → registered → submitted → result.
- [ ] Send a weekly digest of relevant opportunities.

## How it works

```text
Opportunity platforms
        |
        | scheduled collection
        v
Python collector  ----->  SQLite database
                                |
                                v
                     Java backend (Spring Boot)
                         /              \
                        v                v
                React web app      Telegram alerts
```

1. **Collect:** The Python collector retrieves opportunity information from a permitted source. It prefers an official API or an openly accessible structured data endpoint. HTML parsing is considered only when permitted and necessary.
2. **Normalize:** Each listing is converted into a common format, such as title, type, mode, deadline, eligibility text, official link and source platform.
3. **Store:** The normalized records are saved in the database. Duplicate detection will be added as more than one source is integrated.
4. **Serve:** The Java Spring Boot backend exposes REST APIs for the React frontend and handles application logic such as filtering, saved listings and eligibility checks.
5. **Notify:** Telegram integration sends reminders for opportunities a student has saved.

If eligibility information is ambiguous or incomplete, Mauka should return **Unclear** rather than make an unsupported guess.

## Tech stack

| Layer | Technology | Responsibility |
|---|---|---|
| Data collection | Python (`requests`; BeautifulSoup only if needed) | Retrieve and normalize opportunity listings |
| Backend | Java + Spring Boot | REST APIs, application logic and eligibility rules |
| Database | SQLite initially; PostgreSQL may be considered later | Store opportunities and user-related data |
| Database access | Spring Data JPA / JDBC | Let the Java backend work with the database |
| Frontend | React | Opportunity feed, filters and student dashboard |
| Alerts | Telegram Bot API | Reminders for saved opportunities |
| Version control | Git and GitHub | Track and manage project code |
| Deployment | To be decided | Deployment will be planned after the first working version |

Python is used for the initial collector; Java Spring Boot is the main application backend. They are separate components with separate responsibilities.

## Planned project structure

```text
Mauka/
├── backend/                 # Java Spring Boot application
├── collector/               # Python collectors and data normalization
├── frontend/                # React application
├── data/                    # Local SQLite database during development
├── docs/                    # Project notes and documentation
├── .gitignore
└── README.md
```

The structure may grow as features are implemented. Generated environments, build output, database files containing local data and secret configuration must not be committed to GitHub.

## Getting started

Mauka is being developed in stages. Setup instructions will be expanded as each component is built. The first development milestone is the Python collector and local database.

### Prerequisites for Week 1

- Windows 11 (commands below use PowerShell).
- Python installed and available through `py` or `python`.
- Git (for version control; it can be initialized after the initial setup).

### Set up the collector environment on Windows

From the Mauka project root, run:

```powershell
cd collector
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

If the `py` command is unavailable, check the Python installation and PATH before continuing. If PowerShell blocks environment activation, use the process-scoped execution-policy workaround documented in the development notes rather than changing the machine-wide policy.

The collector's dependency list will be recorded in `requirements.txt` as the collector is implemented. Install those dependencies with:

```powershell
pip install -r requirements.txt
```

### Backend and frontend setup

The Java backend and React frontend will receive their setup instructions when those components are created. The planned backend uses a supported JDK, Spring Boot and Maven. The frontend uses Node.js and npm.

## Roadmap

- [ ] **Week 1 — First data source:** inspect the permitted Unstop data access method, collect listings, normalize the records and store them in SQLite. Verify that records can be retrieved.
- [ ] **Week 2 — Java backend:** create the Spring Boot application, connect it to the database, expose opportunity APIs, and implement initial filters and rule-based eligibility checks.
- [ ] **Week 3 — React frontend:** build the opportunity feed and connect it to the Java APIs. Add scheduled collection and the first usable alert flow as the integration allows.
- [ ] **After the first working version:** add a second source and deduplication, then improve eligibility extraction, deadline clash detection, Telegram reminders, testing, security and deployment.

The roadmap is a learning plan, not a claim that these features are already available.

## Limitations and responsible use

- **Listings can be outdated or incomplete.** Source pages may change, and collection may not capture every update. Students should confirm details and deadlines on the official opportunity page before applying.
- **Eligibility verdicts can be wrong.** Automated checks will be tested against a manually reviewed sample. Ambiguous cases should be marked **Unclear**. Accuracy figures will be published only after testing.
- **Collection must respect source rules.** Prefer official APIs and permitted public data. Check each platform's terms and applicable access rules, respect robots.txt as one signal, avoid authentication or access-control workarounds, and keep request rates low. Robots.txt alone does not grant permission to collect data.
- **Heuristic flags are not guarantees.** Any paid-event or organizer-trust indicators added later will be labelled as signals that require user verification.
- **Independent project.** Mauka is not affiliated with, endorsed by or operated by Unstop or any other platform listed by the service.

## Contributing

Mauka is currently a personal learning project and is not yet open for outside contributions. Suggestions and bug reports are welcome through GitHub issues.

## License

To be decided. A license will be added before the project is distributed for reuse.

## Author

Built by **Misari** as a hands-on project to learn Java backend development, Python-based data collection, databases and frontend integration.
