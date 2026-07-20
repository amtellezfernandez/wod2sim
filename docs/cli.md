# CLI

## Setup And Execution

| Command | Purpose |
| --- | --- |
| `wod2sim-doctor` | Validate the installed package and optional AlpaSim environment. |
| `wod2sim-setup` | Apply and validate the tracked AlpaSim override layer. |
| `wod2sim-ready` | Check platform, local AlpaSim environment, Docker, GPU, image, and scene readiness. |
| `wod2sim-launch` | Materialize or execute one matched driver and AlpaSim run. |
| `wod2sim-batch` | Execute scenes independently with retries and timeouts. |
| `wod2sim-reproduce` | Plan or execute setup through evidence packaging. |

## Inputs And Run Records

| Command | Purpose |
| --- | --- |
| `wod2sim-build-local-cache` | Build or validate a local scene cache. |
| `wod2sim-build-oracle-proxy` | Build the scene-matched actor proxy required by the direct planner. |
| `wod2sim-audit-run` | Normalize driver logs and check route and sensor inputs. |
| `wod2sim-support-bundle` | Package selected logs, configs, and audit output. |
| `wod2sim-batch-summary` | Aggregate a multi-scene batch. |
| `wod2sim-benchmark-summary` | Aggregate reproduction manifests and run audits. |
| `wod2sim-benchmark-readiness` | Check whether a requested public benchmark matrix is complete. |
| `wod2sim-promote-batch-summary` | Copy a validated local summary to an explicit destination. |
| `wod2sim-evidence` | Inspect AlpaSim runtime metrics. |
| `wod2sim-challenge-driver` | Serve or self-test the AlpaSim E2E-style external driver. |

## Development Targets

| Command | Purpose |
| --- | --- |
| `make test` | Run the test suite. |
| `make lint` | Run Ruff over the repository. |
| `make conformance` | Run the dependency-light adapter conformance tier. |
| `make coverage` | Run the test suite with the coverage gate. |
| `make smoke` | Install a fresh copied checkout and exercise the public CLI. |
| `make build` | Build the wheel and source distribution. |
| `make verify` | Run lint, conformance, coverage, smoke, and build. |
| `make clean` | Remove local build, cache, and Python bytecode artifacts. |

Run any command with `--help` for its complete arguments.

`wod2sim-ready` is a launch-readiness check. By default it requires the local
AlpaSim Python environment and `alpasim_wizard` executable because
`wod2sim-launch` needs both even when only materializing commands. Use
`--skip-local-env` only for host or container diagnostics.
