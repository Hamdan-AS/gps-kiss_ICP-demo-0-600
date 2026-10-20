# GPS + KISS-ICP demo (0–600 s)

This repository is a portable evidence export from a ROS 2 Humble KISS-ICP run over a 600-second recorded interval. It is intended for ROS 2 developers who want to inspect the trajectory, compare independent GPS and lidar motion, or reuse the generated artifacts in analysis tools without downloading the source bag or a multi-gigabyte map.

The technical interface and coordinate conventions are documented in [docs/ARTIFACT_CONTRACT.md](docs/ARTIFACT_CONTRACT.md).

## Included

- `artifacts/trajectory.tum`: 2,994 estimated poses in the KISS local frame.
- `artifacts/gps_kiss_comparison.png`: GPS and KISS distance comparison.
- `artifacts/gps_distance.csv` and `artifacts/kiss_distance.csv`: local ENU path samples and cumulative distances.
- `reports/summary.json`: selected counts and comparison metrics. Exact GPS origin coordinates are omitted.
- `reports/run_manifest.json`: portable run metadata using workspace-relative input/config names.
- `reports/trajectory_check.txt`: trajectory integrity check receipt.
- `reports/evidence_validation.json`: compact integrity counts from the generated evidence bag; the bag itself is excluded.

## Artifact map

| Path | Use |
| --- | --- |
| `artifacts/trajectory.tum` | KISS-ICP pose stream for trajectory tools and offline analysis. |
| `artifacts/gps_distance.csv` | GPS-derived local ENU samples and cumulative distance. |
| `artifacts/kiss_distance.csv` | KISS-ICP-derived local ENU samples and cumulative distance. |
| `artifacts/gps_kiss_comparison.png` | Visual comparison of the two distance traces. |
| `reports/summary.json` | Run counts, distances, and held-out comparison metric. |
| `reports/run_manifest.json` | Portable provenance and source interface metadata. |
| `reports/trajectory_check.txt` | Human-readable trajectory integrity receipt. |
| `reports/evidence_validation.json` | Message counts and evidence validation status. |

## Not included

The full-resolution map (about 1.6 GiB), full evidence rosbag (about 2.0 GiB), and RViz display bags/maps are not included. They are generated data, exceed normal GitHub file limits, and may reveal details of the recorded location. The source bag and ROS workspace are also not included. See `.gitignore` for guarded extensions.

The included trajectory and ENU CSVs are local-frame data and can still disclose route shape. Confirm that the recorded data is suitable for public release before publishing this folder.

## Interpretation

GPS is an independent comparison and is not fused into KISS-ICP. Alignment is for display only: a 2D yaw and translation fit on the first half, with the second half held out. Cross-sensor timing is approximate, and neither estimate is ground truth or an absolute accuracy claim.

The historical run used ROS 2 Humble and the workspace configuration `config/kiss_icp.yaml`. Host-specific paths and CPU affinity settings have been removed from the portable reports. No machine-specific playback command is included; playback requires the original workspace and the omitted ROS bag artifacts.

## Reuse and validation

The export is analysis-ready but not a standalone ROS 2 package. Start with the artifact contract, then use the JSON reports as the machine-readable source of run metadata. The included validation receipt records 2,994 finite, strictly ordered poses, a 599.600-second trajectory duration, a maximum 0.600-second timestamp gap, and normalized quaternions. The CSV files use `bag_relative_s` plus local `east_m` and `north_m` coordinates; their first row is the zero-distance reference sample.

Before publishing derived data, review whether the route shape is acceptable for your use case. The export intentionally omits exact GPS origin coordinates and all raw recordings.
