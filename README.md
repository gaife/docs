# GAIFE Documentation

## Overview
GAIFE stands for Generative Artificial Intelligence for Enterprise

## Setup

### Requirements

- Python 3.11+
- Project dependencies managed by [uv](https://docs.astral.sh/uv/) or use `pip install -r requirements.txt`
  - Install uv from [here](https://docs.astral.sh/uv/getting-started/installation/)

### Development

- Install dependencies:

```bash
uv sync
# or
pip install -r requirements.txt
```

- Set up pre-commit:

```bash
uv run pre-commit install
```

- Run pre-commit hooks:

```bash
uv run pre-commit run --all-files
```

- Start the development server:

```bash
uv run mkdocs serve
```
