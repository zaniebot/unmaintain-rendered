```yaml
number: 547
title: "ty not using `PYTHONPATH` and `PYTHONSAFEPATH`"
type: issue
state: closed
author: yoursvivek
labels:
  - imports
  - runtime semantics
assignees: []
created_at: 2025-05-30T08:21:04Z
updated_at: 2025-09-20T11:40:11Z
url: https://github.com/astral-sh/ty/issues/547
synced_at: 2026-01-10T02:06:24Z
```

# ty not using `PYTHONPATH` and `PYTHONSAFEPATH`

---

_Issue opened by @yoursvivek on 2025-05-30 08:21_

### Summary

`ty` uses `--extra-search-path` to find addition paths to search for python modules but it seems to ignore site paths defined by standard python variables `PYTHONPATH`[^1] and `PYTHONSAFEPATH`[^2].

It would be nice to have ty honor these and avoid endusers from configuring these twice.

[^1]: https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH
[^2]: https://docs.python.org/3/using/cmdline.html#envvar-PYTHONSAFEPATH

### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Label `imports` added by @AlexWaygood on 2025-05-30 08:22_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-30 08:22_

---

_Comment by @AlexWaygood on 2025-05-30 08:28_

It might make sense for us to add support for these search-path sources (especially if other type checkers do), but note that they are not mentioned in https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering. Ideally we should also try to update the spec if we make this change.

---

_Comment by @yoursvivek on 2025-05-30 12:29_

I think it should be implied that if python runtime is able to use those packages/modules, so should type checker. Admittedly, I'm not well versed with specs for typing tools.

I tried to create a simple scenarios and compared `ty` with `mypy`, `pyright`, `pyrefly` and `cPython` runtime to do some _market survey_. Hope it's useful.

Summary:
1.  All except `ty` are honoring `PYTHONPATH`.
2. None are honoring `PYTHONSAFEPATH`.
3. `pyrefly` warns about `PYTHONPATH` environment variable, informing that other environments might not have it set.
4. `pyright` finds `a` from `./a.py` instead of `mods/a.py` unlike others; but does search in `mods` as demonstrated by finding `b`. Seems to be related to search path module order, but I haven't investigated further?
5. Note, cPython runtime ignores `pwd` because of safepath.
 
Arguably, very few people have to deal with weird search path manipulations, but having predictable, well documented search path resolution similar to runtime is better.

Side note: My personal foray into manipulating PYTHONPATH/PYTHONSAFEPATH is virtue of large projects with heremetic build systems including codegen. Manipulating search paths allows me to use tools (lsp, linter, typecheck, editor) not managed by build-system to work seamlessly and provide better user experience. I hope I'm not alone who makes such choices.

---
Environment:
```
❯ uv self version
uv 0.7.8 (Homebrew 2025-05-23)
❯ ty version
ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)
❯ uv run python --version
Python 3.13.2
❯ env | grep -i PYTHON
PYTHONSAFEPATH=1
PYTHONPATH=mods
PYTHONDONTWRITEBYTECODE=1
❯ uv run python -m site
sys.path = [
    '/Users/ocrolus/Projects/misc/tyimports/mods',
    '/Users/ocrolus/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/lib/python313.zip',
    '/Users/ocrolus/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/lib/python3.13',
    '/Users/ocrolus/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/lib/python3.13/lib-dynload',
    '/Users/ocrolus/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/lib/python3.13/site-packages',
]
USER_BASE: '/Users/ocrolus/.local' (exists)
USER_SITE: '/Users/ocrolus/.local/lib/python3.13/site-packages' (doesn't exist)
ENABLE_USER_SITE: True
```

Files:
```
❯ tree --noreport 
.
├── a.py
├── c.py
└── mods
    ├── __main__.py
    ├── a.py
    └── b.py
```

`a.py`
```python
alpha: str = "alpha"
alpha_str: str = "alpha str"
```
`c.py`
```python
gamma: int = 100
```
`mods/__main__.py`
```python
#!/usr/bin/env python

from typing import TYPE_CHECKING
from a import alpha, alpha_list
from b import beta

if TYPE_CHECKING:
    from a import alpha_str
    from c import gamma


def main() -> None:
    print(f"{alpha + beta = }")
    print(f"{alpha_list = }")


if __name__ == "__main__":
    main()
```
`mods/a.py`
```python
alpha: int = 1
alpha_list: list[int] = [1, 2, 3]
```
`mods/b.py`
```python
beta: int = 10
```
---

