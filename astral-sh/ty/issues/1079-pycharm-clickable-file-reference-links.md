```yaml
number: 1079
title: "PyCharm: clickable file reference links"
type: issue
state: open
author: davidhyman
labels:
  - cli
  - diagnostics
assignees: []
created_at: 2025-08-21T10:44:33Z
updated_at: 2025-10-22T17:36:15Z
url: https://github.com/astral-sh/ty/issues/1079
synced_at: 2026-01-12T15:54:24Z
```

# PyCharm: clickable file reference links

---

_@davidhyman_

### Summary

Using `ty` in PyCharm editor window doesn't give a clickable jump-to-source file link. This makes it cumbersome to jump to the source referenced by a _diagnostic_.

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 21/21 files                                                                                                                                                error[unresolved-reference]: Name `notatype` used when not defined                                                                                                                                                               
  --> src/project/file.py:33:18
```

Seems similar to:
https://github.com/astral-sh/ruff/issues/19983

In that instance, it works (the file link is clickable):
```
ruff check --output-format=concise
src/project/file.py:33:18: F821 Undefined name `notatype`
```

However the workaround/fix of using `--output-format=concise` does not result in a working format using `ty` (the file link is still not clickable):
```
src/project/file.py:33:18: error[unresolved-reference] Name `notatype` used when not defined
```

- `ty`: `ty 0.0.1-alpha.19`
- `pycharm`: `PyCharm 2025.2.0.1`
- `ruff`: ` 0.12.9`
- `os`: `Ubuntu 24.04.3 LTS`


### Version

ty 0.0.1-alpha.19

---

_Label `server` added by @AlexWaygood on 2025-08-21 10:54_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-21 10:54_

---

_Comment by @MichaReiser on 2025-08-21 11:19_

I created an issue upstream: https://youtrack.jetbrains.com/issue/PY-83546/Ruff-File-paths-are-no-longer-clickable

> However the workaround/fix of using --output-format=concise does not result in a working format using ty (the file link is still not clickable):

I can't reproduce this part. The concise links are clickable for me:

<img width="1470" height="723" alt="Image" src="https://github.com/user-attachments/assets/2b670bd2-5b1d-4d50-8f84-b3f8658f8039" />

Edit: I guess most links are clickable but not all. Paths without a `/` segment aren't clickable

The following lines aren't clickable for reasons? 

```
stub.pyi:2:8: error[unresolved-import] Cannot resolve imported module `numpy`
stub.pyi:3:8: error[unresolved-import] Cannot resolve imported module `numpy`
yaml/yaml-stubs/__init__.pyi:1:7: error[unresolved-import] Cannot resolve imported module `.loader`
```

---

_Comment by @MichaReiser on 2025-08-21 11:23_

The `yaml/yaml-stubs/__init__.pyi` seems to be a pre-existing issue: Even with Ruff 0.11, the links aren't clickable

<img width="1470" height="723" alt="Image" src="https://github.com/user-attachments/assets/f660bb5e-2bd6-4354-9fa1-dd9143e269cd" />

---

_Label `server` removed by @MichaReiser on 2025-08-21 11:24_

---

_Label `cli` added by @MichaReiser on 2025-08-21 11:24_

---

_Comment by @InSyncWithFoo on 2025-08-27 03:04_

For the record, this is [fixed by RyeCharm](https://github.com/astral-sh/ruff/issues/19983#issuecomment-3226579399).

---

_Comment by @KotlinIsland on 2025-10-22 07:40_

will this be needed when ty is supported in pycharm 2025.3?
https://youtrack.jetbrains.com/issue/PY-81279/Support-ty-as-a-type-checker

---

_Comment by @MichaReiser on 2025-10-22 08:29_

I think that's up to you (JetBrains) on how polished the experience should be. As a user, I'd expect that I can click on links and RustRover already supports the format. 

There's also this tracking issue for Ruff https://youtrack.jetbrains.com/issue/PY-83546 (ty and Ruff use the same diagnostic format)

---

_Comment by @MichaReiser on 2025-10-22 17:36_

@KotlinIsland, please reach out (e.g discord or on github) if there's anything we can do to help with the native Ruff/ty integration

---
