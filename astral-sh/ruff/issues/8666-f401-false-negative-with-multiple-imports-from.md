```yaml
number: 8666
title: "F401: False negative with multiple imports from same top-level package"
type: issue
state: closed
author: ThiefMaster
labels: []
assignees: []
created_at: 2023-11-13T23:05:52Z
updated_at: 2023-11-14T00:12:19Z
url: https://github.com/astral-sh/ruff/issues/8666
synced_at: 2026-01-12T15:54:48Z
```

# F401: False negative with multiple imports from same top-level package

---

_@ThiefMaster_

```python
def _import_modules():
    import foo.a
    import foo.b
    import bar
```

```
$ ruff check --isolated --select F401 --no-cache rufftest/ruff_sample.py
rufftest/ruff_sample.py:3:12: F401 [*] `foo.b` imported but unused
rufftest/ruff_sample.py:4:12: F401 [*] `bar` imported but unused
Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

Tested with the latest master (0.1.5 + a few days worth of commits).

It looks like it only shows an unused import for the very last import from any given top-level package.

While I believe this will never result in a case where there is no warning at all in case of unused imports, it is annoying when combined with `RUF100` because now it asks me to remove the `noqa` from lines that are in fact unused imports.

---

_Comment by @ThiefMaster on 2023-11-14 00:03_

I looked into this a bit and it looks like this is happening because both get added using `add_binding("foo")`, so the second one shadows the first one. No idea how to fix it though...

---

_Comment by @charliermarsh on 2023-11-14 00:12_

There's a bit of history here:
- https://github.com/astral-sh/ruff/issues/60
- https://github.com/astral-sh/ruff/issues/7972
- https://github.com/astral-sh/ruff/pull/5011

I'm going to close as a duplicate of https://github.com/astral-sh/ruff/issues/60 for now. Submodule imports are really, really confusing and I haven't seen a proposal yet that correctly handles all the cases.


---

_Closed by @charliermarsh on 2023-11-14 00:12_

---
