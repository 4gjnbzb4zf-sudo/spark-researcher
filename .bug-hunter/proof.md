<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [src/spark_researcher/frontier.py:38](https://github.com/vibeforge1111/spark-researcher/blob/main/src/spark_researcher/frontier.py#L38)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-417`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Log the frontier failure instead of swallowing it

The exception handler at <code>src/spark_researcher/frontier.py:38</code> catches the error and returns a failure-shaped result without logging. If this fires in production, you'll never see it in the application's own logs — only the downstream consumer notices, usually as silent data loss.

### 🔴 Before

`src/spark_researcher/frontier.py:38`

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
            logging.getLogger(__name__).debug("frontier _parse_json candidate failed: %s", exc)
            continue
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `src/spark_researcher/frontier.py:38` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
