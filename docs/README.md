# WOD2Sim Documentation

| Guide | Purpose |
| --- | --- |
| [Getting started](getting-started.md) | Install WOD2Sim, connect an AlpaSim checkout, and materialize a run. |
| [Design](design.md) | External-driver architecture, policy presets, trajectory conversion, and validation. |
| [WOMD targeting](womd-targeting.md) | Distinguish policy integration, native WOMD execution, and WOMD-to-AlpaSim scene conversion. |
| [Reproduction](reproduction.md) | Plan or execute a run and retain its configuration and evidence. |
| [Conformance](conformance.md) | Dependency-light checks for the adapter and public command surface. |
| [AlpaSim E2E compatibility](challenge-compatibility.md) | Package and run the evaluator-owned external-driver path. |
| [CLI](cli.md) | Public commands and development targets. |
| [Changelog](changelog.md) | Adapter release history. |

The repository does not install AlpaSim, redistribute gated scene assets, or
ship learned-policy checkpoints. Start with the dependency-light
`constant_velocity` or `route_following` preset, then connect a learned policy
only after matching its required observations, coordinates, timing, and route
inputs to the adapter.
