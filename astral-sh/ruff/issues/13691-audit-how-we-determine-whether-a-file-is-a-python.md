```yaml
number: 13691
title: "Audit how we determine whether a file is a \"Python source file\""
type: issue
state: open
author: AlexWaygood
labels:
  - cli
assignees: []
created_at: 2024-10-09T13:29:19Z
updated_at: 2025-10-02T12:39:31Z
url: https://github.com/astral-sh/ruff/issues/13691
synced_at: 2026-01-12T15:54:53Z
```

# Audit how we determine whether a file is a "Python source file"

---

_@AlexWaygood_

I noticed in #13682 that there's some inconsistency regarding how we determine whether a file is a "Python source file" currently. In the code for `ruff server` (and the red-knot port of the server), we take care to do case-insensitive matching when figuring out whether something is a notebook file or not:

https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/ruff_server/src/session/index.rs#L124-L126

https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/red_knot_server/src/session/index.rs#L75-L79

Elsewhere, however, we mostly use case-sensitive matching:

https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/ruff_python_ast/src/lib.rs#L91-L101

https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/red_knot_python_semantic/src/module_resolver/path.rs#L41-L60

For places like the red-knot module resolver, it's likely correct to do case-sensitive matching (Python recognises `foo.py` as an importable module, but not `foo.PY`), but in other places it may not be. We should audit the code to make sure we're using case-sensitive matching and case-insensitive matching for file extensions in the correct places. We should also add comments to the places where there might be a subtle reason why case-(in)sensitive matching is required, rather than vice versa.

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-09 13:29_

---

_Comment by @MichaReiser on 2024-10-09 13:38_

> For places like the red-knot module resolver, it's likely correct to do case-sensitive matching (Python recognises foo.py as an importable module, but not foo.PY

Note: I think that depends on the file system's case sensitivity.

Other than that. I think we should ignore casing when matching files because that's what most desktop environments to. They use the same application to open a `test.jpg` or `test.JPG`. 

---

_Label `cli` added by @MichaReiser on 2024-10-09 13:38_

---

_Comment by @AlexWaygood on 2024-10-09 13:44_

Here, for example, we might want to continue to do case-sensitive matching, since the rule only applies to the names of Python modules that are intended to be importable by other Python modules: https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/ruff_linter/src/rules/pep8_naming/rules/invalid_module_name.rs#L51-L59

If a `FOO.PY` module can't be imported due to the fact that it uses a `.PY` extension on a case-sensitive file system, then the name of the file isn't really relevant to any PEP-8 concerns, as it's not a module (even if it might be a Python file). The rule is `invalid-module-name`, not `invalid-python-file-name`.

---

_Comment by @dhruvmanila on 2024-10-10 05:09_

There's also this issue https://github.com/astral-sh/ruff-vscode/issues/574 related to the server although not sure if this affects that.

---

_Comment by @hauntsaninja on 2024-10-20 05:11_

Fwiw mypy has some special handling in a few places to be case sensitive even on case insensitive file systems

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-18 11:37_

---

_Added to milestone `v0.9` by @AlexWaygood on 2024-11-18 11:37_

---

_Removed from milestone `v0.9` by @MichaReiser on 2025-01-08 13:35_

---

_Added to milestone `v.0.10` by @MichaReiser on 2025-01-08 13:35_

---

_Comment by @MichaReiser on 2025-02-28 13:31_

I think this is the relevant code in mypy:

https://github.com/python/mypy/blob/c32d11e60f04a0798292c438186199ccdec0db61/mypy/fscache.py#L185-L243

Both methods are called from within module resolution. @AlexWaygood and @carljm any chance you know how Python detects the real casing of a file in its module resolver?

---

_Comment by @MichaReiser on 2025-02-28 13:40_

I think I found it: 

https://github.com/python/cpython/blob/9f0879baf15fdcf89f1b85cc244d596d4d0f4e47/Lib/importlib/_bootstrap_external.py#L1411-L1440

It seems Python indexes module on a per directory basis and uses `listdir` to get the real casing of files.

It does seem that the extension casing is ignored, at least on windows 

https://github.com/python/cpython/blob/9f0879baf15fdcf89f1b85cc244d596d4d0f4e47/Lib/importlib/_bootstrap_external.py#L1425-L1438


Python assumes entire platforms as case sensitive or insensitive even though a mounted FAT partition on linux is case sensitive and macOs and both NTFS and macos can be configured to be case sensitive 

https://github.com/python/cpython/blob/9f0879baf15fdcf89f1b85cc244d596d4d0f4e47/Lib/importlib/_bootstrap_external.py#L54-L76

---

_Comment by @carljm on 2025-02-28 13:58_

> Python assumes entire platforms as case sensitive or insensitive

I don't think this is really true, or it's only sort of true. The platform checking code you linked does not automatically determine any default case handling. It is purely for determining whether the legacy `PYTHONCASEOK` environment variable (which makes imports  case insensitive) is available or not. Historically this was platform specific and there's no desire to make it more broadly available, because it's discouraged to use it. So some platforms just don't support that option.

The case insensitive suffix handling on Windows is a similar story. It was a legacy feature added on windows a long time ago and it is preserved on windows to avoid breakage there, but there is no desire to ever expand it to any other platform.

So in both these cases, there aren't assumptions about case sensitivity made that would cause a bug if there's a case sensitive file systems on Windows or non case sensitive file systems on Linux. There are just certain case-related legacy features that are documented as only available on certain platform(s).

I think if you ignore these two legacy features, the story for Python imports is pretty simple: imports are always case sensitive, based on needing to match what `listdir` returns. 

---

_Comment by @MichaReiser on 2025-02-28 14:04_

Oh right, there's an `os.environ` check when building the `relaxed` thingy. 

---

_Removed from milestone `v0.10` by @MichaReiser on 2025-03-10 13:58_

---

_Added to milestone `v0.11` by @MichaReiser on 2025-03-10 13:58_

---

_Removed from milestone `v0.12` by @ntBre on 2025-06-10 20:38_

---

_Added to milestone `v0.13` by @ntBre on 2025-06-10 20:38_

---

_Removed from milestone `v0.13` by @ntBre on 2025-09-05 13:06_

---

_Added to milestone `v0.14` by @ntBre on 2025-09-05 13:06_

---

_Removed from milestone `v0.14` by @ntBre on 2025-10-02 12:39_

---

_Added to milestone `v0.15` by @ntBre on 2025-10-02 12:39_

---
