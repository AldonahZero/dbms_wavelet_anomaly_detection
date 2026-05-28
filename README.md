# DBMS Wavelet Anomaly Detection

Course-project scaffold for detecting abnormal runtime behavior in DBMS fuzzing using wavelet time-frequency features and machine-learning models.

## Topic

基于小波时频特征与机器学习的DBMS模糊测试运行时异常检测研究

## Goal

The project treats DBMS fuzzing runtime logs as multi-channel non-stationary signals. Typical channels include RSS memory, SQL execution time, coverage growth, return status, timeout state, and crash state. The planned pipeline is:

```text
DBMS fuzzing logs
-> preprocessing and sliding windows
-> DWT/WPT wavelet decomposition
-> wavelet energy, energy entropy, high-frequency ratio
-> SVM / Random Forest / One-Class SVM anomaly detection
-> evaluation and paper figures
```

## Main Experiments

1. Baseline statistics vs. wavelet features.
2. DWT vs. WPT feature extraction.
3. Supervised SVM/Random Forest vs. semi-supervised One-Class SVM.
4. Feature ablation for wavelet energy, wavelet entropy, high-frequency ratio, RSS, execution time, and coverage channels.
5. Window length sensitivity for `L=32`, `L=64`, and `L=128`.

## Expected Data Columns

```text
timestamp
target_dbms
case_id
exec_time_ms
rss_mb
coverage_edges
status
```

Optional fields:

```text
cpu_percent
return_code
signal
sql_size
mutation_stage
bug_id
```

## Directory Layout

```text
.
+-- configs/
+-- data/
|   +-- raw/
|   +-- processed/
|   +-- splits/
+-- docs/
+-- outputs/
|   +-- figures/
|   +-- metrics/
|   +-- models/
+-- src/
+-- README.md
```

## Generate Synthetic Logs

For pipeline debugging before real SQLeek/DBMS fuzzing logs are available:

```bash
python3 src/generate_synthetic_fuzzing_logs.py \
  --rows 120000 \
  --dbms sqlite postgresql mysql \
  --runs-per-dbms 4
```

This creates a 120,000-row CSV with three DBMS profiles under `data/raw/`. Generated data, summaries, model outputs, and non-README Markdown notes are intentionally ignored by Git.

## Notes

The expected metrics currently used in the paper draft are placeholders for structure checking only. They must be replaced by real results after running the pipeline on actual SQLeek or DBMS fuzzing logs.
