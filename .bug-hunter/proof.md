<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/self_edit.py:90](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/self_edit.py#L90)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `MISS-740`
- **Category**: `missing-timeout`
- **Detector**: `missing-timeout` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Bound the self edit request so it returns within a known window

The call at <code>src/spark_researcher/self_edit.py:90</code> has no timeout. A stalled peer hangs the caller indefinitely — which in async paths means the whole event loop, and in sync paths means a wedged process until manual restart.

### 🔴 Before

`src/spark_researcher/self_edit.py:90`

```python
        ["git", "-C", str(repo_root), "status", "--porcelain", "--untracked-files=no"],
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
    )
```

### 🟢 After

```python
        ["git", "-C", str(repo_root), "status", "--porcelain", "--untracked-files=no"],
        capture_output=True,
        text=True,
        encoding="utf-8",
        errors="replace",
        timeout=30,
    )
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/self_edit.py:90` |
| Category | `missing-timeout` |
| Severity | 🟡 MEDIUM |
| Detector | `missing-timeout` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
