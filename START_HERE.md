# NetraDrift — complete source handoff

This archive contains every tracked file of the implemented website, unchanged, plus this guide, a local database configuration, a seed-only SQLite database, and a source manifest. Start here before the original README.

## 1. Run on your computer

Install Node.js 22.13 or later (Node 22 LTS is a suitable baseline) and VS Code. Extract the ZIP. Open the inner `NetraDrift` folder in VS Code; it contains `package.json`. Open Terminal > New Terminal and run these commands one at a time:

```sh
node --version
npm run install:ci
npx wrangler d1 migrations apply site-creator-d1 --local --config wrangler.local.json --persist-to .wrangler/state
npm run dev
```

Accept Wrangler's local migration prompt if shown. The migration command initializes only the local database. Open the localhost address printed in the terminal, normally http://localhost:5173. Keep the terminal running; Ctrl+C stops it. On subsequent visits, use `npm run dev`; installation and initial migration need not be repeated.

On Windows, if PowerShell blocks `npm.ps1`, choose Command Prompt as the VS Code terminal, or use `npm.cmd` and `npx.cmd`. You do not need to change your computer's execution policy.

The public demo is at `/`. The real-data workspace is at `/research`. On localhost, the sign-in button uses the included development identity adapter; it does not require a real ChatGPT login. The original README contains earlier private-site guidance; the current public demo and protected research workspace are described in its later sections.

Internet access is needed to install dependencies and load online map tiles. Camera and location require browser permission and a compatible device. A localhost browser supports development access to these APIs; a deployed version needs HTTPS. No external LLM API key is needed for the implemented application.

## 2. What is included

| Location | Contents |
|---|---|
| `app/` | Pages, API routes, authentication integration, global styles |
| `components/`, `hooks/` | Screens, reusable interface components, interactions and animation |
| `lib/` | Data processing, scoring, demo logic and shared helpers |
| `analysis/` | Python Isolation Forest training, dependencies and model card |
| `data/source/` | All five original uploaded CSV datasets |
| `data/` | Prepared analysis data and exported model used for inference |
| `db/`, `drizzle/` | Database schema, migrations and seed records |
| `database-seed.sqlite` | Portable SQLite snapshot built from the migrations; no live user records |
| `public/`, `vendor/` | Included visual assets, styles and licenses |
| `build/` | Required source code for the Sites/Vite integration, not disposable build output |
| `scripts/`, `examples/` | Installation, build, preparation and verification tools and examples |
| `package.json`, `package-lock.json` | Dependency definitions and exact dependency lockfile |
| `.openai/hosting.json` | Original hosting configuration; a project identifier is not a credential |
| `wrangler.local.json` | Added local-only D1 configuration for initializing the development database |
| `SOURCE_MANIFEST.json` | Source commit and SHA-256 hashes of all original tracked files |
| `README.md`, `DESIGN.md`, `UX-CONTRACT.md` | Original project documentation |

The SQLite snapshot is for inspection with a SQLite viewer. The app uses the local D1 database created by the migration command, not this snapshot file directly.

## 3. Features and implementation boundaries

Included: animated demo network, map, peer comparisons, investigation screens, cost/delay/duplicate review, timelines, MP/field/citizen demo personas, camera/location capture, evidence hashing, saved review decisions and tasks, and real-data analysis. The public demo uses synthetic project records; the research workspace uses the supplied data.

The trained Isolation Forest model and training source are included. Alerts prioritize human review; they are not confirmed fraud findings. The original CSVs do not supply all quantities, coordinates or scheduled completion dates needed to validate every demonstration detector on real projects.

LLM/RAG integration remains planned. The existing method assistance is deterministic, not an external language model. Demo personas do not constitute production government role authentication. Browser GPS and evidence hashes do not independently prove that construction took place.

## 4. Reproduce the data/model work (optional)

Running the website does not require retraining: prepared data and model are already included. To retrain, install Python 3.11 or 3.12, create a virtual environment, and activate it:

```sh
python -m venv .venv
```

Windows Command Prompt: `.venv\Scripts\activate.bat`

macOS/Linux: `source .venv/bin/activate`

Then run from the project root:

```sh
python -m pip install -r analysis/requirements.txt
node scripts/prepare-data.mjs
python analysis/train.py
node scripts/verify-data.mjs
python scripts/verify-database.py
```

The training uses a fixed random seed. See `analysis/model-card.md` and the original README for feature definitions and limitations. There are no confirmed fraud labels, so this is not an accuracy-certified fraud classifier. Rebuilding the analysis files does not automatically update an existing database. For a fresh disposable database, the seed-generation script is `node scripts/seed-db.mjs`; do not rewrite already-applied migrations for a database you need to preserve.

## 5. Build and verify

```sh
npx tsc --noEmit
node scripts/verify-demo.cjs
npm run build
```

The demo verification script uses Node's SQLite support. The source package was checked against all tracked source files, and the supplied seed database was checked for integrity. A fresh installation on your particular operating system and real-device camera/GPS behavior still require local verification.

## 6. Source code versus hosting and saved data

The archive includes the complete implemented project source, not just the visible HTML. Third-party `node_modules`, generated build output, caches, Git history, and private credentials are intentionally excluded. `npm run install:ci` restores dependencies from the included lockfile; `npm run build` regenerates build output.

Live visitor uploads, photos, session records, and review decisions are stored separately on the hosting service and are not copied into this source archive. The included migrations and seed data let you initialize your own development instance.

The original deployment uses Sites/Cloudflare Workers and a D1 database. Its project identifier does not grant hosting access or provision infrastructure. Deploying under your own account requires configuring that provider's database and authentication. The production authentication integration expects trusted platform-provided identity headers; do not expose a standalone deployment that trusts user-supplied identity headers. Development mock sign-in is intended only for localhost.

Changes in this extracted folder do not change the live website until you separately deploy them. PPTs and promotional videos are separate deliverables, not website runtime files.
