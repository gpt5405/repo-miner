# Repo Miner

Repo Miner is a Python tool for collecting and normalizing GitHub repository data (commits and issues) into clean CSV files for downstream analysis.

---

## Features

- Fetch commit history with metadata (`sha, author, email, date, message`)
- Fetch issues (excluding pull requests) with metadata  
  (`id, number, title, user, state, created_at, closed_at, comments, open_duration_days`)
- Normalize timestamps into **ISO-8601 format**
- Compute issue open duration (in days)
- Command-line interface for flexible use
- Unit-tested with GitHub Actions CI

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/gpt5405/repo-miner.git
cd repo-miner
```

### 2. Create a virtual environment
On macOs/Linux:
```bash
python3 -m venv venv
source venv/bin/activate
```
On Windows (PowerShell):
```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure GitHub API token
You need a GitHub Personal Access Token (classic, with repo scope).  
Set it as an environment variable:  
On macOS/Linux:  
```bash
export GITHUB_TOKEN=<"your_token_here">
```
On Windows (PowerShell):  
```bash
setx GITHUB_TOKEN <"your_token_here">
```

---

## Usage

### Fetch Commits

Fetch commits from a repository and save them as CSV:
```bash
python -m src.repo_miner fetch-commits --repo owner/repo --out commits.csv
```
Limit the number of commits:
```bash
python -m src.repo_miner fetch-commits --repo owner/repo --max 50 --out commits.csv
```
Output columns:
>sha, author, email, date, message

### Fetch Issues

Fetch issues (excluding pull requests) from a repository:
```bash
python -m src.repo_miner fetch-issues --repo owner/repo --out issues.csv
```
Filter by state and/or limit:
```bash
python -m src.repo_miner fetch-issues --repo owner/repo --state closed --max 20 --out issues.csv
```
Output columns:
>id, number, title, user, state, created_at, closed_at, comments, open_duration_days

created_at and closed_at are normalized to ISO-8601 format (YYYY-MM-DDTHH:MM:SS).  
open_duration_days is computed as the difference between created_at and closed_at (or None if still open).

---

## Running Tests
This projects uses pytest. Run test suite with:
```bash
pytest
```
On GitHub, tests run automatically via GitHub Actions CI.

---

## Example

```bash
python -m src.repo_miner fetch-commits --repo octocat/Hello-World --max 5 --out commits.csv
```
Saved 5 commits to commits.csv
```bash
python -m src.repo_miner fetch-issues --repo octocat/Hello-World --state open --out issues.csv
```
Saved 2 issues to issues.csv
