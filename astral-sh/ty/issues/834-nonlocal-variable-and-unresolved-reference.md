```yaml
number: 834
title: "`nonlocal` variable and `unresolved-reference`"
type: issue
state: closed
author: lswainemoore
labels: []
assignees: []
created_at: 2025-07-16T20:38:53Z
updated_at: 2025-07-16T20:47:46Z
url: https://github.com/astral-sh/ty/issues/834
synced_at: 2026-01-12T15:54:24Z
```

# `nonlocal` variable and `unresolved-reference`

---

_@lswainemoore_

### Summary

Running `ty` locally, I have issues with `nonlocal` variables. This code:
```
ty_test.py:

class UhOh:
    @staticmethod
    def should_work():
        foo = 1

        def bar():
            nonlocal foo
            foo += 1

        bar()
        bar()

        print(foo)

UhOh.should_work()
``` 

produces: 
```
$ uvx ty check ty_test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-reference]: Name `foo` used when not defined
  --> ty_test.py:8:13
   |
 6 |         def bar():
 7 |             nonlocal foo
 8 |             foo += 1
   |             ^^^
 9 |
10 |         bar()
   |
info: rule `unresolved-reference` is enabled by default

Found 1 diagnostic
```

with:

```
$ uvx ty --version
ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)
```

and no `ty` customization in my `pyproject.toml`

Oddly, this works fine on the playground: https://play.ty.dev/ed6ae20c-6715-4767-a744-393399f74103

### Version

ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)

---

_Comment by @carljm on 2025-07-16 20:45_

Thanks for the report! This works on the playground because the playground is automatically deployed from latest main branch and this was fixed since the last ty release. We should make another release soon!

---

_Closed by @carljm on 2025-07-16 20:45_

---

_Comment by @lswainemoore on 2025-07-16 20:47_

Oops ok great! Will stay tuned, thanks.

---
