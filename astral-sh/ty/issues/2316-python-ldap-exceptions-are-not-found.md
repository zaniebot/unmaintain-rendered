```yaml
number: 2316
title: python-ldap exceptions are not found
type: issue
state: closed
author: Salamandar
labels:
  - question
  - needs-info
assignees: []
created_at: 2026-01-03T13:01:15Z
updated_at: 2026-01-06T22:15:12Z
url: https://github.com/astral-sh/ty/issues/2316
synced_at: 2026-01-10T01:56:41Z
```

# python-ldap exceptions are not found

---

_Issue opened by @Salamandar on 2026-01-03 13:01_

### Summary


Using python-ldap exceptions trigger this issue:

```
error[unresolved-attribute]: Module `ldap` has no member `SERVER_DOWN`
    |
214 |         try:
215 |             result = self.con.search_s(base, ldap.SCOPE_SUBTREE, filter, attrs)
216 |         except ldap.SERVER_DOWN as e:
    |                ^^^^^^^^^^^^^^^^
217 |             raise e
218 |         except Exception as e:
```

Indeed these classes are not explicitly set, see the corresponding code there: https://github.com/python-ldap/python-ldap/blob/main/Lib/ldap/constants.py#L82

Not sure how you can fix `ty` to handle this, but mypy works properly on that.

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @AlexWaygood on 2026-01-03 13:09_

Thanks for the report!

To reproduce this issue, I created a `foo.py` file like the following:

```py
import ldap
ldap.SERVER_DOWN
```

I then ran `uvx --with=python-ldap mypy foo.py`. The output was:

```
foo.py:1: error: Skipping analyzing "ldap": module is installed, but missing library stubs or py.typed marker  [import-untyped]
foo.py:1: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
```

When you say "mypy works properly", do you have `--ignore-missing-imports` set in your mypy configuration file? Or do you have some stubs for this package also installed in your environment?

If you set `--ignore-missing-imports` in your mypy configuration, mypy will silently infer the type of all unresolved modules to `Any`. Mypy also, by default, treats all packages without a `py.typed` file as not being resolvable. So if you have `--ignore-missing-imports` set, mypy would not complain about the above snippet.

If you're looking for similar behaviour from ty, then https://github.com/astral-sh/ty/issues/2082 might be helpful for you -- it would essentially be a way of opting into mypy-like behaviour (where some modules are treated as `Any`, even if the import can be resolved) on a per-module basis.

---

_Label `question` added by @AlexWaygood on 2026-01-03 13:09_

---

_Label `needs-info` added by @AlexWaygood on 2026-01-03 13:09_

---

_Comment by @Salamandar on 2026-01-06 22:15_

> do you have --ignore-missing-imports set in your mypy configuration file

*gasp*, you're right, we explicitly ignored the ldap module.

> Mypy also, by default, treats all packages without a py.typed file as not being resolvable

That's probably the reason i get a different behaviour. Thanks for the explanation.

---

_Closed by @Salamandar on 2026-01-06 22:15_

---
