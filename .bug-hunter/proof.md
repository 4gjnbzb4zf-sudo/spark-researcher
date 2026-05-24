<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/self_edit.py:336](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/self_edit.py#L336)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `MISS-568`
- **Category**: `missing-timeout`
- **Detector**: `missing-timeout` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Add a timeout to the self edit call so a stalled peer can't hang the caller

The call at <code>src/spark_researcher/self_edit.py:336</code> has no timeout. A stalled peer hangs the caller indefinitely — which in async paths means the whole event loop, and in sync paths means a wedged process until manual restart.

### 🔴 Before

`src/spark_researcher/self_edit.py:336`

```python
        process = subprocess.run(command, cwd=str(workspace_root), capture_output=True, text=True, encoding="utf-8", errors="replace")
```

### 🟢 After

```python
        process = subprocess.run(command, cwd=str(workspace_root), capture_output=True, text=True, encoding="utf-8", errors="replace", timeout=config.self_edit.command_timeout_seconds)
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/self_edit.py:336` |
| Category | `missing-timeout` |
| Severity | 🟡 MEDIUM |
| Detector | `missing-timeout` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
