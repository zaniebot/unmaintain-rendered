```yaml
number: 22548
title: How to resolve suspicious-url-open-usage when passing a urllib.request.Request to urllib.request.urlopen
type: issue
state: closed
author: Jerakin
labels:
  - question
assignees: []
created_at: 2026-01-13T13:03:02Z
updated_at: 2026-01-14T21:19:26Z
url: https://github.com/astral-sh/ruff/issues/22548
synced_at: 2026-01-14T21:42:56Z
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

_Comment by @ntBre on 2026-01-14 14:37_

> [@ntBre](https://github.com/ntBre) it requires the urllib import

Ah of course, thanks!

@Jerakin Unless I'm missing something else, your last snippet only appears to work in Ruff because `urllib.request` isn't imported:

```pycon
>>> import urllib
...
... req = urllib.request.Request(
...     "http://example.com",
...     data={},
...     headers={"Content-Type": "application/json"},
... )
... with (
...     request.urlopen(req, timeout=0.05) as resp,
... ):
...     ...
...
Traceback (most recent call last):
  File "<python-input-0>", line 3, in <module>
    req = urllib.request.Request(
          ^^^^^^^^^^^^^^
AttributeError: module 'urllib' has no attribute 'request'
```

Fixing the import like in Micha's playground example reintroduces the error. One workaround, which may not be practical in your case, is to use the `Request` directly in the `urlopen` call. I think that's what #10964 implemented. It seems to work in the [playground](https://play.ruff.rs/24fd11eb-a7d9-4cfe-8b7a-09f5466e4751).

Otherwise I think we would have to extend the rule to try to resolve the value of the `req` argument. In which case, this may be a duplicate of https://github.com/astral-sh/ruff/issues/7918.


---

_Comment by @Jerakin on 2026-01-14 21:19_

Looking at the comments in that issue I would treat it as a duplicate of https://github.com/astral-sh/ruff/issues/7918

---

_Closed by @Jerakin on 2026-01-14 21:19_

---
