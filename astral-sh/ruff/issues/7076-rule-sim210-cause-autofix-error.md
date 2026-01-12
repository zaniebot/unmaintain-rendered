```yaml
number: 7076
title: Rule SIM210 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T09:35:43Z
updated_at: 2023-09-03T22:39:24Z
url: https://github.com/astral-sh/ruff/issues/7076
synced_at: 2026-01-12T15:54:46Z
```

# Rule SIM210 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select SIM210
```

file content:
```
def subresource_integrity(reqs: dict, expectation='sri-implemented-and-external-scripts-loaded-securely') -> dict:
    output = {
    }
    if response.headers.get('Content-Type', '').split(';')[0] not in HTML_TYPES:
        try:
            soup = bs(reqs['resources']['__path__'], 'html.parser')
        except:
            output['result'] = 'html-not-parsable'
        for script in scripts:
                samesld = True if (psl.privatesuffix(urlparse(response.url).netloc) ==
                                   psl.privatesuffix(src.netloc)) else False
                if src.scheme == '':
                    securescheme = True
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `7675231.py`, the rule codes SIM210, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

7675231.py:10:27: SIM210 Use `bool(psl.privatesuffix(urlparse(response.url).netloc) == psl.privatesuffix(src.netloc))` instead of `True if psl.privatesuffix(urlparse(response.url).netloc) == psl.privatesuffix(src.netloc) else False`
Found 1 error.

```

[7675231.py.zip](https://github.com/astral-sh/ruff/files/12505635/7675231.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 12:01_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 22:09_

---

_Closed by @charliermarsh on 2023-09-03 22:39_

---
