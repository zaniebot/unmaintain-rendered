```yaml
number: 22548
title: How to resolve suspicious-url-open-usage when passing a urllib.request.Request to urllib.request.urlopen
type: issue
state: open
author: Jerakin
labels: []
assignees: []
created_at: 2026-01-13T13:03:02Z
updated_at: 2026-01-13T13:03:02Z
url: https://github.com/astral-sh/ruff/issues/22548
synced_at: 2026-01-13T13:22:39Z
```

# How to resolve suspicious-url-open-usage when passing a urllib.request.Request to urllib.request.urlopen

---

_@Jerakin_

### Summary

[suspicious-url-open-usage](https://docs.astral.sh/ruff/rules/suspicious-url-open-usage/)

Ruff says I need to check the validity of the url when using request.urlopen

However, how do I do that when passing in a Request object rather than a str url?

```python
req = request.Request(
    "http://example.com",
    data={},
    headers={"Content-Type": "application/json"},
)
with (
    request.urlopen(req, timeout=0.05) as resp,
):
    ...
```

---
