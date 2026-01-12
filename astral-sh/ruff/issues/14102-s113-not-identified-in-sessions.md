```yaml
number: 14102
title: "S113: Not identified in sessions"
type: issue
state: open
author: kiblik
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-11-05T10:41:51Z
updated_at: 2024-11-06T04:46:59Z
url: https://github.com/astral-sh/ruff/issues/14102
synced_at: 2026-01-12T15:54:53Z
```

# S113: Not identified in sessions

---

_@kiblik_

S113 can idenify if request is missing timeout and it runs "natively". However, it does not identify run from existing sessions.

## code

```python
import requests

requests.get("https://example.com")  # violation identified - Good
requests.get("https://example.com", timeout=5)  # correct implementation

s = requests.session()
s.get("https://example.com")  # violation NOT identified - Bad
s.get("https://example.com", timeout=5)  # correct implementation
```

## config

```toml
[lint]
select = ["S113"]
```

## version

```bash
$ ruff --version
ruff 0.7.2
```


---

_Comment by @dhruvmanila on 2024-11-06 04:46_

Thanks for the issue. I don't think this is currently possible as Ruff needs to know the type of symbol `s` here. We're currently working on it (Red Knot project) but it'll take some time before it can be utilized.

---

_Label `bug` added by @dhruvmanila on 2024-11-06 04:46_

---

_Label `type-inference` added by @dhruvmanila on 2024-11-06 04:46_

---