Observations:

1. Python runtime on running `uv run mods` produces. Since `./a.py` and `./c.py` are not available therefore both imports in `TYPE_CHECKING` would have produced error otherwise. 
```
❯ uv run mods
alpha + beta = 11
alpha_list = [1, 2, 3]
```
_IMHO this is exactly how type checkers should be able to find modules. In addition they should be able to look into type shedding according to specs._

2. Pyright
```
❯ pyright mods
/Users/ocrolus/Projects/misc/tyimports/mods/__main__.py
  /Users/ocrolus/Projects/misc/tyimports/mods/__main__.py:2:22 - error: "alpha_list" is unknown import symbol (reportAttributeAccessIssue)
  /Users/ocrolus/Projects/misc/tyimports/mods/__main__.py:11:14 - error: Operator "+" not supported for types "str" and "int" (reportOperatorIssue)
2 errors, 0 warnings, 0 informations 
```

3. Mypy
```
❯ mypy mods
mods/__main__.py:6: error: Module "a" has no attribute "alpha_str"; maybe "alpha_list"?  [attr-defined]
Found 1 error in 1 file (checked 3 source files)
```

4. pyrefly
```
❯ pyrefly check mods
 WARN PYTHONPATH environment variable is set to `mods`. Checks in other environments may not include these paths.
ERROR /Users/ocrolus/Projects/misc/tyimports/mods/__main__.py:6:19-28: Could not import `alpha_str` from `a` [missing-module-attribute]
 INFO 1 errors shown, 0 errors ignored, 3 modules, 64 transitive dependencies, 17,594 lines, took 0.09s, peak memory physical 78.3 MiB
```

5. `ty`
```
❯ ty check mods
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Module `a` has no member `alpha_list`
 --> mods/__main__.py:2:22
  |
1 | from typing import TYPE_CHECKING
2 | from a import alpha, alpha_list
  |                      ^^^^^^^^^^
3 | from b import beta
  |
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `b`
 --> mods/__main__.py:3:6
  |
1 | from typing import TYPE_CHECKING
2 | from a import alpha, alpha_list
3 | from b import beta
  |      ^
4 |
5 | if TYPE_CHECKING:
  |
info: make sure your Python environment is properly configured: https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment
info: rule `unresolved-import` is enabled by default

Found 2 diagnostics
```
5.a `ty` with extra search path
```
❯ ty check --extra-search-path=mods mods 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Module `a` has no member `alpha_str`
 --> mods/__main__.py:6:19
  |
5 | if TYPE_CHECKING:
6 |     from a import alpha_str
  |                   ^^^^^^^^^
7 |     from c import gamma
  |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

---

_Comment by @AlexWaygood on 2025-08-21 11:15_

@natelust has opened a PR implementing this feature in https://github.com/astral-sh/ruff/pull/20016. @natelust -- the survey of other type checkers above seems to indicate that they all respect `PYTHONPATH` by default, but your PR requires users to opt into the support using a CLI flag. If we add this feature, could you explain why you thought it should be behind a CLI flag -- is there a reason why it would unwelcome to some users? Why shouldn't it be on by default?

@carljm, I'm curious for your thoughts on whether it's worthwhile to implement this feature. It seems reasonable to me, but I'd love a second opinion!

---

_Comment by @MichaReiser on 2025-08-21 11:27_

If we want this to be opt in, a related question for me is what's the upside compared to e.g. a `TY_EXTRA_PATHS` environment variable (that adds additional paths)

---

_Comment by @carljm on 2025-08-21 14:06_

I don't have strong feelings here; it seems reasonable to me to respect PYTHONPATH. I think compatibility with other type checkers is quite valuable, in the absence of strong reasons to do something different. So I would lean towards making this opt-out.

Not sure we need a warning like pyrefly, but it is important that if we find a PYTHONPATH, we tell the user about it in whatever verbosity level we tell them what venv we are using. 

---

_Comment by @natelust on 2025-08-21 16:26_

@AlexWaygood thanks for your time looking at this. I have no strong reason for making this opt-in vs opt-out, except that my natural inclination when introducing new features are to keep the existing behavior people have developed work flows around unchanged, and put new additions behind feature flags, in some sense "reverse" deprecating. However I do see that this means that at some point either the default changes and you are in the same situation, or you get a pile of opt-ins that everyone must apply. I would be perfectly happy for searching PYTHONPATH to be the default.

---

_Closed by @MichaReiser on 2025-09-20 11:40_

---
