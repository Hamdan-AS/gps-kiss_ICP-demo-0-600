# GPS + KISS-ICP demo (0–600 s)

This GitHub-ready snapshot contains lightweight plots, trajectory output, and summary reports from a KISS-ICP run over a 600-second recorded interval.

## Included

- `artifacts/trajectory.tum`: 2,994 estimated poses in the KISS local frame.
- `artifacts/gps_kiss_comparison.png`: GPS and KISS distance comparison.
- `artifacts/gps_distance.csv` and `artifacts/kiss_distance.csv`: local ENU path samples and cumulative distances.
- `reports/summary.json`: selected counts and comparison metrics. Exact GPS origin coordinates are omitted.
- `reports/run_manifest.json`: portable run metadata using workspace-relative input/config names.
- `reports/trajectory_check.txt`: trajectory integrity check receipt.
- `reports/evidence_validation.json`: compact integrity counts from the generated evidence bag; the bag itself is excluded.

## Not included

The full-resolution map (about 1.6 GiB), full evidence rosbag (about 2.0 GiB), and RViz display bags/maps are not included. They are generated data, exceed normal GitHub file limits, and may reveal details of the recorded location. The source bag and ROS workspace are also not included. See `.gitignore` for guarded extensions.

The included trajectory and ENU CSVs are local-frame data and can still disclose route shape. Confirm that the recorded data is suitable for public release before publishing this folder.

## Interpretation

GPS is an independent comparison and is not fused into KISS-ICP. Alignment is for display only: a 2D yaw and translation fit on the first half, with the second half held out. Cross-sensor timing is approximate, and neither estimate is ground truth or an absolute accuracy claim.

The historical run used ROS 2 Humble and the workspace configuration `config/kiss_icp.yaml`. Host-specific paths and CPU affinity settings have been removed from the portable reports. No machine-specific playback command is included; playback requires the original workspace and the omitted ROS bag artifacts.


