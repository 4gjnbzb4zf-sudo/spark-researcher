<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/trainers.py:81](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/trainers.py#L81)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `MISS-463`
- **Category**: `missing-timeout`
- **Detector**: `missing-timeout` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Add a timeout to the trainers call so a stalled peer can't hang the caller

The call at <code>src/spark_researcher/trainers.py:81</code> has no timeout. A stalled peer hangs the caller indefinitely — which in async paths means the whole event loop, and in sync paths means a wedged process until manual restart.

### 🔴 Before

`src/spark_researcher/trainers.py:81`

```python
    process = subprocess.run(
        spec.compile_command,
        cwd=str(project_root),
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
    )
```

### 🟢 After

```python
    process = subprocess.run(
        spec.compile_command,
        cwd=str(project_root),
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
        timeout=600,
    )
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/trainers.py:81` |
| Category | `missing-timeout` |
| Severity | 🟡 MEDIUM |
| Detector | `missing-timeout` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
