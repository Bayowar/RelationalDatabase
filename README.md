# Relational Database Architecture & US Public Health Analytics Pipeline

## Project Overview
This repository contains a data engineering portfolio demonstrating how to design, normalize, populate, and query relational databases from raw data structures. 

Using **Python**, **SQL**, and the **SQLite3 API Engine**, this application processes large-scale, archived public health records originally aggregated by *The New York Times*. The data pipeline implements row-by-row stream validation, migrates data from a single denormalized CSV file into multiple structured table schemas, enforces data constraints, and executes aggregate analytical queries combined with a `matplotlib` visualization layer.

---

## Repository Roadmap & Progression Strategy

The repository documents two distinct phases of the database development lifecycle:

* **`BuildingRelationaldb-2` (Phase 1: Baseline Architecture & Exploratory Testing)**
    * Connects to the local SQLite storage layer, validates baseline environments, and inspects raw file properties using `pandas` (`df_covid.head()`).
    * Implements exploratory SQL filtering based on Federal Information Processing Series boundaries (`WHERE fips BETWEEN 1001.0 AND 1002.0;`) and runs state-level summation metrics using structural `GROUP BY state` operations.
* **`BuildingRelationaldb3` (Phase 2: Database Normalization & Multi-Table Analytics)**
    * Advances database engineering by breaking down the single-table model into a normalized structural design with isolated entities (`covid_data_` and `state_summary`).
    * Integrates strict operational validation checks directly into the ETL loop to drop incomplete data entries and runs relational multi-table `JOIN` statements.

---

## Technical Features & Engineering Highlights

* **Stream-Parsed Data Ingestion:** To prevent local memory overhead caused by loading vast file arrays all at once, the pipeline leverages Python's native `csv` library stream reader, fetching and parsing rows sequentially.
* **Automated Data Sanitization:** Features custom conditional checking logic (`if row[3] == '' or row[5] == '': continue`) that programmatically isolates rows with missing information, safely ensuring null or unassigned metrics do not corrupt the active database.
* **SQL Injection Protection:** Every database write, transactional insert, and value population across the script strictly uses parameterized queries and placeholder tuple tokens (`?` bindings) to secure the application layer.
* **Feature Engineering:** Implements dynamic text splitting on date fields (`int(date.split('-')[0])`) during the extraction cycle to programmatically extract and populate an independent `year` column.
* **Multi-Table Relational Schema:** Reorganizes data away from raw denormalized schemas by establishing structural boundaries across two database entities linked via matching regional fields (`JOIN state_summary ss ON cd.state = ss.state`).
* **Integrated Visual Analytics:** Links database query output matrices directly to Python data science plotting libraries (`pandas` and `matplotlib.pyplot`) to calculate and render graphical bar charts tracking cumulative public health statistics across states.

---

## Database Design & Physical Schema Architecture

The relational architecture initializes two independent table components within the compiled `us_covid_recent_counties.db` database:

### 1. Observation Ledger (`covid_data_`)
Tracks fine-grained regional variables across unique space-time coordinates:
* `id` (`INTEGER PRIMARY KEY AUTOINCREMENT`): Explicit system-generated identifier tracing transactional insert order.
* `date` (`TEXT`): Standardized temporal coordinate string tracking entries.
* `county` / `state` (`TEXT`): Regional geographic and administrative boundary classifications.
* `fips` (`INTEGER`): Standardized Federal Information Processing Series tracking index code.
* `cases` (`INTEGER`): Cumulative metrics documenting local public health vectors.
* `deaths` (`REAL`): Quantified scale tracking recorded mortality metrics.
* `country` (`TEXT`): Hardcoded sovereign identity classification field defaulting to `U.S.A`.
* `year` (`INTEGER`): Extracted baseline parameter optimized for partition queries.

### 2. State Summary Framework (`state_summary`)
Acts as a pre-compiled performance-enhancing reference framework storing broader jurisdictional aggregations:
* `state` (`TEXT PRIMARY KEY`): Unique state identifier serving as the primary anchor.
* `total_cases` (`INTEGER`): Jurisdictional sum capturing cumulative macro transmission profiles.
* `total_deaths` (`INTEGER`): Cumulative historical mortality metrics grouped by state boundaries.

---

## Technologies & Tools
* **Language Environment:** Python 
* **Query Domain:** Structured Query Language (SQL)
* **Database Platform:** SQLite3 Relational Engine
* **Data Science Infra:** `pandas`, `numpy`, `csv`, `matplotlib`
