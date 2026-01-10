```yaml
number: 2398
title: Please support imports from zip
type: issue
state: open
author: mitya57
labels:
  - wish
  - imports
assignees: []
created_at: 2026-01-08T15:59:52Z
updated_at: 2026-01-08T16:09:29Z
url: https://github.com/astral-sh/ty/issues/2398
synced_at: 2026-01-10T01:56:41Z
```

# Please support imports from zip

---

_Issue opened by @mitya57 on 2026-01-08 15:59_

### Summary

Python has a feature of importing code from ZIP files.

Here is a recipe where Python can successfully import `foo` module from `foo.zip`:
```bash
dmitry@l3:/tmp/playground$ mkdir foo
dmitry@l3:/tmp/playground$ echo 'a = 42' > foo/__init__.py
dmitry@l3:/tmp/playground$ zip foo.zip foo/__init__.py
  adding: foo/__init__.py (stored 0%)
dmitry@l3:/tmp/playground$ rm -rf foo
dmitry@l3:/tmp/playground$ echo -e 'import foo\nprint(foo.a)' > main.py
dmitry@l3:/tmp/playground$ PYTHONPATH=./foo.zip python3 ./main.py 
42
```

However, with the same `PYTHONPATH`, ty fails to resolve this import:
```bash
dmitry@l3:/tmp/playground$ PYTHONPATH=./foo.zip ty check ./main.py 
error[unresolved-import]: Cannot resolve imported module `foo`
 --> main.py:1:8
  |
1 | import foo
  |        ^^^
2 | print(foo.a)
  |
info: Searched in the following paths during module resolution:
info:   1. /tmp/playground (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.10

---

_Label `imports` added by @AlexWaygood on 2026-01-08 16:01_

---

_Comment by @carljm on 2026-01-08 16:04_

Thanks for the suggestion! Does any other type checker support this?

---

_Label `wish` added by @carljm on 2026-01-08 16:04_

---

_Comment by @mitya57 on 2026-01-08 16:09_

mypy doesn't work.

But pyright works:
```bash
dmitry@l3:/tmp/playground$ pyright ./main.py 
/tmp/playground/main.py
  /tmp/playground/main.py:1:8 - error: Import "foo" could not be resolved (reportMissingImports)
1 error, 0 warnings, 0 informations
dmitry@l3:/tmp/playground$ PYTHONPATH=./foo.zip pyright ./main.py 
0 errors, 0 warnings, 0 informations
```

---
