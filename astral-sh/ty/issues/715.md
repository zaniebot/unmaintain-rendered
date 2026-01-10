```yaml
number: 715
title: silence import resolution errors from an existing compiled extension module
type: issue
state: closed
author: Stamppot82
labels:
  - imports
assignees: []
created_at: 2025-06-27T07:29:38Z
updated_at: 2025-06-27T20:41:17Z
url: https://github.com/astral-sh/ty/issues/715
synced_at: 2026-01-10T02:07:36Z
```

# silence import resolution errors from an existing compiled extension module

---

_Issue opened by @Stamppot82 on 2025-06-27 07:29_

### Summary

**Environment:** ty v0.0.1a12, Python 3.12, macOS, uv project

**Problem:** ty check reports unresolved-import errors for Cython extensions compiled to .so files, despite the imports working correctly at runtime.

**Console Output:** 
```
error[unresolved-import]: Cannot resolve imported module `.math_utils`
 --> src/__init__.py:5:7
  |
5 | from .math_utils import add
  |       ^^^^^^^^^^
```

**Project Structure:**
```
src/
├── __init__.py
├── math_utils.pyx                    # Cython source
└── math_utils.cpython-312-darwin.so  # Compiled extension (exists)
```

**Issue:** ty only looks for .py files but not compiled .so extensions. The import from .math_utils import ... fails because ty cannot find a [math_utils.py]

**Expected:** ty should resolve imports from compiled Cython extensions when:

A .pyx source file exists
A corresponding .so file is present
The package is properly built/installed

**Impact:**
False positives make ty unusable for projects using Cython extensions, which are common in scientific Python, performance-critical libraries, and hybrid codebases.

**Workaround:** None effective (stub files don't work for dynamic imports from compiled extensions).


---

_Comment by @AlexWaygood on 2025-06-27 13:00_

We don't plan to add support for inferring types from Cython `.pyx` sources in the near future, I'm afraid. Just to be able to read that source code (let alone infer types), we'd either have to make major changes to our existing parser or write a new parser.

> **Workaround:** None effective (stub files don't work for dynamic imports from compiled extensions).

Unfortunately I believe stub files are the only solution to this problem for now, both for ty and any other Python type checker

---

_Comment by @carljm on 2025-06-27 14:40_

I think it's very unlikely that we ever attempt to parse `.pyx` files, unless it becomes a specified typing feature that all Python type checkers support (I think zero type checkers support it today).

> stub files don't work for dynamic imports from compiled extensions

Can you say more about this? What's an example of a "dynamic import" that would be supported by parsing `pyx` files but can't be supported in a stub file?



---

_Comment by @AlexWaygood on 2025-06-27 14:46_

see https://github.com/astral-sh/ruff/issues/10250 for the equivalent Ruff issue, which would be a pre-requisite for this even being a possibility

---

_Comment by @erictraut on 2025-06-27 17:12_

If I understand the bug report correctly, the OP isn't expecting ty to parse the extension module or infer precise types for symbols imported from it. Any such symbols can be treated as `Unknown` if a stub file isn't present.

The question is whether ty should report an import resolution error here. Other type checkers (including mypy, pyright, and pyrefly) do not report an import resolution error if the import statement targets an extension module.

---

_Renamed from "ty cannot resolve imports from Cython-compiled extensions (.so files)" to "silence import resolution errors from an existing compiled extension module" by @carljm on 2025-06-27 18:21_

---

_Comment by @carljm on 2025-06-27 18:22_

Yes, I think silencing import errors in that case makes sense. I think it should be based on the existence of a `.so` file in the import path; `.pyx` files shouldn't be considered.

---

_Label `imports` added by @carljm on 2025-06-27 18:23_

---

_Label `help wanted` added by @carljm on 2025-06-27 18:23_

---

_Added to milestone `Beta` by @carljm on 2025-06-27 18:23_

---

_Label `help wanted` removed by @AlexWaygood on 2025-06-27 18:30_

---

_Comment by @erictraut on 2025-06-27 18:31_

FWIW, pyright looks for the following file extensions to identify extension module binaries.

```
const supportedNativeLibExtensions = ['.pyd', '.so', '.dylib'];
```

One additional gotcha ... when trying to resolve module `foo`, it's not enough to simply look for `foo.so`, etc. because extension module file names can include compound file extensions that encode additional information like platform details. For example: `foo.cpython-32m.so`.

---

_Comment by @carljm on 2025-06-27 18:34_

I'm curious why you found it important to include `.pyd` files? AFAIK the runtime doesn't ever import from those. Are there cases where you are unable to find a `.so` or `.dylib` but you are able to find a `.pyd`?

---

_Comment by @JelleZijlstra on 2025-06-27 18:41_

At runtime Python accepts .so files with the suffixes in `importlib.machinery.EXTENSION_SUFFIXES`:

```
>>> import importlib.machinery
>>> print(importlib.machinery.EXTENSION_SUFFIXES)
['.cpython-312-darwin.so', '.abi3.so', '.so']
```

So to be strictly in line with the runtime, type checkers should only allow extension modules with these suffixes. The values are machine and version dependent; from looking at the code `.dll` is allowed under Cygwin, for example.

---

_Comment by @erictraut on 2025-06-27 18:50_

> I'm curious why you found it important to include `.pyd` files? AFAIK the runtime doesn't ever import from those.

My understanding is that the runtime does import ".pyd" files on Windows. I did a quick internet search and asked ChatGPT, and they seem to agree. I don't have a Windows machine handy at the moment though, so I can't confirm myself.

---

_Comment by @JelleZijlstra on 2025-06-27 18:52_

I looked at the CPython code and that seems right; grep for `PYD_TAGGED_SUFFIX`.

---

_Comment by @carljm on 2025-06-27 18:54_

I suspect we don't want to rely on querying the active interpreter for its `importlib.machinery.EXTENSION_SUFFIXES`, and would rather hardcode a list of supported suffixes. We could make that list somewhat configured-platform-dependent to try to match some of the runtime behavior, though the it seems low-consequence if we silence an import error from a present shared library for a different platform. I don't think we should try to model the platform tags in `.so` filenames at all (we should recognize a found `.so` regardless of its platform tags.)

---

_Comment by @AlexWaygood on 2025-06-27 20:39_

> The question is whether ty should report an import resolution error here. Other type checkers (including mypy, pyright, and pyrefly) do not report an import resolution error if the import statement targets an extension module.

I agree that we want to avoid an `unresolved-import` error (we already have it on our roadmap to emulate pyright's feature where it is able to resolve modules to C extensions), but I think we probably do want to issue a warning-level diagnostic that the module we've resolved the import to doesn't have any stubs installed, and all members of that module will therefore be inferred as `Unknown`.

In terms of the implementation, this will therefore either involve changing our `resolve_module` query so that it returns a `Result` rather than an `Option`, or changing our `Module` struct so that it distinguishes between modules from which we can infer types and opaque C extensions from which nothing cna be inferred.

---

_Comment by @Stamppot82 on 2025-06-27 20:40_

To clarify on my initial report; I was lacking a `pyi` (stub) file, because `cythonize` did not except the `generate_pyi` argument, but this is ofcourse off-topic... Manually adding the pyi file did resolve the unresolved-import error.

> Any such symbols can be treated as `Unknown` if a stub file isn't present.

This is indeed do-able!

---

_Comment by @AlexWaygood on 2025-06-27 20:41_

I'll close this as a duplicate of https://github.com/astral-sh/ty/issues/487, but link to the discussion here from that issue

---

_Closed by @AlexWaygood on 2025-06-27 20:41_

---
