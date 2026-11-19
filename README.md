# TenderLiveAI – AI-Powered Tender Compliance & Risk Intelligence Platform

TenderLiveAI is a web-based procurement intelligence prototype designed to help users review government tenders, organize bidder information, identify potential compliance gaps, and maintain a traceable review workflow.

The project brings together tender discovery, bidder compliance review, risk indicators, discrepancy tracking, and audit-oriented reporting in a single interface. It includes scraper services for India's Government e-Marketplace (GeM) and Central Public Procurement Portal (CPPP).

> **Project status:** Prototype / demonstration application. The repository includes sample tender and bidder records, generated verification examples, and fallback scraper data. Do not treat displayed verification results, risk scores, bidder identities, or audit entries as authoritative government records unless independently validated against the relevant official source.

---
<img width="1893" height="910" alt="image" src="https://github.com/user-attachments/assets/868a741b-526d-49ff-969a-a523602b544b" />

## Contents

- [Problem Statement](#problem-statement)
- [Solution](#solution)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Application Workflow](#application-workflow)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [Running the Application](#running-the-application)
- [Tender Scraper API](#tender-scraper-api)
- [Tender Data Sources](#tender-data-sources)
- [Important Implementation Notes](#important-implementation-notes)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [Disclaimer](#disclaimer)

---

## Problem Statement

Government procurement involves reviewing tender conditions, bidder documents, eligibility requirements, statutory registrations, financial information, and technical qualifications. When this information is spread across multiple documents and portals, the review process can become time-consuming and difficult to track.

TenderLiveAI explores how a centralized digital workflow could support procurement teams by organizing tender information, highlighting potential discrepancies, presenting compliance indicators, and recording review actions.

## Solution

TenderLiveAI provides a centralized interface for:

1. Viewing and searching tender records.
2. Ingesting tender information from GeM and CPPP scraper services.
3. Converting scraped tender records into the application's verification-workbench data structure.
4. Reviewing bidder information, requirements, and sample cross-verification results.
5. Highlighting potential discrepancies for officer review.
6. Recording decisions and review activity in an audit-trail interface.
7. Viewing and printing procurement evaluation report layouts.

The platform is intended as a decision-support prototype. Final procurement decisions must remain with authorized officials and rely on verified source documents and official registry responses.

## Key Features

### 1. Tender Management
- Tender registry and dashboard views.
- Search and filter tender records.
- Tender details including department, location, estimated value, deadline, and source.
- Option to upload tender information through the application interface.
- Entry point to run the GeM and CPPP scraper workflow.

### 2. GeM and CPPP Tender Scraping
- Separate scraper services for GeM BidPlus and CPPP.
- Attempts to retrieve public tender listing pages.
- GeM scraper attempts to extract a CSRF token and query the `all-bids-data` endpoint.
- CPPP scraper parses tender listing tables and attempts to create stable tender detail links.
- Scraper logs report progress, warnings, and errors.
- Fallback records are used when live portal access or parsing is unsuccessful.

### 3. Verification Workbench
- Structured tender requirements grouped by category.
- Bidder profiles and compliance indicators.
- Cross-verification presentation for sources such as GSTN, Udyam, and MCA21.
- Discrepancy review with severity, supporting reference, confidence indicator, and suggested action.
- Officer decision workflow, including approval, rejection, or clarification paths in the interface.

### 4. Reports
- Procurement evaluation report view.
- Print-friendly report layout using the browser's print functionality.

### 5. Audit Trail
- Review activity displayed in an audit-trail interface.
- Search and category filtering.
- Copyable hash values and JSON export of the displayed audit entries.

### 6. User Interface
- Landing page explaining the platform.
- Dashboard, tender registry, verification workflow, reports, and audit-trail screens.
- Responsive interface built with React and Tailwind CSS.

---

## Technology Stack

| Area | Technologies |
|---|---|
| Frontend | React 19, TypeScript |
| Build tool | Vite 6 |
| Styling | Tailwind CSS 4 |
| UI and icons | Lucide React |
| Charts | Recharts |
| Animations | Motion |
| Backend | Node.js, Express |
| TypeScript runtime | tsx |
| Scraping and HTML parsing | Fetch API, Cheerio |
| Python scraper implementation | Python, BeautifulSoup, curl_cffi |
| Build bundling | esbuild |

---

## Application Workflow

```text
                  TenderLiveAI
                       |
          +------------+------------+
          |                         |
    Tender Registry           Scraper Services
          |                    /           \
    Search / Filter           GeM           CPPP
          |                    \           /
          |                     Tender Data
          |                          |
          +-------------+------------+
                        |
              Tender Conversion
                        |
               Verification Workbench
                        |
        +---------------+----------------+
        |               |                |
   Requirements    Bidder Review    Discrepancies
        |               |                |
        +---------------+----------------+
                        |
              Officer Review Workflow
                        |
               Reports & Audit Trail
```

---

## Project Structure

```text
TenderLiveAI-main/
│
├── public/
│   └── assets/                 # Public images and static assets
│
├── scrapers/                   # Python scraper implementation
│   ├── __init__.py
│   ├── base.py
│   ├── cppp_scraper.py
│   ├── gem_scraper.py
│   └── run_scrapers.py
│
├── server/
│   ├── scrapers.ts             # TypeScript scraper services
│   └── scrapers/
│       ├── cpppScraper.ts
│       ├── gemScraper.ts
│       └── tenderConverter.ts
│
├── src/
│   ├── components/             # React screens and UI components
│   ├── data/
│   │   └── tendersData.ts      # Initial/sample tender data
│   ├── services/
│   │   └── scraperApi.ts       # Frontend API client and fallback
│   ├── assets/
│   ├── App.tsx
│   ├── index.css
│   ├── main.tsx
│   └── types.ts                # Shared TypeScript data models
│
├── .env.example
├── index.html
├── package.json
├── server.ts                   # Express API and Vite development server
├── tsconfig.json
└── vite.config.ts
```

---

## Getting Started

### Prerequisites

Install the following before running the project:

- **Node.js:** A current LTS release is recommended.
- **npm:** Included with Node.js.
- **Git:** Optional, for cloning the repository.

Check your installed versions:

```bash
node --version
npm --version
```

### 1. Clone the repository

```bash
git clone https://github.com/Aashi1102/TenderLiveAI
cd TenderLiveAI-main
```

If you have downloaded the project as a ZIP file, extract it and open the extracted project directory in your terminal or code editor.

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Copy the example environment file:

**Windows (Command Prompt):**
```cmd
copy .env.example .env
```

**macOS / Linux:**
```bash
cp .env.example .env
```

The example file contains a `GEMINI_API_KEY` placeholder. The current source code does not reference this variable, so a Gemini API key is not required for the documented local startup and scraper workflow. Do not add real secrets to source control.

### 4. Start the development server

```bash
npm run dev
```

Open the URL printed by the development server in your browser. The Express server is configured to use port **3000**.

---

## Running the Application

### Development

```bash
npm run dev
```

This starts the Express server and integrates the Vite development server for the frontend.

### Production build

```bash
npm run build
```

The build command:
1. Builds the frontend using Vite.
2. Bundles `server.ts` with esbuild into `dist/server.cjs`.

### Start the production server

After a successful build:

```bash
npm start
```

### TypeScript check

```bash
npm run lint
```

This runs the TypeScript compiler in no-emit mode.

### Preview the frontend build

```bash
npm run preview
```

This runs Vite's preview server. It is intended for previewing the frontend build; use `npm start` to start the bundled Express application and its API routes.

---

## Tender Scraper API

The Express backend exposes the following endpoints.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Returns service health information. |
| `GET` | `/api/scrapers/status` | Returns scraper portal metadata and supported-feature information. |
| `POST` | `/api/scrapers/run` | Runs the selected GeM and/or CPPP scraper. |
| `POST` | `/api/scrapers/convert-to-tender` | Converts a scraped tender payload into the workbench tender structure. |

### 1. Health check

```http
GET /api/health
```

Example response:

```json
{
  "status": "ok",
  "service": "Tender Live Scraper Engine",
  "time": "2026-11-19T10:00:00.000Z"
}
```

The timestamp above is illustrative.

### 2. Check scraper status

```http
GET /api/scrapers/status
```

Returns configured portal metadata, candidate endpoints, and scraper feature descriptions.

### 3. Run a scraper

```http
POST /api/scrapers/run
Content-Type: application/json
```

Request body:

```json
{
  "source": "all",
  "limit": 10
}
```

Supported `source` values:

- `gem` – GeM scraper only.
- `cppp` – CPPP scraper only.
- `all` – both scraper services.

The `limit` value is clamped by the backend to a range of 1–30 tenders per selected source.

The response includes:
- `success`
- `source`
- `count`
- `tenders`
- `logs`
- `durationMs`
- `status` (`live`, `fallback`, or `partial`)

Example using cURL:

```bash
curl -X POST http://localhost:3000/api/scrapers/run \
  -H "Content-Type: application/json" \
  -d '{"source":"gem","limit":10}'
```

### 4. Convert a scraped tender

```http
POST /api/scrapers/convert-to-tender
Content-Type: application/json
```

Request body:

```json
{
  "scraped": {
    "tender_id": "EXAMPLE-TENDER-ID",
    "title": "Example tender title",
    "department": "Example Department",
    "location": "New Delhi",
    "budget": null,
    "deadline": null,
    "eligibility_criteria": "Example eligibility details",
    "source_url": "https://example.gov.in/tender",
    "source_name": "GeM"
  }
}
```

This is an illustrative payload. The endpoint expects a scraped tender object with a `tender_id` and returns a converted tender structure.

---

## Tender Data Sources

### Government e-Marketplace (GeM)

The TypeScript GeM scraper targets public GeM BidPlus listing pages, including:

- `https://bidplus.gem.gov.in/all-bids`
- `https://bidplus.gem.gov.in/bidlists`

When the listing page exposes the expected token, the scraper attempts to submit a request to the GeM `all-bids-data` endpoint and parse the returned records.

### Central Public Procurement Portal (CPPP)

The TypeScript CPPP scraper checks public CPPP listing pages, including:

- `https://eprocure.gov.in/cppp/latestactivetendersnew`
- `https://eprocure.gov.in/cppp/latestactivetenders`
- `https://eprocure.gov.in/cppp/tendersclosingbydays/bytoday`

It attempts to parse tender listing tables and derive stable tender detail links where possible.

**Important:** These portals may change their HTML, APIs, access controls, or request requirements. Network access may also be restricted by CAPTCHA, firewall, rate limiting, or hosting environment. Consequently, live scraping is not guaranteed. The application includes fallback behavior; check the scraper response status and logs to distinguish live results from fallback results.

---

## Optional: Running the Python Scrapers

The repository also contains a separate Python scraper implementation under `scrapers/`. It is not the command used by the Express `/api/scrapers/run` endpoint, which calls the TypeScript scraper services.

To try the Python CLI separately, install its Python dependencies:

```bash
python -m pip install beautifulsoup4 curl_cffi
```

Run both sources:

```bash
python scrapers/run_scrapers.py --source all --limit 10
```

Run one source:

```bash
python scrapers/run_scrapers.py --source gem --limit 10
python scrapers/run_scrapers.py --source cppp --limit 10
```

The Python runner prints a JSON result to standard output. Depending on portal availability and the current scraper implementation, it may return empty results or encounter access/parsing errors.

---

## Important Implementation Notes

Please read these notes before presenting the prototype as a production procurement or compliance system:

1. **Sample and generated data:** The repository contains predefined tender data and code that generates example bidder profiles, requirements, scores, and cross-verification entries. These are demonstration records, not proof of actual bidder compliance.
2. **Registry verification:** The displayed GSTN, Udyam, and MCA21 verification examples should not be described as confirmed live integrations. The current code includes illustrative verification results; production registry access would require authorized APIs, credentials, and validation.
3. **Risk scoring:** The current risk indicators are generated from example bidder scores and rules in the code. They are not a validated procurement risk model.
4. **Audit trail:** The interface supports audit-style activity records and hash display/export. The current implementation should not be represented as a production-grade immutable ledger or tamper-proof audit infrastructure.
5. **Document intelligence:** The interface models document and clause review workflows. A complete production OCR, PDF parsing, semantic extraction, and evidence-grounded AI verification pipeline is not established by the current implementation.
6. **Authentication and authorization:** A login interface is present, but production-grade identity management, role-based access control, and secure session handling should be implemented and tested before deployment.
7. **Persistence:** Do not assume that application records are stored in a production database. A persistent backend and data-retention strategy should be added for real deployments.
8. **Scraper reliability:** Live scraping depends on the availability and structure of external government portals. Always inspect the returned status and logs.

---

## Future Enhancements

Potential next steps for evolving the prototype include:

- Integrate authorized official APIs for GSTN, MCA, Udyam, and other registries.
- Add secure authentication and role-based access for procurement officers and administrators.
- Implement persistent storage for tenders, bidders, documents, decisions, and audit events.
- Add PDF/DOCX ingestion, OCR, clause extraction, and source-page citations.
- Build an evidence-grounded AI compliance engine that maps each tender clause to bidder-submitted evidence.
- Develop explainable risk scoring with configurable rules and validation against reviewed cases.
- Implement cryptographically verifiable, append-only audit logging.
- Add automated tests, scraper fixtures, error monitoring, and API validation.
- Improve scraper resilience with change detection, retries, and portal-specific parsing tests.
- Deploy the application with secure secrets management, monitoring, and backups.

## Contributing

Contributions, bug reports, and feature suggestions are welcome.

1. Fork the repository.
2. Create a feature branch:

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. Make your changes and run the available checks:

   ```bash
   npm run lint
   npm run build
   ```

4. Commit your changes:

   ```bash
   git commit -m "Add: your feature description"
   ```

5. Push the branch and open a pull request.

For scraper changes, include representative HTML/JSON fixtures where possible so parsing behavior can be tested without relying on live government portals.

## Disclaimer

TenderLiveAI is an independent prototype for learning, demonstration, and exploration of procurement-compliance workflows. It is not an official Government of India, GeM, or CPPP product and is not affiliated with or endorsed by those portals. Portal names and references are used to identify intended public data sources.

Always verify tender details, bidder credentials, statutory status, eligibility, and compliance findings directly through official sources and authorized processes. The application must not be used as the sole basis for procurement, eligibility, or disqualification decisions.

---

**Built as a procurement intelligence prototype.**
