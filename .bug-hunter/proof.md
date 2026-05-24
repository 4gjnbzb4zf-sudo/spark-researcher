<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/verifier.py:90](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/verifier.py#L90)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-720`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Log the verifier failure instead of swallowing it

This <code>except</code> block at <code>src/spark_researcher/verifier.py:90</code> drops the exception and continues. Operators investigating a failure read every log line in the path and find nothing. The clue lives only in the caller's return value, which the caller usually treats as fine.

### 🔴 Before

`src/spark_researcher/verifier.py:90`

```python
        try:
            payload = json.loads(candidate)
        except json.JSONDecodeError:
            continue
```

### 🟢 After

```python
        try:
            payload = json.loads(candidate)
        except json.JSONDecodeError as exc:
            logging.getLogger(__name__).debug("Failed to parse JSON candidate: %s", exc)
            continue
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/verifier.py:90` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
