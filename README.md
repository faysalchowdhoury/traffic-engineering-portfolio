# Traffic engineering × ML systems portfolio

Twelve repositories, each a working Python 3.11+ package with typed code, tests, a CLI, a FastAPI
service, a Dockerfile and a results report produced by actually running it. Numbers below come from
those runs. Where a method failed or a result went against expectations, the repo says so.

| # | Repository | What it does | Headline result |
|---|---|---|---|
| 1 | [safety-ssam-trajectory-engine](https://github.com/faysalchowdhoury/safety-ssam-trajectory-engine) | FHWA SSAM-style conflict engine (TTC, PET, DeltaV) with OBB overlap and spatial indexing, on a simulated signalised intersection | Protected-permissive FYA cut crossing conflicts 50% and severe ones 82% vs permissive, at the cost of LOS C → D |
| 2 | [traffic-gnn-surrogate-metamodel](https://github.com/faysalchowdhoury/traffic-gnn-surrogate-metamodel) | GNN / LightGBM surrogates of an actuated-coordinated corridor simulator, with model registry and drift checks | GNN delay R² 0.81 vs 0.45 for the HCM equation; 2 ms per prediction vs a 1.5-s simulation (~730× faster) |
| 3 | [cloud-trajectory-etl-geh-pipeline](https://github.com/faysalchowdhoury/cloud-trajectory-etl-geh-pipeline) | PySpark ETL of real NGSIM I-80 (703,742 rows), HMM lane matching, RTS smoothing, CTM calibration with Optuna, GEH | HMM lane assignment 98.3% vs 68.2% nearest-lane at 2 m noise; calibrated CTM speed RMSE 3.0 m/s on validation |
| 4 | [cits-transit-signal-priority-hub](https://github.com/faysalchowdhoury/cits-transit-signal-priority-hub) | J2735-style BSM/SRM/SSM messaging, broker with latency and loss, in-cycle transit signal priority | Connected TSP cut bus signal delay 42% (loops 14-24%) and kept a 38% benefit at 90% packet loss |
| 5 | [dynamic-hot-pricing-controller](https://github.com/faysalchowdhoury/dynamic-hot-pricing-controller) | HOT-lane CTM with logit lane choice; PID, density-lookup and Q-learning toll controllers | Tuned PID held 99% speed reliability; the first RL agents reached 50% and 37% before a redesign reached 97% |
| 6 | [interchange-geometry-benchmark](https://github.com/faysalchowdhoury/interchange-geometry-benchmark) | Cap-X-style critical-lane screening of diamond, DDI and turbo-roundabout dumbbell, with Monte Carlo design-year risk | DDI cut delay 38% at balanced demand; the diamond wins when arterial through traffic dominates |
| 7 | [vectorized-micromobility-physics](https://github.com/faysalchowdhoury/vectorized-micromobility-physics) | 2-D non-lane-based mixed traffic (cars, motorcycles, scooters, cargo bikes), dynamic PCU, Optuna calibration | Mixed-traffic target hit within 0.2%; car-only missed by 41%, a structural limit documented in the repo |
| 8 | [synchro-vissim-ntcip-controller](https://github.com/faysalchowdhoury/synchro-vissim-ntcip-controller) | NEMA TS2 dual-ring actuated-coordinated controller, Synchro UTDF I/O, MAXBAND vs delay-based offsets, NTCIP 1202 over SNMP | Simulation-based offsets cut corridor delay 20%; MAXBAND produced a one-way band that raised westbound delay 60% |
| 9 | [cav-fleet-platoon-simulator](https://github.com/faysalchowdhoury/cav-fleet-platoon-simulator) | Wiedemann 99 human drivers mixed with PATH ACC/CACC vehicles | Capacity is U-shaped: −10% at 20-40% CAV, +65% (4,339 veh/h) at 100% CACC |
| 10 | [v2i-arterial-greenwave-mesh](https://github.com/faysalchowdhoury/v2i-arterial-greenwave-mesh) | GLOSA speed advice from SPaT with a power-based fuel model | 600 m range: −61% stops, −7% fuel; at 300 m the fuel benefit nearly disappears (−1%) |
| 11 | [transit-hub-pedestrian-egress](https://github.com/faysalchowdhoury/transit-hub-pedestrian-egress) | Social-force platform evacuation vs an NFPA 130-style hand calculation | Hand calc 1.6-2.5× more conservative, mostly because the simulation lacks stair ascent (stated as a limitation) |
| 12 | [drone-trajectory-video-fusion](https://github.com/faysalchowdhoury/drone-trajectory-video-fusion) | Drone video to georeferenced trajectories: DLT homography, stabilisation, ByteTrack, CLEAR-MOT, count fusion | MOTA 0.942, 0.30 m position error; fused counts were worse than video alone (small-sample weighting) |

## Shared conventions

* `pip install -r requirements.txt && pip install --no-deps -e .`, then `pytest` and the package CLI.
  Each README gives the exact command that reproduces its `results/` folder.
* Pinned dependencies; `pydantic` models for configuration and API payloads; `logging` instead of prints.
* Real data where it exists (NGSIM I-80 in repos 3 and 12); otherwise simulators written here, with the
  assumptions listed. Nothing claims Vissim, Synchro or field-measured results.
* Each repo has `docs/linkedin_post.md`, a short write-up of the main finding.

## License

MIT, per repository.
