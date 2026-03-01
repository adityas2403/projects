# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this Projects directory.

## Top-Level Structure

```
Projects/
├── quant-research/          # Quantitative finance domain
│   ├── core/                # Shared infrastructure (TypeScript + Python libs)
│   ├── data/                # Domain-specific datasets
│   │   ├── raw/
│   │   └── processed/
│   └── <project>/           # Individual research projects (e.g. yield-curve, vol-surface)
├── cricket/                 # Cricket analytics domain
├── finance-research/        # General finance research domain
├── jobs/                    # Scheduled/automated tasks (PowerShell, cron)
└── scratch/                 # Throwaway experiments
```

## quant-research/core

Polyglot quant library with TypeScript and Python layers.

### Commands

#### TypeScript/JavaScript

```bash
npm install                  # Install dependencies
npm run build                # Compile TypeScript
npm run typecheck            # Type check without emitting
npm run lint                 # ESLint
npm run lint:fix             # Auto-fix ESLint issues
npm run format               # Prettier format
npm run format:check         # Check formatting
npm run test                 # Run Vitest tests
npm run test:watch           # Watch mode
npm run test:coverage        # With coverage
```

Run a single test file: `npx vitest run path/to/test.spec.ts`

#### Python (uses `uv`)

```bash
uv sync --all-packages --dev  # Install all dependencies
uv run pytest -v              # Run tests
uv run pytest path/to/test.py # Run a single test file
uv run pytest -k "test_name"  # Run tests matching pattern
uv run ruff check .           # Lint
uv run ruff format .          # Format
uv run jupyter lab            # Launch Jupyter Lab
```

### Architecture

- `packages/core/` — NPM workspace package targeting Node 24+, ES2022
- `python/research/` — `uv` workspace package requiring Python 3.12+
- `python/notebooks/` — Jupyter research notebooks (outputs stripped before commit)
- Key Python deps: `pandas>=2.2`, `numpy>=2.0`, `scikit-learn>=1.5`, `scipy>=1.13`, `statsmodels>=0.14`, `matplotlib>=3.9`
- Ruff (v0.9+) replaces black/isort/flake8

## Code Standards

- Line length: **100 characters** (both Python and TypeScript)
- Python: type annotations required (Ruff `ANN` rules); exempted in notebooks and tests
- TypeScript: strict mode, no unchecked index access, prefer type imports
- Formatting: 2-space indent (TS), 4-space indent (Python), LF line endings

## CI (quant-research/core)

GitHub Actions runs on push to `main`/`develop` and PRs to `main`:
1. **JS/TS job**: typecheck → lint → format:check → test:coverage
2. **Python job**: ruff check → ruff format --check → pytest --cov
