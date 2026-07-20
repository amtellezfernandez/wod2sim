# Design

WOD2Sim connects short-horizon trajectory policies to AlpaSim's long-lived
external-driver service. The policy returns a local ego trajectory; AlpaSim
owns the downstream controller, physics, sensors, and next simulator state.

```text
AlpaSim messages
  -> session state
  -> policy observation
  -> trajectory policy
  -> output validation and resampling
  -> AlpaSim trajectory response
```

## Input Assembly

The driver receives camera images, ego motion, high-level commands, route
waypoints, and lifecycle messages over time. WOD2Sim:

- keeps each simulator session isolated;
- rejects messages that arrive in an invalid lifecycle state;
- retains route geometry separately from high-level route intent;
- tracks camera timestamps and content freshness;
- exposes only the inputs declared by the selected model adapter.

WOD-style describes this policy-interface shape. It is not an official Waymo
message format and does not imply that a WOMD scenario is running in AlpaSim.

## Model Presets

The dependency-light models are:

- `constant_velocity`: a straight-line smoke baseline;
- `route_following`: a waypoint-following baseline.

Optional models use the same external-driver boundary:

- `token_dagger_bc`: a learned token policy with a compatible local checkpoint;
- `direct_actor_planner`: a candidate planner with a scene-matched actor proxy.

The challenge-style driver also supports the public NAVSIM EgoStatusMLP
architecture for the retained integration run. It is not registered as a
general release-core preset because its checkpoint and framework dependencies
are external.

## Trajectory Conversion

Policy outputs are interpreted as ego-relative endpoint samples over a
five-second horizon. If the point count already matches
`round(output_frequency_hz * horizon_seconds)`, WOD2Sim returns the trajectory
unchanged. Otherwise it anchors interpolation at the current ego origin,
interpolates x/y positions onto the runtime endpoint grid, and recomputes
headings.

Outputs with non-finite coordinates, invalid shapes, or inconsistent timing are
rejected before they reach the AlpaSim controller.

## Setup And Runtime Checks

`wod2sim-setup` validates an AlpaSim checkout before applying the tracked
override files. `wod2sim-ready` checks platform, environment, Docker/GPU,
runtime image, model inputs, and selected scene assets. `wod2sim-launch` then
materializes the exact driver and simulator commands before optional execution.

Executed workflows retain expanded configuration, commands, provenance, driver
events, normalized audits, and summaries. Private checkpoints and gated scene
assets remain local.

## AlpaSim Overrides

The tracked override layer under `src/wod2sim/alpasim_overrides/` extends the
AlpaSim checkout at its plugin and route-message boundaries. The source copy
under `third_party/alpasim_overrides/` records provenance and modifications.
WOD2Sim policy logic remains in this package; AlpaSim itself is not vendored.

## Non-Goals

WOD2Sim is not:

- a simulator or controller replacement;
- a new autonomous-driving policy;
- a WOMD-to-AlpaSim scene converter;
- a source of AlpaSim scenes or learned checkpoints;
- a policy-performance benchmark.
