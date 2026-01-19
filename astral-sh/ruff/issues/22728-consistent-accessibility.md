```yaml
number: 22728
title: Consistent accessibility
type: issue
state: open
author: AndreuCodina
labels: []
assignees: []
created_at: 2026-01-19T17:23:50Z
updated_at: 2026-01-19T17:25:41Z
url: https://github.com/astral-sh/ruff/issues/22728
synced_at: 2026-01-19T17:26:04Z
```

# Consistent accessibility

---

_@AndreuCodina_

### Summary

A rule should prevent public functions that return **private classes**.

For example, the following code has a public class with a public method returning a private class.

```python
class _Array:
    pass


class Numpy:
    def create_array(self) -> _Array:
        return _Array()
```

That forces you to import a private class.

In C#:

<img width="779" height="298" alt="Image" src="https://github.com/user-attachments/assets/55a0e424-8cc3-48e9-af84-00073f069d46" />

**Private modules and folders** should also be covered. For example, in my libraries, I have lots of files/folders prefixed with an underscore (`_async_concurrent_dictionary.py`, `_service_lookup/_service_provider_engine.py`...), containing classes without an underscore. This can be considered the equivalent of `internal` of C#.
This is important because I import those classes from `/src` and `/tests`, but I don't want user libraries to see or import the code.
This issue is related to this topic: https://github.com/astral-sh/ruff/issues/16892

**Aliases** should be covered too (https://github.com/astral-sh/ruff/issues/20916).

---
