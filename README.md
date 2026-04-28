# Istanpolis Data Engineering Tools

Python tooling built for the **Istanpolis** research project at UC Berkeley (2021–2024), a collaborative digital humanities initiative documenting the Greek communities of Istanbul from the 19th–20th centuries. The project is led by Professor Christine Philliou (Department of History) in partnership with researchers across UC Berkeley, Koç University, and the University of Crete.

## What I Built

My role on the project was technical: building Python tooling and workflows so the research team — historians, graduate students, and international collaborators — could assemble, parse, clean, and query large semi-structured datasets reliably. Three main deliverables:

### 1. Ottoman Census Parser (`defter_parser.ipynb`)

Parses Ottoman Turkish census registers (*defters*) from Excel spreadsheets into structured, analysis-ready DataFrames. The source data is a 3,100+ row spreadsheet with 23 columns covering household records from 1820s-era Greek communities in Istanbul — names, professions, places of origin, ages, tax status, and more.

The parser handles:
- Dropping metadata rows and extracting the column-to-FactGrid property mapping from embedded header rows
- Cleaning unnamed/overflow columns
- Extracting and normalizing FactGrid property IDs (P-values) for each column to enable linked data upload
- Data quality checks (null value counts per column, record boundary detection)
- Connecting to the FactGrid API to resolve property IDs to human-readable labels

### 2. Fuzzy Search for Transliteration Matching

Ottoman Turkish names have inconsistent transliterations across sources — student workers' translations often differed by one or two characters from the official ArcGIS/Wikidata translations. I wrote a fuzzy matching algorithm in Python to automate the reconciliation, reducing manual workload by ~60%. Before this tool, each name had to be manually matched to a physical ArcGIS location.

### 3. FactGrid Entity Import API (`import_entity.ipynb`)

The team needed to bulk-download and upload datasets from [FactGrid](https://database.factgrid.de), a Wikibase-powered linked data platform for historical research. The existing API integration didn't support the team's query patterns. I rebuilt the integration using the `wikibaseintegrator` Python library to automate entity retrieval for local database keys, improving data consistency and eliminating ~15% of null values in the local dataset.

## Tech Stack

- **Python** — pandas, numpy, requests, sqlite3
- **wikibaseintegrator** — Python client for Wikibase/FactGrid API
- **Google Colab** — collaborative notebook environment
- **FactGrid** — Wikibase-powered linked data platform
- **Data formats** — Excel (.xlsx), CSV, JSON (Wikibase API responses)

## Project Context

Istanpolis digitizes early Ottoman census records (*nüfus defterleri*) to build prosopographic profiles of Greek community members — recording names, professions, family relationships, places of origin, and institutional affiliations. The data feeds into FactGrid's linked data graph and supports mapping, demographic analysis, and archival research.

The project involves an international team across UC Berkeley, Istanbul, Athens, and Crete. My tools were used by researchers with varying technical skill levels, so deliverables were provided as documented, runnable Colab notebooks with clear outputs that non-technical collaborators could verify.

## Notebooks

- [Defter Basic Parser](https://colab.research.google.com/drive/1X4VMjOG9fgoIs_qVxofnoZst1AGHx5QP) — Census data parsing and FactGrid property mapping
- [Entity Import](https://colab.research.google.com/drive/1ertLES80T8o-AYWcgKl1gl1tjSuWeUGD) — FactGrid entity retrieval and import

## What I'd Improve

- **Add schema validation** with pydantic or pandera to catch data quality issues before FactGrid upload
- **Automate the full pipeline** with Airflow — from Excel ingestion through parsing, fuzzy matching, and FactGrid upload
- **Replace the fuzzy search** with a more robust approach using `rapidfuzz` or `jellyfish` for better transliteration matching across Turkish/Greek/Ottoman scripts
- **Add unit tests** for the parser's boundary detection and column mapping logic
