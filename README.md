# GitHub → Microsoft Fabric → OneLake Dynamic Ingestion Pipeline

A metadata-driven ingestion framework built using **Microsoft Fabric Pipelines** to dynamically discover files from **GitHub**, apply filtering rules, and ingest data into **OneLake** with support for future **incremental loading and Delta conversion**.

---

## Overview

This project demonstrates how to build a reusable ingestion framework in Microsoft Fabric without hardcoded source paths.

The pipeline dynamically:

* Reads folders/files from GitHub
* Traverses repository content
* Filters files using business rules
* Copies data into OneLake
* Tracks ingestion state for future incremental processing
* Supports scheduling and parameter-driven execution
* Maintains artifacts in Git

---

## Architecture

GitHub Repository
↓
Lookup Folder Structure
↓
Recursive File Discovery
↓
File Filtering (CSV + Size Rules)
↓
Copy Activity
↓
OneLake Landing Zone
↓
Checkpoint Framework (Incremental Ready)

---

## Features

### Dynamic File Discovery

Automatically discovers repository folders and files using GitHub API.

### Parameter Driven Execution

Pipeline supports runtime parameters:

| Parameter      | Description         |
| -------------- | ------------------- |
| p_owner        | GitHub owner        |
| p_repo         | Repository name     |
| p_folder       | Source folder       |
| p_branch       | Git branch          |
| p_targetFolder | OneLake destination |

---

### File Filtering

Current filtering rules:

* Only CSV files
* File size based routing
* Small files → Standard ingestion
* Large files → Separate processing path

---

### OneLake Landing

Files are copied into:

```text
Files/
 ├── csv10to100/
 └── csvGreater100/
```

Current output format:

```text
Parquet
```

Planned:

```text
Delta Lake
```

---

## Incremental Load (Work In Progress)

Current design includes checkpoint tracking.

Target behavior:

* Detect already processed files
* Skip unchanged files
* Process only new or modified files
* Reduce execution time and compute

Checkpoint metadata:

| Column         |
| -------------- |
| file_path      |
| file_sha       |
| last_loaded_ts |

---

## Repository Structure

```text
fabric-pipelines/
│
├── pipeline/
│   ├── Loop.json
│   └── manifest.json
│
├── notebooks/
│   ├── NB_Update_Checkpoint.ipynb
│   └── NB_Update_Checkpoint-BuiltIn.zip
│
└── README.md
```

---

## Technology Stack

* Microsoft Fabric
* Fabric Pipelines
* OneLake
* Notebook (PySpark)
* GitHub API
* Lakehouse
* SQL Endpoint
* Git

---

## Challenges Solved

* Recursive folder traversal
* Dynamic pipeline parameters
* File filtering using expressions
* GitHub REST integration
* OneLake ingestion
* Git version control

---

## Next Improvements

* True incremental ingestion
* SHA-based checkpoint validation
* Parquet → Delta conversion
* Monitoring and alerts
* CI/CD deployment
* Production hardening

---

## Getting Started

Clone repository:

```bash
git clone https://github.com/sathyarajeshpk/fabric-pipelines.git
```

Open in Microsoft Fabric and import pipeline artifacts.

---

## Author

Sathya Rajesh

If you found this useful, feel free to connect and share feedback.
