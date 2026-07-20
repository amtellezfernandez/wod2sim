# WOMD Targeting

WOD2Sim implements a trajectory-policy interface on AlpaSim scenes. It does not
load Waymo Open Motion Dataset (WOMD) records into AlpaSim. These are different
targets with different data and runtime requirements.

## Choose The Target

| Target | Status | Correct starting point |
| --- | --- | --- |
| Run a trajectory policy through AlpaSim's external driver | Implemented | Use the WOD2Sim presets, setup tooling, and AlpaSim scenes. |
| Connect a policy trained with WOMD-derived data to AlpaSim | Adapter path implemented; checkpoint compatibility is model-specific | Match the checkpoint's observations, coordinates, timing, route input, output sampling, and dependencies to a WOD2Sim model adapter. |
| Run policies directly on licensed WOMD records | Not provided by this branch | Use [Waymax](https://github.com/waymo-research/waymax) or another WOMD-native runtime. |
| Run an actual WOMD scenario inside AlpaSim | Not implemented | Build a licensed dataset-to-scene converter and validate maps, actors, signals, routes, clocks, and renderable assets. |
| Create a reactive multi-agent counterfactual from a logged scenario | Not implemented | Add and validate reactive models for the non-ego agents; logged future tracks alone are playback. |

## Target A Policy At AlpaSim

AlpaSim sends live sensor, ego-motion, command, and navigation messages to an
external driver. WOD2Sim assembles those messages into policy inputs and
expects a five-second ego-relative trajectory in return.

Start with a dependency-light run:

```bash
uv run wod2sim-launch \
  --mode print \
  --alpasim-root /path/to/alpasim \
  --model route_following \
  --scene-preset fresh_3scene
```

Before registering a learned checkpoint, record:

- the exact observation tensors and preprocessing;
- the ego and world coordinate conventions;
- whether route geometry, a high-level command, or neither is required;
- input cadence, output horizon, point count, and timestamp convention;
- checkpoint revision, file hash, framework version, and device requirements;
- how missing cameras, routes, or actor state must be handled.

`token_dagger_bc` is a checkpoint adapter, not a promise that an arbitrary
WOMD-trained checkpoint has compatible inputs. Incompatible checkpoints should
fail readiness rather than be silently reshaped.

## Run Actual WOMD Records

WOMD contains logged agent tracks, vector-map features, traffic-control state,
and scenario metadata. A WOMD-native stack should retain those semantics rather
than converting them through an unrelated simulator scene format.

Waymax provides a direct WOMD loading and simulation path. A production
WOD2Sim-to-Waymax binding would still need to map:

- simulator state and policy observations;
- SDC ownership and action application;
- reset, step, termination, and log-playback behavior;
- route provenance and policy input requirements;
- timestamps, action cadence, and output horizon;
- licensed dataset access, versions, and retained run metadata.

That binding is not part of this adapter release.

## Convert WOMD Scenes Into AlpaSim

Running a WOMD record inside AlpaSim is a separate scene-conversion project. At
minimum it requires:

- license-compliant WOMD access and artifact provenance;
- vector-map geometry and topology conversion;
- dynamic map state and traffic-control conversion;
- actor dimensions, poses, velocities, identities, and ownership;
- route candidates or navigation targets;
- a common frame, origin, heading convention, timestamp, and tick rate;
- AlpaSim-compatible scene and renderable assets;
- explicit treatment of logged versus reactive non-ego agents.

Until those pieces exist and are tested together, describe WOD2Sim as a policy
adapter for AlpaSim scenes, not as a WOMD-to-AlpaSim converter.

## Official References

- [Waymo Open Dataset](https://waymo.com/open/)
- [WOMD format documentation](https://waymo.com/open/data/motion/)
- [Waymax repository](https://github.com/waymo-research/waymax)
- [NVIDIA AlpaSim repository](https://github.com/NVlabs/alpasim)
