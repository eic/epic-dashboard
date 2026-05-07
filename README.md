# EIC GitHub Dashboard

A lightweight GitHub Pages dashboard tracking issue, pull-request, and commit
activity for key [EIC](https://www.eicug.org/) software repositories.

## Tracked repositories

| Repository | Description |
|---|---|
| [eic/epic](https://github.com/eic/epic) | EIC detector simulation |
| [eic/EICrecon](https://github.com/eic/EICrecon) | EIC reconstruction framework |
| [eic/containers](https://github.com/eic/containers) | Software container images |
| [eic/detector_benchmarks](https://github.com/eic/detector_benchmarks) | Detector performance benchmarks |
| [eic/physics_benchmarks](https://github.com/eic/physics_benchmarks) | Physics performance benchmarks |

## Dashboard features

- **Stat cards** — current open issues, open PRs, and commits this week per repo
- **Issues over time** — weekly snapshot of open issue count (past 26 weeks)
- **Pull requests over time** — weekly snapshot of open PR count (past 26 weeks)
- **Commits per week** — bar chart of weekly commit activity (past 26 weeks)

## How it works

A [GitHub Actions workflow](.github/workflows/update-data.yml) runs every
Monday at 06:00 UTC (and on every push that touches the script or workflow).
It executes `scripts/fetch_data.py`, which:

1. Uses the **GitHub GraphQL API** to batch-fetch weekly open issue/PR counts,
   reconstructing historical snapshots via `created` and `closed` date filters.
2. Uses the **GitHub REST stats API** (`/repos/{owner}/{repo}/stats/commit_activity`)
   to retrieve the last 52 weeks of commit activity per repository.

The results are written to `data/dashboard.json` and committed back to `main`,
which GitHub Pages serves directly.

## Local development

No build step is required — the dashboard is plain HTML + Chart.js loaded from a CDN.

```bash
# Serve locally (any static file server works)
python -m http.server 8080
# then open http://localhost:8080
```

To regenerate `data/dashboard.json` locally:

```bash
export GITHUB_TOKEN=ghp_...   # personal access token with public repo read scope
python scripts/fetch_data.py
```

## Manual data refresh

Trigger the workflow at any time from the **Actions** tab of this repository
using the *workflow_dispatch* event, or push a change to `scripts/fetch_data.py`.
