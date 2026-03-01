# ReleaseRun Stack Health Check

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-ReleaseRun-blue?logo=github)](https://github.com/marketplace/actions/releaserun-stack-health-check)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Scan your project for end-of-life dependencies, known CVEs, and version health issues. Get an A-F grade for your entire tech stack in CI.

## Quick Start

```yaml
- uses: Releaserun/releaserun-action@v1
```

That's it. Add it to any workflow and it'll scan your project, post a health report as a PR comment, and optionally fail the build if your stack health drops below a threshold.

## What It Checks

- **package.json** / **package-lock.json** (Node.js, npm)
- **requirements.txt** / **Pipfile** (Python)
- **go.mod** (Go)
- **Gemfile** / **Gemfile.lock** (Ruby)
- **pom.xml** (Java/Maven)
- **Dockerfile** (Docker base images)
- **docker-compose.yml** (Service versions)
- **.terraform.lock.hcl** (Terraform providers)

For each detected technology, it checks:
- Current version vs latest
- End-of-life status and dates
- Known CVEs from OSV database
- Overall health grade (A through F)

## Usage

### Basic (scan and comment on PRs)

```yaml
name: Stack Health
on: [pull_request]

jobs:
  health-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Releaserun/releaserun-action@v1
```

### Fail on low grades

```yaml
- uses: Releaserun/releaserun-action@v1
  with:
    fail-on: 'D'  # Fail if any tech grades D or F
```

### Scan a specific directory

```yaml
- uses: Releaserun/releaserun-action@v1
  with:
    path: './backend'
```

### JSON output for scripting

```yaml
- uses: Releaserun/releaserun-action@v1
  id: health
  with:
    format: 'json'
    comment: 'false'

- run: echo "Grade is ${{ steps.health.outputs.grade }}"
```

### Scheduled checks

```yaml
name: Weekly Stack Health
on:
  schedule:
    - cron: '0 9 * * 1'  # Every Monday at 9am

jobs:
  health-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Releaserun/releaserun-action@v1
        with:
          comment: 'false'
          fail-on: 'D'
```

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `path` | `.` | Directory to scan |
| `fail-on` | `F` | Fail if grade is at or below this (A/B/C/D/F/none) |
| `format` | `table` | Output format: `table` or `json` |
| `comment` | `true` | Post results as PR comment |
| `badge` | `true` | Include health badges in PR comment |

## Outputs

| Output | Description |
|--------|-------------|
| `grade` | Overall stack health grade (A-F) |
| `technologies` | Number of technologies detected |
| `eol-count` | Number of EOL technologies |
| `cve-count` | Total CVE count |
| `report` | Full JSON report |

## PR Comment Example

The action posts a formatted table as a PR comment:

> ## 🟡 Stack Health Report — Grade: **B**
>
> | Metric | Value |
> |--------|-------|
> | Technologies scanned | 5 |
> | EOL dependencies | 1 |
> | Known CVEs | 3 |
> | Overall grade | B |
>
> ### Details
>
> | Technology | Version | EOL | CVEs | Grade |
> |------------|---------|-----|------|-------|
> | Node.js | 20.11.0 | 2026-04-30 | 0 | A |
> | Python | 3.11.7 | 2027-10-24 | 1 | B |
> | PostgreSQL | 15.4 | 2027-11-11 | 2 | B |
> | Docker | 25.0 | Active | 0 | A |
> | Kubernetes | 1.28 | 2025-10-28 | 0 | C |

## Links

- [ReleaseRun Tools](https://releaserun.com/tools/) — 30+ free developer tools
- [CLI on npm](https://www.npmjs.com/package/releaserun) — Run locally without the Action
- [Badge Generator](https://releaserun.com/tools/badge-generator/) — SVG badges for your README
- [Badge API Docs](https://releaserun.github.io/badges) — Full badge API reference with code examples
- [Stack Health Scorecard](https://releaserun.com/tools/stack-health/) — Interactive web version
- [EOL Timeline](https://releaserun.com/tools/eol-timeline/) — Visual EOL tracker

## License

MIT
