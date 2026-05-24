<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/memory.py:49](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/memory.py#L49)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-479`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Log the memory failure instead of swallowing it

This <code>except</code> block at <code>src/spark_researcher/memory.py:49</code> drops the exception and continues. Operators investigating a failure read every log line in the path and find nothing. The clue lives only in the caller's return value, which the caller usually treats as fine.

### 🔴 Before

`src/spark_researcher/memory.py:49`

```python
    except PermissionError:
        # Windows/Obsidian can transiently hold generated docs open. Keep going;
        # later writes will refresh files that still exist.
        return
```

### 🟢 After

```python
    except PermissionError:
        # Windows/Obsidian can transiently hold generated docs open. Keep going;
        # later writes will refresh files that still exist.
        import warnings
        warnings.warn(f"could not unlink {path}: still in use", stacklevel=2)
        return
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/memory.py:49` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
