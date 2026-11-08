# Environment conventions

- Mamba env name: `health-nudge`, defined in `environment.yml`
  (only `python=3.11` and `poetry`, from conda-forge).
- Poetry manages ALL Python packages and installs INTO the mamba env
  (`poetry.toml`: `virtualenvs.create = false`). Never create a separate virtualenv.
- Never `pip install` or `mamba install` Python packages directly.
  Use `poetry add <pkg>` or `poetry add --group dev <pkg>`.
- Ask the user before adding any new dependency (state the package and why).
- Commit `poetry.lock` whenever dependencies change.
- The shell does not keep an activated env between commands. ALWAYS run:
  `mamba run -n health-nudge <command>`
  e.g. `mamba run -n health-nudge pytest`, `mamba run -n health-nudge poetry add x`.
