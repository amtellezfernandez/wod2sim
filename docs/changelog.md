# Changelog

All notable adapter-release changes are tracked here.

## Unreleased - 2026-07-20

- Focused the public branch on the AlpaSim external-driver adapter, setup and
  readiness tooling, reproducible execution, and real integration evidence.
- Added a hash-validated AlpaSim run with NAVSIM EgoStatusMLP: `197/197` finite
  outputs over `19.93` simulated seconds through the live external driver,
  controller, and physics services.
- Retained the raw camera-and-map run video, expanded configs, simulator
  results, driver telemetry, and immutable source/checkpoint hashes.
- Added deterministic reconstruction of the public fixture's declared flat
  physics surface and a telemetry-recording seed-frame video-model server.
- Added the AlpaSim E2E challenge-style external-driver package and one retained
  local conformance run.
- Clarified that WOD2Sim moves a policy interface onto AlpaSim scenes; it does
  not convert WOMD scenes into AlpaSim or make logged non-ego agents reactive.

## 0.1.0 - 2026-07-17

- Published AlpaSim adapters for `constant_velocity`, `route_following`,
  `token_dagger_bc`, and `direct_actor_planner`.
- Added setup, readiness, launch, batch, audit, summary, and support-bundle
  commands.
- Standardized runtime configuration on the `WOD2SIM_` environment namespace.
- Added packaged AlpaSim override files with third-party attribution.
- Added full tests, wheel smoke checks, and fresh-checkout CI coverage.
