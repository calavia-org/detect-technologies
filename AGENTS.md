# AGENTS.md

## Repository Overview

GitHub Action (composite) that detects buildable technologies in repositories based on file indicators. Entire logic lives in `action.yml` as a bash script.

**Repository**: `calavia-org/detect-technologies` (renamed from `detect-technologies-action`)

## Architecture

- **`action.yml`**: Single file containing all logic (bash composite action)
- **`tests/`**: Empty directory (no tests implemented yet)
- **`.github/workflows/`**: Uses shared workflows from `calavia-org/workflows-lib`
- **`.pre-commit-config.yaml`**: Local validation with actionlint + action structure check

## Supported Technologies

Python, Go, Rust, Java, Ansible Collections, Containers, Ansible Execution Environments, Helm

## Key Commands

```bash
# Setup virtual environment (one-time)
python3 -m venv .venv
source .venv/bin/activate

# Install pre-commit (one-time)
pip install pre-commit

# Install pre-commit hooks (one-time)
pre-commit install

# Run all pre-commit hooks manually
pre-commit run --all-files

# Run just actionlint
pre-commit run actionlint --all-files

# Run just markdownlint
pre-commit run markdownlint --all-files
```

## Development Notes

- **No dependencies**: Pure bash script in action.yml, no package.json or build tools
- **Pre-commit hooks**: actionlint, check-yaml, markdownlint, conventional commits, no direct commits to main
- **Release**: Uses `calavia-org/release-github@v1` for semantic versioning and GitHub releases
- **Output format**: Technologies detected as comma-separated list, individual paths as separate outputs

## Gotchas

- Helm detection uses `find` to locate Chart.yml anywhere in the repo (not just root)
- Ansible Collection requires `type: collection` in galaxy.yml (not just presence of file)
- Python detection prioritizes pyproject.toml > setup.py > setup.cfg > requirements.txt > Pipfile

## References

- **README.md**: Usage examples and output documentation
- **action.yml**: Complete implementation with all detection logic
- **calavia-org/release-github**: Release action used for semantic versioning
