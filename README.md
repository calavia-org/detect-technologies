# Detect Technologies Action

A GitHub Action that detects buildable technologies in a repository based on file indicators. Supports Python, Go, Rust, Java, Ansible Collections, Containers, Ansible Execution Environments, and Helm.

## Supported Technologies

| Technology | Indicator Files |
|------------|-----------------|
| **python** | `pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `Pipfile` |
| **go** | `go.mod` |
| **rust** | `Cargo.toml` |
| **java** | `pom.xml`, `build.gradle`, `build.gradle.kts` |
| **ansible-collection** | `galaxy.yml` (with `type: collection`) |
| **container** | `Dockerfile`, `Containerfile` |
| **ansible-execution-environment** | `execution-environment.yml`, `execution-environment.yaml` |
| **helm** | `Chart.yml`, `chart.yml`, `helm/`, `charts/` |

## Usage

```yaml
- uses: calavia-org/detect-technologies-action@v1
  id: techs

- name: Build Python
  if: steps.techs.outputs.python != ''
  run: pip install -e .

- name: Build Go
  if: steps.techs.outputs.go != ''
  run: go build -o bin/ .

- name: Build Rust
  if: steps.techs.outputs.rust != ''
  run: cargo build --release

- name: Build Java
  if: steps.techs.outputs.java != ''
  run: mvn package

- name: Build Container
  if: steps.techs.outputs.container != ''
  run: podman build -t myapp .

- name: Build Helm Chart
  if: steps.techs.outputs.helm != ''
  run: helm package ${{ steps.techs.outputs.helm }}
```

## Outputs

| Output | Description |
|--------|-------------|
| `technologies` | Comma-separated list of detected technologies |
| `count` | Number of technologies detected |
| `python` | Path to Python indicator file (empty if not detected) |
| `go` | Path to Go indicator file (empty if not detected) |
| `rust` | Path to Rust indicator file (empty if not detected) |
| `java` | Path to Java indicator file (empty if not detected) |
| `ansible-collection` | Path to Ansible collection indicator file (empty if not detected) |
| `container` | Path to Container indicator file (empty if not detected) |
| `ansible-execution-environment` | Path to Ansible EE indicator file (empty if not detected) |
| `helm` | Path to Helm indicator file (empty if not detected) |

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `fail-on-missing` | Fail if no technologies detected | `false` |

## License

MIT
