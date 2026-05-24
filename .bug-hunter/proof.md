<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/memory.py:51](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/memory.py#L51)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-026`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Log the memory failure instead of swallowing it

This <code>except</code> block at <code>src/spark_researcher/memory.py:51</code> drops the exception and continues. Operators investigating a failure read every log line in the path and find nothing. The clue lives only in the caller's return value, which the caller usually treats as fine.

### 🔴 Before

`src/spark_researcher/memory.py:51`

```python
    except PermissionError:
        # Windows/Obsidian can transiently hold generated docs open. Keep going;
        # later writes will refresh files that still exist.
        return
```

### 🟢 After

```python
    except PermissionError as exc:
        # Windows/Obsidian can transiently hold generated docs open. Keep going;
        # later writes will refresh files that still exist.
        logging.getLogger(__name__).warning("Could not unlink %s: %s", path, exc)
        return
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/memory.py:51` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
