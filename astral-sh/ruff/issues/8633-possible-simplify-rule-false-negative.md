```yaml
number: 8633
title: Possible simplify rule false-negative
type: issue
state: open
author: ofek
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-12T19:12:23Z
updated_at: 2023-11-13T04:42:44Z
url: https://github.com/astral-sh/ruff/issues/8633
synced_at: 2026-01-12T15:54:48Z
```

# Possible simplify rule false-negative

---

_@ofek_

```python
@cached_property
def python_path(self) -> str:
    if self.name == '3.7':
        if sys.platform == 'win32':
            return r'python\install\python.exe'

        return 'python/install/bin/python3'

    if sys.platform == 'win32':
        return r'python\python.exe'

    return 'python/bin/python3'
```

Is that, with everything selected, supposed to be fine? I would have assumed the return would be flagged as the following recommendation:

```python
return r'python\python.exe' if sys.platform == 'win32' else 'python/bin/python3'
```

Feel free to close if this is the expected behavior!

---

_Comment by @charliermarsh on 2023-11-12 20:32_

This is expected, but I feel like it could make sense to include under one of the simplify rules? https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/

---

_Label `rule` added by @charliermarsh on 2023-11-12 20:32_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-12 20:32_

---

_Comment by @dhruvmanila on 2023-11-13 04:42_

(I think there's an open issue with a similar request but I'm unable to find it 😅)

---
