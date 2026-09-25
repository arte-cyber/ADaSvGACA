# ADaS 2.0 Complete v0.3

This package provides the Blue Team ADaS 2.0 stage-mapping layer.

## How to run
From the package root:

```bash
python scripts/adas2_v0_3_complete_pipeline.py --input-dir inputs --output-dir outputs
```

## Inputs included
- `inputs/author2_seed`: Author 2's original lab seed files, including `vm2log.txt`
- `inputs/redteam_demo1`: Red Team Demo 1 artifacts
- `inputs/redteam_demo2`: Red Team Demo 2 artifacts

## Key note
Author 2's seed lab files already included `vm2log.txt`, so VM2 host-log analysis has been part of ADaS 2.0 from the start. If red team can locate or export VM2 logs from the final run, place them as `vm2log.txt` beside `vm1log.txt`, `vm3log.txt`, and `capture.pcap`, then rerun this script.

## Outputs
- `outputs/adas2_stage_alerts.csv`
- `outputs/adas2_stage_summary.csv`
- `outputs/adas2_comparison_table.csv`
- `outputs/adas2_run_notes.md`
