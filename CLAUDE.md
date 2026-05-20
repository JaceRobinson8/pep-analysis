# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Exploratory analysis of all Python Enhancement Proposals (PEPs) — timeline, influence scoring, and narrative of how Python has evolved over time.

## Setup

```bash
pip install -r requirements.txt
jupyter notebook pep_analysis.ipynb
```

## Architecture

Everything lives in a single Jupyter notebook (`pep_analysis.ipynb`) with five sections:

1. **Data fetch** — pulls all `pep-NNNN.rst` files from the `python/peps` GitHub repo via the GitHub Contents API. Parses RST headers (title, author, status, type, created date, python-version) and body text. Computes cross-references by scanning for `PEP NNN` and `:pep:\`NNN\`` patterns.

2. **Timeline charts** — scatter plot of all PEPs by creation date (y-axis = type band, marker shape = status) and a stacked bar of PEPs-per-year by type.

3. **Influence scoring** — composite score from three signals:
   - `cited_by_count` (0.5 weight): how many other PEPs reference this one
   - `is_landmark` (0.3 weight): membership in a manually curated set of ~30 landmark PEPs
   - `so_score` (0.2 weight): Stack Overflow question count for the Python version tag associated with the PEP

4. **Evolution narrative** — PEP activity and average influence per era (Pre-2000 → Modern Python 2020+), plus an annotated landmark PEP timeline with bubble sizing by influence score.

5. **Reference network** — 60×60 cross-reference heatmap of the most-cited PEPs.

## Notes

- The GitHub API rate-limits unauthenticated requests. Set `GITHUB_TOKEN` in the notebook's config cell to avoid throttling during the initial fetch (~400+ PEPs).
- The Stack Overflow signal is coarse — it maps `python-version` header values to SO version tags, not individual PEP features.
