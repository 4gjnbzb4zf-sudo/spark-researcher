<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/chips.py:428](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/chips.py#L428)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `MISS-122`
- **Category**: `missing-timeout`
- **Detector**: `missing-timeout` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Bound the chips request so it returns within a known window

The call at <code>src/spark_researcher/chips.py:428</code> has no timeout. A stalled peer hangs the caller indefinitely — which in async paths means the whole event loop, and in sync paths means a wedged process until manual restart.

### 🔴 Before

`src/spark_researcher/chips.py:428`

```python
    result = subprocess.run(
        invoked,
        cwd=str(context.chip_root),
        env=_build_hook_env(context),
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
    )
```

### 🟢 After

```python
    result = subprocess.run(
        invoked,
        cwd=str(context.chip_root),
        env=_build_hook_env(context),
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
        timeout=300,
    )
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/chips.py:428` |
| Category | `missing-timeout` |
| Severity | 🟡 MEDIUM |
| Detector | `missing-timeout` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
