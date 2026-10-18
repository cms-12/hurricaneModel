# hurricaneModel
A PostgreSQL and Python project modeling Florida hurricane wind losses on a synthetic insurance portfolio.

## Setup

### Prerequisites
- Python 3.11 or newer
- PostgreSQL 15 or newer (developed on 17) with the PostGIS extension installed
- `psql` available on your PATH

### 1. Python environment

Create and activate a virtual environment:

```bash
python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install dependencies from the pinned requirements file:

```bash
pip install -r requirements.txt
```

Key packages: pandas, numpy, scipy, SQLAlchemy, psycopg2, GeoPandas, matplotlib.

### 2. Database

Create the project database and role (run as a PostgreSQL admin):

```bash
psql -U postgres -c "CREATE DATABASE catmodel;"
```

Copy `.env.example` to `.env` and fill in your connection details (host, port, database name, user, password). The `.env` file is gitignored and must never be committed.

Apply the migrations in order. They create the `raw`, `staging`, `core`, and `results` schemas and enable PostGIS:

```bash
psql -U postgres -d catmodel -f sql/migrations/001_create_schemas.sql
psql -U postgres -d catmodel -f sql/migrations/002_enable_postgis.sql
# ...continue in numeric order
```

### 3. Verify the setup

```bash
psql -U postgres -d catmodel -c "SELECT postgis_full_version();"
python -m catmodel.check_connection
```

If both commands return without errors, setup is complete.
