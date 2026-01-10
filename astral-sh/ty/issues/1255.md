```yaml
number: 1255
title: "error[unresolved-attribute]: Type `<module 'sqlalchemy'>` has no attribute `types`"
type: issue
state: closed
author: benglewis
labels: []
assignees: []
created_at: 2025-09-25T15:25:10Z
updated_at: 2025-09-25T15:56:13Z
url: https://github.com/astral-sh/ty/issues/1255
synced_at: 2026-01-10T02:06:25Z
```

# error[unresolved-attribute]: Type `<module 'sqlalchemy'>` has no attribute `types`

---

_Issue opened by @benglewis on 2025-09-25 15:25_

### Summary

SQLAlchemy does actually have a module named `types`, which should be accessible when using `sa.types`, e.g. here is some valid code that shows this error

```python
import sqlalchemy as sa

print(sa.types)
```


### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @benglewis on 2025-09-25 15:25_

```pycon
Python 3.13.5 | packaged by conda-forge | (main, Jun 16 2025, 08:24:05) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlalchemy as sa
>>> sa.types
<module 'sqlalchemy.types' from '.../.pixi/envs/dev/lib/python3.13/site-packages/sqlalchemy/types.py'>
>>>
```

Just to show that it does exist

---

_Comment by @carljm on 2025-09-25 15:56_

Thanks for the report! This is a case of #133 

---

_Closed by @carljm on 2025-09-25 15:56_

---
