# tpt-nova-sim Progress
License: MIT OR Apache-2.0 · (c) TPT Solutions · Edition 2024 · Determinism: same-platform bitwise

## [ ] Layer 0: Repo & Infra
- [ ] Cargo workspace; crates: tpt-nova-sim (facade), -time, -sensors, -replay, -agents, -datasets (+ -py for PyO3)
- [ ] Naming check (all crates prefixed `tpt-nova-sim-`)
- [ ] LICENSE-MIT, LICENSE-APACHE, per-crate `license = "MIT OR Apache-2.0"`, SPDX headers
- [ ] README, CONTRIBUTING (dual-license contribution clause), CODE_OF_CONDUCT, SECURITY
- [ ] Git deps on TPT-Solutions: tpt-nova (>=0.4,<0.6), tpt-kinetix, tpt-math, tpt-anima (optional feature), tpt-nova-drivers
- [ ] deny.toml: allow MIT/BSD-2/BSD-3/ISC/Zlib/BSL-1.0 (+Apache-2.0 only when dual with MIT); ban GPL/AGPL/LGPL/MPL/SSPL
- [ ] CI: fmt, clippy, test, cargo-deny, Linux + Windows + macOS
- [ ] vectis-domain.toml
- [ ] schemas/components.nova.json, schemas/events.nova.json
- [ ] drivers/ directory
- [ ] Dependency-direction guard (no tpt-nova-games / -viz / -vectis deps)

## [ ] Layer 1: Deterministic Time (tpt-nova-sim-time)
- [ ] `SimTime`, `TimeAnchor` components
- [ ] `sim_time_step_system`, `time_scale_system`
- [ ] `time_step_driver` (fixed, variable, adaptive), `rewind_driver` trait
- [ ] Time scale and pause/resume
- [ ] Seeded RNG; no wall-clock in sim logic
- [ ] Tests: step consistency; 2x == 1x final state

## [ ] Layer 2: Replay & Logging (tpt-nova-sim-replay)
- [ ] Event log format (versioned, delta-encoded)
- [ ] `ReplayRecorder` + `replay_record_system`
- [ ] `ReplayPlayer` + `replay_playback_system` + seek-to-tick
- [ ] `replay_storage_driver` (file, in-memory ring buffer, network stream)
- [ ] `replay_injection_driver`
- [ ] Ghost playback (A/B vs live)
- [ ] Rewind integration with Layer 1
- [ ] Tests: bit-for-bit replay; 60 s record/playback/scrub

## [ ] Layer 3: Synthetic Sensors (tpt-nova-sim-sensors)
- [ ] `LidarSensor`, `CameraSensor`, `ImuSensor` components (+ GPS, radar types)
- [ ] `sensor_poll_system` (sim-time, decoupled rates)
- [ ] `lidar_ray_cast_driver`: CPU multithreaded
- [ ] `lidar_ray_cast_driver`: GPU compute (for hot-swap)
- [ ] `sensor_noise_system` + `sensor_noise_driver` (Gaussian, weather, IMU drift)
- [ ] `sensor_render_system` (depth/segmentation ground truth)
- [ ] Sensors query world state only (no duplicate physics from tpt-kinetix)
- [ ] Test: CPU->GPU driver hot-swap without ECS crash

## [ ] Layer 4: AI & Agents (tpt-nova-sim-agents)
- [ ] `ObservationSpec` / `ActionSpec` (Gymnasium-shaped Space/Env traits)
- [ ] `RlAgent` + observe/act/reward systems
- [ ] Multi-agent reward shaping
- [ ] `BehaviorTree` + `behavior_tree_eval_driver`
- [ ] `policy_inference_driver` (mock random, ONNX, tpt-crucible)
- [ ] Runtime policy hot-swap via tpt-nova-drivers
- [ ] PyO3 Gymnasium bindings (`tpt-nova-sim-py`) + Python tests
- [ ] Test: agent at fixed 100 Hz independent of render rate

## [ ] Layer 5: Datasets & Binding (tpt-nova-sim-datasets)
- [ ] `TimeSeriesSource`, `DataBinding` components
- [ ] `dataset_reader_driver`: CSV (MIT)
- [ ] `dataset_reader_driver`: Parquet (custom/MIT-only parser)
- [ ] ROS bag reader (stretch)
- [ ] `dataset_stream_system` (async chunks)
- [ ] `data_bind_system`
- [ ] Synchronized multi-stream playback; HIL streaming demo

## [ ] Integration & Importers
- [ ] Importers: urdf (quick-xml), point_cloud, parquet, rosbag, sensor_calibration
- [ ] Facade crate re-exports + feature flags
- [ ] End-to-end example sim

## [ ] Release
- [ ] Success criteria verified (spec §9, 5 items)
- [ ] `cargo deny check` clean on full tree
- [ ] Docs (rustdoc + guide), determinism caveats documented
- [ ] Changelog, tag v0.1.0, publish

## Open Items
- [ ] Confirm exact GitHub repo names/URLs for upstream crates
- [ ] Decide GPU compute backend (wgpu license check)
- [ ] Decide ONNX runtime choice (license check)
