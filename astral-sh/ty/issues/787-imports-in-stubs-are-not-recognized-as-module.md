```yaml
number: 787
title: Imports in stubs are not recognized as module attributes
type: issue
state: closed
author: zeevro
labels:
  - imports
assignees: []
created_at: 2025-07-09T08:36:43Z
updated_at: 2025-07-14T17:14:12Z
url: https://github.com/astral-sh/ty/issues/787
synced_at: 2026-01-10T02:07:36Z
```

# Imports in stubs are not recognized as module attributes

---

_Issue opened by @zeevro on 2025-07-09 08:36_

### Summary

```console
$ cat mymod.pyi
import json

$ cat test.py
import mymod

print(mymod.json.dumps({}))

$ ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'mymod'>` has no attribute `json`
 --> test.py:3:7
  |
1 | import mymod
2 |
3 | print(mymod.json.dumps({}))
  |       ^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic

$ mv mymod.pyi mymod.py

$ ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```


### Version

ty 0.0.1-alpha.14

---

_Label `imports` added by @sharkdp on 2025-07-10 08:16_

---

_Comment by @sharkdp on 2025-07-10 08:18_

Thank you for reporting this. It looks related to https://github.com/astral-sh/ty/issues/133. Given that this ticket is not resolved, I am more surprised by the fact that it works for non-stubs, then by the fact that it fails for stubs...

---

_Comment by @AlexWaygood on 2025-07-14 17:14_

Unfortunately this is unrelated to #133: this is the [re-export convention](https://typing.python.org/en/latest/spec/distributing.html#import-conventions) for stub files. All _imported_ symbols in a stub file `foo.pyi` are considered private by default unless they are included in `foo.__all__`, they were imported into `foo.pyi` using a `*` import, or they are explicitly re-exported in `foo.pyi` using a "redundant alias" (here that would be `import json as json` rather than just `import json`). If we did not follow the re-export convention as laid out by the typing spec, we would think that many objects exist at runtime despite the fact that they were only imported into stub files to provide accurate type hints (we used to think that Python had a `Literal` builtin symbol, because typeshed imports `typing.Literal` in its stubs for the `builtins` module!).

---

_Closed by @AlexWaygood on 2025-07-14 17:14_

---
