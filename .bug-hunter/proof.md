<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/beliefs.py:27](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/beliefs.py#L27)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-692`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Stop dropping beliefs errors without a log line

The exception handler at <code>src/spark_researcher/beliefs.py:27</code> catches the error and returns a failure-shaped result without logging. If this fires in production, you'll never see it in the application's own logs — only the downstream consumer notices, usually as silent data loss.

### 🔴 Before

`src/spark_researcher/beliefs.py:27`

```python
    except FileNotFoundError:
        pass
    except PermissionError:
        pass
```

### 🟢 After

```python
    except FileNotFoundError:
        pass
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/beliefs.py:27` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
Single-purpose change. Compile-verified locally; behavior unchanged for the success path.
