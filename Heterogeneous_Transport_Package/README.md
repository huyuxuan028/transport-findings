# Updated Heterogeneous Furniture Transport Project

This package supersedes the old identical-three-vehicle demonstration for the
new brief. All reports are English and have no report preparation date.

## Three repository areas

1. `transport-findings`: formalization, representation rationale, papers, meeting script.
2. `transport-generator`: audited Amazon sample, normal-based transport generator, tests.
3. `transport-algorithms`: two-array genetic algorithm, independent validator, pilot.

Each area is prepared as an independent local Git repository with a commit and
portable Git bundle. No remote GitHub repository has been created or pushed.
See `GITHUB_SETUP.md` for the remaining publication step.

## Quick start on Windows

Run from the corresponding extracted directory. Python 3.10+ is sufficient for
data processing, generating, solving and tests; no third-party packages needed.

```powershell
cd transport-generator
python import_catalog.py data/amazon_furniture_sample.csv
python generate.py --catalog data/catalog_proxy.json --allow-proxy --cycles 2 --seed 42 --output data/development.json
python -m unittest -v test_generator
```

```powershell
cd ../transport-algorithms
python solver.py
python -m unittest -v test_solver
python run_experiments.py
```

The algorithm repository contains a frozen copy of the development input. If you
change generator parameters, explicitly copy the new JSON to `data/example.json`
or pass `--instance` to the solver. This prevents silent changes to benchmark data.
Generated JSON, observed outputs and figures are already included.

## Critical assumptions

- One physical vehicle of each supplied type; counts need agreement.
- Multiple sequential trips; all customer orders available at time zero.
- Weight and aggregate volume only; no geometric loading, shifts or windows.
- The audited dataset sample has no records meeting the strict packaging rule.
  The runnable example explicitly enables catalog proxies, never verified boxes.
- Default fuel objective is a proposal. It is a constant-rate estimate, not an
  observed fuel measurement or a physical load/speed-dependent model.
- Top speeds and overall vehicle lengths are recorded but are not average speed
  or usable cargo-bay dimensions. Schedule speed is an explicit 35 km/h assumption.
- No certified optimum or final GA-versus-swarm benchmark has been established.

## What is complete and what is pending

Research, representation rationale, catalog audit, initial generator, initial GA,
validation and reproducible pilot are complete for the stated simplified model.
Publication to GitHub is pending repository creation/destination selection. The
available GitHub connector can inspect or edit existing repositories but does
not expose a new-repository operation in this session. No browser fallback or
publication to an unrelated existing repository was performed.
