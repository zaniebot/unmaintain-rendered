```yaml
number: 22548
title: How to resolve suspicious-url-open-usage when passing a urllib.request.Request to urllib.request.urlopen
type: issue
state: open
author: Jerakin
labels:
  - question
assignees: []
created_at: 2026-01-13T13:03:02Z
updated_at: 2026-01-14T12:19:10Z
url: https://github.com/astral-sh/ruff/issues/22548
synced_at: 2026-01-14T12:43:48Z
```

# How to resolve suspicious-url-open-usage when passing a urllib.request.Request to urllib.request.urlopen

---

_@Jerakin_

### Summary

[suspicious-url-open-usage](https://docs.astral.sh/ruff/rules/suspicious-url-open-usage/)

Ruff says I need to check the validity of the url when using request.urlopen

However, how do I do that when passing in a Request object rather than a str url?

```python
from urllib import request

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

_Comment by @ntBre on 2026-01-13 15:49_

Is `S310` triggering on this code? I tried it in the [playground](https://play.ruff.rs/901302df-99fd-4221-9982-c67a06198129), and I'm not seeing any diagnostics. I could be missing a setting or something, though.

Based on a quick look at the rule implementation, I think passing a string literal to `request.Request` should be enough to mark the URL as valid.

---

_Label `question` added by @ntBre on 2026-01-13 15:49_

---

_Comment by @Jerakin on 2026-01-14 08:39_

Yeah. I get the warning.

Using ruff through PyCharm.

<img width="432" height="259" alt="Image" src="https://github.com/user-attachments/assets/c75fd303-f919-4a09-9a9f-65cfca071c0b" />

PyCharm Professional 2025.3.1
Ruff used through the new "Python -> Tools -> Ruff" settings.
Pointing to Ruff on my path `ruff 0.14.11`


---

_Comment by @MichaReiser on 2026-01-14 09:11_

@ntBre it requires the urllib import 

https://play.ruff.rs/d389748f-f4d1-4a56-bea5-298c0f049c44

---

_Comment by @Jerakin on 2026-01-14 12:18_

That's seems to be the issue.
Not using a `from urllib` pattern makes it work.

```python
import urllib

req = urllib.request.Request(
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
