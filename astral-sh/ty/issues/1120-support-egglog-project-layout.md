```yaml
number: 1120
title: "Support \"egglog\" project layout"
type: issue
state: closed
author: sharkdp
labels:
  - configuration
  - imports
assignees: []
created_at: 2025-09-03T11:41:01Z
updated_at: 2025-09-05T11:53:50Z
url: https://github.com/astral-sh/ty/issues/1120
synced_at: 2026-01-12T15:54:24Z
```

# Support "egglog" project layout

---

_@sharkdp_

The [egglog-python](https://github.com/egraphs-good/egglog-python) project uses a layout where the Python source code is kept under `python/egglog` and tests are under `python/tests`. We can simulate this layout using something like

```
▶ tree           
.
└── python
    ├── my_package
    │   └── __init__.py
    └── tests
        └── test.py
```

with `python/my_package/__init__.py` containing
```py
class C: ...
```
and `python/tests/test.py` containing
```py
from my_package import C
```


Both `pyright` and `mypy` handle this layout just fine without any configuration (I verified that they actually check the source files):
```
▶ pyright                 
0 errors, 0 warnings, 0 informations

▶ mypy .                  
Success: no issues found in 2 source files
```

`pyrefly` doesn't work with that layout by default, but if you explicitly pass `python` as a path, it also works fine:
```py
▶ pyrefly check python/ 
 INFO 0 errors
```

`ty` does not support this layout, even if the path is passed explicitly:
```
▶ ty check --output-format=concise                                              
test.py:1:6: error[unresolved-import] Cannot resolve imported module `my_package`
Found 1 diagnostic

▶ ty check --output-format=concise python                                       
test.py:1:6: error[unresolved-import] Cannot resolve imported module `my_package`
Found 1 diagnostic
```

This can be fixed by passing `--extra-search-path python`, but I wonder if ty should support this out of the box somehow? At least when calling `ty check python`?



Related PR: https://github.com/astral-sh/ruff/pull/20078

---

_Label `configuration` added by @sharkdp on 2025-09-03 11:41_

---

_Label `imports` added by @sharkdp on 2025-09-03 11:41_

---

_Comment by @carljm on 2025-09-03 15:41_

I wonder what heuristics other type checkers are using to determine the import root ("project root") in this case. The answer we want is that the project root is `python/`. I think our current heuristic, if `--project` is not specified explicitly on the CLI, is (something like -- would need to double check the code) the project root is wherever we find a `pyproject.toml` or `ty.toml` (fallback to CWD if we don't find one?), and then we have a special case for the source code being kept inside a `src/` subdirectory. Effectively this layout is the same, except with `python/` instead of `src/`. We could easily just add `python/` subdir as an automatically-handled special case, like `src/`.

It's also possible to use heuristics involving presence or absence of an `__init__.py` in a directory (e.g. we decide that `python/` is a "container" directory, not a top-level package, because it has no `__init__.py`). But this is risky because lack of `__init__.py` doesn't mean a module isn't a Python package, it can be a namespace package (and sometimes that's done accidentally because it works fine at runtime, even if there's no intention for it to be a namespace package.)

It would also be possible to just always add the path given on the CLI as a search path, but this can also lead to confusing errors if a sub-package directory is passed on CLI, and there's a submodule in that directory with a name that clashes with a top-level module name.

BTW I think the "correct" way to handle this today in ty is not `--extra-search-paths` but `--project`, e.g. `ty check --project python/`. Or of course it can be configured in the `pyproject.toml` or `ty.toml`.

---

_Comment by @AlexWaygood on 2025-09-03 16:16_

> BTW I think the "correct" way to handle this today in ty is not `--extra-search-paths` but `--project`, e.g. `ty check --project python/`. Or of course it can be configured in the `pyproject.toml` or `ty.toml`.

That will affect pyproject.toml and ty.toml discovery as well as first-party search paths -- I agree `--extra-search-path` is not the answer here (those search paths take precedence over even the vendored typeshed stubs in module resolution!), but the best way to set this from the command line right now is probably `ty check --config "environment.root = ['./python']"`. See https://docs.astral.sh/ty/modules/#first-party-modules

---

_Comment by @AlexWaygood on 2025-09-03 16:22_

Of course, if the user has a virtual environment activated -- and if you're working on a Python project locally, you'd usually either have one explicitly activated in your shell or you'd use a project-management tool like uv/poetry/hatch/pdm/flit/etc. that always has it implicitly activated prior to executing any commands -- then the `./python` directory will be added as a search path anyway. It'll be editably installed in the `site-packages` directory, and so we'd pick up the editable installation naturally if the user invoked ty using `uv run ty` or `uv run --with=ty ty`. That's not a good solution for #20078 because a cold editable install of egglog takes forever (it's a maturin-based project, so a cold editable install involves compiling a bunch of Rust code), so that would significantly slow down the execution time of our mypy_primer CI job. Most of our users who are invoking ty on maturin-based projects will be doing so with a warm cache, however; `uv run ty check` shouldn't take _too_ long for them.

---

_Comment by @AlexWaygood on 2025-09-04 16:09_

> I wonder what heuristics other type checkers are using to determine the import root ("project root") in this case.

The logic mypy uses is documented here: https://mypy.readthedocs.io/en/stable/running_mypy.html#mapping-file-paths-to-modules.

It's obviously nice that mypy's set of heuristics means that it's able to correctly identify in this case that the `python/` directory should be added as a first-party _search path_ rather than constituting a first-party _package_. But what if that heuristic was wrong here? Maybe the user actually _did_ want the `python/` directory to be a first-party namespace package? If that was the case, then mypy's assumption here would have been incorrect, and the user would have had to wade through ^those (complicated!) mypy docs and figure out that they need to pass `--explicit-package-bases` to get mypy to do what they want.

I'm not saying we can't improve our behaviour here. It's obviously really nice UX that mypy and pyright can correctly infer the first-party search path here. But I also really hope that we can stick with something that's simpler and easier to debug/reason about than what mypy has.

---

_Comment by @carljm on 2025-09-04 19:02_

Tbh just special-casing `python/` along with `src/` doesn't seem terrible -- it seems unlikely that someone would want a top-level package named "python".

---

_Comment by @AlexWaygood on 2025-09-04 19:05_

Yeah, and maturin isn't an _un_popular build backend (all maturin-based projects will have a `python/` directory IIUC)

---

_Closed by @sharkdp on 2025-09-05 11:53_

---
