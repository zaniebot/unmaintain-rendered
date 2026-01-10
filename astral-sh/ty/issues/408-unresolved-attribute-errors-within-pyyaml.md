```yaml
number: 408
title: "\"unresolved-attribute\" errors within pyyaml"
type: issue
state: closed
author: rofafor
labels:
  - imports
assignees: []
created_at: 2025-05-15T13:22:25Z
updated_at: 2025-05-16T13:30:12Z
url: https://github.com/astral-sh/ty/issues/408
synced_at: 2026-01-10T02:34:09Z
```

# "unresolved-attribute" errors within pyyaml

---

_Issue opened by @rofafor on 2025-05-15 13:22_

### Summary

Package: [pyyaml](https://github.com/yaml/pyyaml) 6.0.2

Code:
```python
import yaml

malformed_yaml = """
a:
 - 1
 b: 2
"""

try:
    config = yaml.load(malformed_yaml, Loader=yaml.FullLoader)
except yaml.YAMLError as exc:
    print(exc)
```

Output:
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'yaml'>` has no attribute `FullLoader`
  --> foo.py:10:47
   |
 9 | try:
10 |     config = yaml.load(malformed_yaml, Loader=yaml.FullLoader)
   |                                               ^^^^^^^^^^^^^^^
11 | except yaml.YAMLError as exc:
12 |     print(exc)
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `<module 'yaml'>` has no attribute `YAMLError`
  --> foo.py:11:8
   |
 9 | try:
10 |     config = yaml.load(malformed_yaml, Loader=yaml.FullLoader)
11 | except yaml.YAMLError as exc:
   |        ^^^^^^^^^^^^^^
12 |     print(exc)
   |
info: rule `unresolved-attribute` is enabled by default

Found 2 diagnostics
```

### Version

ty 0.0.1-alpha.3 (144a26d44 2025-05-15)

---

_Comment by @sharkdp on 2025-05-15 13:27_

Thank you for reporting this.

Can you say a bit more how you installed `PyYAML` and how you're running ty? I can not reproduce this. It runs without errors on your file for me.

---

_Label `needs-mre` added by @MichaReiser on 2025-05-15 13:28_

---

_Comment by @rofafor on 2025-05-15 15:53_

Sorry for the mess up! I was running this test code on my actual project rather than a new bootstrapped project. It seems that `types-pyyaml` package is the culprit here:

```
% uv init && uv add ty pyyaml

% uv run ty version
ty 0.0.1-alpha.3 (144a26d44 2025-05-15)

% uv run ty check foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!

% uv add types-pyyaml && uv run ty check foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'yaml'>` has no attribute `FullLoader`
  --> /Users/rofa/Temp/foo.py:10:47
   |
 9 | try:
10 |     config = yaml.load(malformed_yaml, Loader=yaml.FullLoader)
   |                                               ^^^^^^^^^^^^^^^
11 | except yaml.YAMLError as exc:
12 |     print(exc)
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `<module 'yaml'>` has no attribute `YAMLError`
  --> /Users/rofa/Temp/foo.py:11:8
   |
 9 | try:
10 |     config = yaml.load(malformed_yaml, Loader=yaml.FullLoader)
11 | except yaml.YAMLError as exc:
   |        ^^^^^^^^^^^^^^
12 |     print(exc)
   |
info: rule `unresolved-attribute` is enabled by default

Found 2 diagnostics
```

---

_Label `needs-mre` removed by @sharkdp on 2025-05-15 18:02_

---

_Label `imports` added by @sharkdp on 2025-05-15 18:02_

---

_Comment by @sharkdp on 2025-05-15 18:21_

Thank you!

I can reproduce this. It looks like we have a bug in our stubs-packages support, because I can not reproduce the same error if it's a non-stub package.

MRE: https://play.ty.dev/54434464-27dd-436d-9b80-97aa0aa63316
Same example, non-stubs package: https://play.ty.dev/a15123bd-d02c-48f5-955e-dd8c3493f8c7

In fact, the playground gives us a hint where the problem may lie: we don't seem to understand the relative import from `.loader` inside `yaml-stubs/__init__.pyi`.

I also added a regression test here: https://github.com/astral-sh/ruff/pull/18123

---

_Comment by @MichaReiser on 2025-05-16 07:07_

The interesting message from the logs is:

```
2025-05-16 09:06:46.69137 DEBUG Relative module resolution `.loader` failed; could not resolve file `/Users/micha/astral/test/yaml/yaml-stubs/__init__.pyi` to a module
```

---

_Comment by @MichaReiser on 2025-05-16 07:17_

The problem is that this call here fails:

https://github.com/astral-sh/ruff/blob/6b64630635688429bb16a1fad5ca5fb8eebd375c/crates/ty_python_semantic/src/module_resolver/resolver.rs#L112

because `xy-stubs` is not a valid module name. 

We need to remove the `-stubs` extension (but only in the first component) similar to what we do in `RelaxedModuleName`. Maybe we could add a `to_module_name` method to `RelaxedModuleName`

---

_Assigned to @MichaReiser by @MichaReiser on 2025-05-16 13:21_

---

_Closed by @MichaReiser on 2025-05-16 13:30_

---
