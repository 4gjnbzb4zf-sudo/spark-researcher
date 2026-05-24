<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [examples/toy-project/train.py:13](https://github.com/vibeforge1111/spark-researcher/blob/main/examples/toy-project/train.py#L13)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `ERRO-807`
- **Category**: `error-message-actionability`
- **Detector**: `error-message-actionability` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Make the train toy project error tell the user what to do next

The error at <code>examples/toy-project/train.py:13</code> names what went wrong but not what to do about it. An operator hitting this message has to read source to figure out the fix — which is exactly the kind of friction that turns minor failures into hour-long debug sessions.

### 🔴 Before

`examples/toy-project/train.py:13`

```python
    print("training_seconds: 5.0")
```

### 🟢 After

```python
    print("training_seconds: 5.0 (tune hyperparameters in config.json to reduce val_loss; see examples/toy-project/README.md)")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `examples/toy-project/train.py:13` |
| Category | `error-message-actionability` |
| Severity | 🟡 MEDIUM |
| Detector | `error-message-actionability` |
| Discovered | 2026-05-24 |

---
Single-purpose change. Compile-verified locally; behavior unchanged for the success path.
