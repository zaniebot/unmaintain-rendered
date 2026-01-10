---
number: 7074
title: Rule UP032 cause autofix error
type: issue
state: open
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T09:27:55Z
updated_at: 2023-09-03T13:28:28Z
url: https://github.com/astral-sh/ruff/issues/7074
synced_at: 2026-01-10T01:22:46Z
---

# Rule UP032 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 09:27_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select UP032
```

file content:
```
class TrainLog(object):
                    pbar += '{}={.3f} '.format(metric_name, metric[0])
                    if len(metric) == 2:
                        pbar += 'valid_{}={:.3f} '.format(metric_name,
                                                          valid_metric)
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `577788.py`, the rule codes UP032, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

577788.py:2:29: UP032 Use f-string instead of `format` call
577788.py:4:33: UP032 Use f-string instead of `format` call
Found 2 errors.
```
[577788.py.zip](https://github.com/astral-sh/ruff/files/12505625/577788.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 12:01_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:01_

---

_Comment by @charliermarsh on 2023-09-03 13:28_

`'{}={.3f} '.format(metric_name, metric[0])` isn't valid but fails at runtime, not at parse time, whereas `f'{metric_name}={metric[0].3f} '` fails at parse time apparently.

---

_Referenced in [astral-sh/ruff#15874](../../astral-sh/ruff/issues/15874.md) on 2025-02-01 19:14_

---
