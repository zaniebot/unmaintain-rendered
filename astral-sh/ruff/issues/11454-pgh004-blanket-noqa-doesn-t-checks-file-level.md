```yaml
number: 11454
title: "PGH004 (blanket-noqa) doesn't checks file-level pragmas"
type: issue
state: closed
author: ashrub-holvi
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-05-17T07:25:05Z
updated_at: 2024-06-04T03:43:00Z
url: https://github.com/astral-sh/ruff/issues/11454
synced_at: 2026-01-12T15:54:51Z
```

# PGH004 (blanket-noqa) doesn't checks file-level pragmas

---

_@ashrub-holvi_

Hi,

If blanket `noqa` is used on a file level, `cat test.py`:
```
# ruff: noqa

print("Hello Astral!")
```
ruff will not complain about it:
```
ruff check --select=PGH004 test.py
All checks passed!
```
but probably it should, because in any case better to disable exact rules, not everything.
Same for `# flake8: noqa` which is also supported by ruff.

```
ruff --version
ruff 0.4.4
```


---

_Label `rule` added by @charliermarsh on 2024-05-17 14:02_

---

_Comment by @charliermarsh on 2024-05-18 17:31_

Seems reasonable to me? May need to be added in preview since it's an increase in the scope of the rule.

---

_Label `help wanted` added by @zanieb on 2024-05-18 17:32_

---

_Closed by @charliermarsh on 2024-06-04 03:43_

---
