```yaml
number: 19305
title: "FURB118: false positive for pytest fixture"
type: issue
state: closed
author: spaceone
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-07-13T10:12:05Z
updated_at: 2025-07-25T15:43:18Z
url: https://github.com/astral-sh/ruff/issues/19305
synced_at: 2026-01-12T15:54:56Z
```

# FURB118: false positive for pytest fixture

---

_@spaceone_

### Summary

```python
@pytest.fixture(scope='session')
def foo(bar):
    return bar['key']
```
cannot be written as `FURB118`:

`foo = pytest.fixture(scope='session')(operator.itemgetter('key'))` as the first argument of a pytest-fixture must be a keyword argument.

### Version

_No response_

---

_Comment by @MichaReiser on 2025-07-14 09:12_

Thanks. Ruff should probably ignore any decorated function (we could allow some well known decorators but I'm not sure if even that is worth it)

---

_Label `bug` added by @MichaReiser on 2025-07-14 09:12_

---

_Label `rule` added by @MichaReiser on 2025-07-14 09:12_

---

_Label `help wanted` added by @MichaReiser on 2025-07-14 09:12_

---

_Closed by @dylwil3 on 2025-07-25 15:43_

---
