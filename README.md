# health-nudge

Streamlit dashboard analysing health-nudge engagement and health metrics
(aggregates only). Built by a coding agent.

## Fresh-machine setup

1. **Install mamba** via [Miniforge](https://github.com/conda-forge/miniforge):
   ```bash
   # macOS / Linux
   curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
   bash Miniforge3-$(uname)-$(uname -m).sh
   ```
   Restart your shell so `mamba` is on the PATH.

2. **Create the environment** (Python 3.11 + Poetry, from conda-forge):
   ```bash
   mamba env create -f environment.yml
   ```

3. **Install dependencies** with Poetry. `poetry.toml` sets
   `virtualenvs.create = false`, so packages install into the mamba env;
   no separate virtualenv is created:
   ```bash
   mamba run -n health-nudge poetry install
   ```

4. **Configure secrets** (after HN-002): copy `.streamlit/secrets.toml.example` to
   `.streamlit/secrets.toml` and fill in the values (never commit it).

5. **Run the app** (`app.py` arrives in HN-002):
   ```bash
   mamba run -n health-nudge streamlit run app.py
   ```

## Development

```bash
mamba run -n health-nudge pytest                                          # tests
mamba run -n health-nudge isort . && mamba run -n health-nudge black .    # format
mamba run -n health-nudge flake8                                          # lint
mamba run -n health-nudge mypy src                                        # types
```

Add packages only with `mamba run -n health-nudge poetry add <pkg>`
(`--group dev` for dev tools) and commit `poetry.lock`. Never use
`pip install` or `mamba install` for Python packages.
