# Artifact contract

This document defines the stable interface of the public GPS + KISS-ICP evidence export. It describes what is present in Git, how to interpret it, and which ROS 2 interfaces produced the reports. It does not claim that the exported files form a runnable ROS 2 package.

## Run identity

- Method: KISS-ICP.
- ROS distribution used for the historical run: ROS 2 Humble.
- Run name: `gps_kiss_demo_20261010T065718_20272`.
- Recorded interval: `0` to `599.991781259` seconds.
- Lidar input topic: `/rslidar_points`.
- IMU: not used (`imu_used: false`).
- Source revision: `b16835283aee62f7d5e2bdf6c1c3bb2930de74ff`.

The portable manifest keeps workspace-relative names for the source bag (`data/sampled_bag`) and configuration (`config/kiss_icp.yaml`). Those inputs are not included in this repository.

## ROS 2 message evidence

The omitted evidence bag was checked for the following topics and counts:

| Topic | Count | Interpretation |
| --- | ---: | --- |
| `/fix` | 600 | GPS fixes used for the independent comparison. |
| `/gps/path` | 600 | GPS path samples. |
| `/kiss/odometry` | 2,994 | KISS-ICP odometry samples. |
| `/kiss/path` | 2,994 | KISS-ICP path samples. |
| `/kiss/map_chunks` | 16 | Published map chunks. |

These counts are preserved in `reports/evidence_validation.json`; the message payloads and bag are intentionally excluded.

## Coordinate and time conventions

- `artifacts/trajectory.tum` follows the TUM trajectory layout: `timestamp tx ty tz qx qy qz qw`.
- TUM translations are in the KISS local Cartesian frame. The export does not preserve ROS `frame_id` metadata, so consumers must not infer a globally fixed frame from the file alone.
- The CSV files use local ENU-style columns: `east_m`, `north_m`, `step_m`, `cumulative_m`, `dt_s`, `speed_mps`, and `gap_over_2_5s`.
- `bag_relative_s` is the sample time relative to the recorded interval. The first CSV row is the reference sample and has zero cumulative distance.
- GPS and KISS traces have independent sample times and are compared after display-only alignment. Cross-sensor timing is approximate.

## Comparison semantics

GPS is not fused into KISS-ICP. The comparison applies a 2D yaw and translation fit on the first half of the interval, then evaluates the held-out second half. The public summary reports:

- GPS horizontal distance: `1131.8838424444079 m`.
- KISS horizontal distance: `1081.9940952420779 m`.
- Held-out horizontal RMSE: `9.196128083640904 m`.

These values are comparison metrics for this recording, not a ground-truth accuracy claim. The run contains `2994` source clouds and matched poses, `600` GPS fixes, `16` map chunks, and `144874377` map points.

## Validation and release boundary

`reports/trajectory_check.txt` is the human-readable integrity receipt. It records finite pose columns, strictly increasing timestamps, sufficient duration, bounded timestamp gaps, bounded pose steps, and normalized quaternions. `reports/evidence_validation.json` records that the evidence checks passed and that GPS coordinates are not included in the public summary.

Raw bags, maps, point clouds, and build products stay outside this repository. The root `.gitignore` guards common ROS recording and map extensions (`*.db3`, `*.mcap`, `*.pcd`, `*.bag`, and `*.bag2`).
