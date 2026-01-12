```yaml
number: 1597
title: "Add command to generate type stubs (equivalent to mypy's `stubgen`)"
type: issue
state: open
author: MamadouSDiallo
labels:
  - feature
  - wish
assignees: []
created_at: 2025-11-20T03:54:41Z
updated_at: 2025-11-20T04:53:49Z
url: https://github.com/astral-sh/ty/issues/1597
synced_at: 2026-01-12T15:54:25Z
```

# Add command to generate type stubs (equivalent to mypy's `stubgen`)

---

_@MamadouSDiallo_

### Is your feature request related to a problem? Please describe.
I am developing a Python library that includes compiled extensions (via Cython, PyO3, or C-extensions). To ensure users receive proper IDE support (autocompletion, signatures, and docstrings), I need to distribute `.pyi` stub files alongside my compiled binaries (`.so` / `.pyd`).

Currently, `ty` does not have a mechanism to generate these stubs. This forces me to maintain `mypy` in my build chain specifically to use `stubgen`, even if I intend to use `ty` for the actual type checking.

### Describe the solution you'd like
I would like a built-in command (e.g., `ty stubgen` or `ty gen-stubs`) that can automatically generate `.pyi` files for a given package.

Crucially, this feature needs to support **compiled extension introspection**. It should be able to:
1.  Recurse through a target package.
2.  Detect compiled modules (binary extensions).
3.  Introspect them (similar to how `stubgen` imports modules at runtime) to extract function signatures and docstrings.
4.  Output standard, compliant `.pyi` files to a specified directory.

### Describe alternatives you've considered
* **Using `mypy`:** This is the current workaround. I install `mypy` solely to run `stubgen -p my_package`. This introduces a heavy dependency into the build environment that I am otherwise trying to replace.
* **Hand-writing stubs:** This is error-prone and not feasible for large or rapidly evolving codebases.

### Additional context
This feature is critical for library authors who distribute binary wheels. It aligns with the philosophy of a unified, high-performance toolchain. If `ty` is handling type checking, it would be highly beneficial if it also handled the generation of the artifacts (stubs) required for that checking to work for end-users of compiled libraries.

---

_Label `feature` added by @carljm on 2025-11-20 04:51_

---

_Label `wish` added by @carljm on 2025-11-20 04:52_

---

_Comment by @carljm on 2025-11-20 04:53_

Thanks for the suggestion! I'm not necessarily opposed to doing this, but I think we have a lot of things that will come before it on the roadmap, especially considering that "use mypy stubgen" is a pretty workable option, and it's not clear that we can significantly improve on mypy's stubgen.

---
