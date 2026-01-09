---
number: 16506
title: "`source-include` is completely ignored when `source-exclude` contains overlapping patterns"
type: issue
state: closed
author: byeblack
labels:
  - question
  - build-backend
assignees: []
created_at: 2025-10-30T01:40:27Z
updated_at: 2025-10-30T14:32:02Z
url: https://github.com/astral-sh/uv/issues/16506
synced_at: 2026-01-07T13:12:19-06:00
---

# `source-include` is completely ignored when `source-exclude` contains overlapping patterns

---

_Issue opened by @byeblack on 2025-10-30 01:40_

### Summary

When using `tool.uv.build-backend.source-exclude` with a broad pattern like `**/*.py`, the `source-include` setting is completely ignored, even when it specifies explicit files that should be included. This makes it impossible to exclude most files while keeping specific ones.

### Platform

Windows 11 x86_64

### Version

0.9.5 (d5f39331a 2025-10-21)

### Python version

3.12.12


### Configuration

```toml
[tool.uv.build-backend]
source-exclude = ["**/*.py"]
source-include = ["*.pyd", "__init__.py", "__main__.py"]
```

### Expected Behavior

The `source-include` patterns should take precedence over or create exceptions to `source-exclude` patterns. In this case, I expect:
- All `.py` files to be excluded (as per `source-exclude`)
- **Except** `__init__.py` and `__main__.py` files, which should be included (as per `source-include`)
- Plus any `.pyd` files should be included

This is a common use case where you want to:
1. Compile most Python files to `.pyd` (or exclude them for other reasons)
2. Keep only essential files like `__init__.py` and `__main__.py` as source

## Actual Behavior

The `source-include` setting is completely ignored. **No** `.py` files are included in the wheel, not even `__init__.py` or `__main__.py`.

From the debug output:
```
DEBUG Wheel excludes: ["__pycache__", "*.pyc", "*.pyo", "**/*.py"]
DEBUG Adding content files to wheel
DEBUG Source root: src
DEBUG Module path: zzz
DEBUG Adding to wheel: zzz
DEBUG Adding to wheel: zzz/config
DEBUG Adding to wheel: zzz/config/1.json
DEBUG Adding to wheel: zzz/gui
DEBUG Adding to wheel: zzz/gui/components
DEBUG Adding to wheel: zzz/gui/page
DEBUG Adding to wheel: zzz/gui/vcruntime140.dll
DEBUG Adding to wheel: zzz/www.txt
DEBUG Visited 8 files for wheel build
```

Notice that no `.py` files are added at all, even though `__init__.py` and `__main__.py` exist in multiple directories.

### Project Structure

```
src/
  zzz/
    __init__.py          # Should be included but is excluded
    __main__.py          # Should be included but is excluded
    app.py               # Should be excluded ✓
    config/
      __init__.py        # Should be included but is excluded
      app.py             # Should be excluded ✓
      ...
    gui/
      app.py             # Should be excluded ✓
      ...
```

### Workarounds Attempted

1. ❌ Using negation in exclude: `source-exclude = ["**/*.py", "!**/__init__.py"]`
   - Result: Error `Invalid character '!' at position 3 in glob`
   
2. ❌ Using character class negation: `source-exclude = ["[!__]*.py"]`
   - Result: Error `Invalid character '!' in range at position 4 in glob`

3. ❌ Only using `source-include` without `source-exclude`
   - Result: Includes ALL Python files, not just the specified ones

### Proposed Solution

Either:
1. Make `source-include` take precedence over `source-exclude` (evaluated after exclude, to re-include specific files)
2. Support glob negation patterns like `!**/__init__.py` in `source-exclude`
3. Document the current behavior clearly and provide guidance for this use case


---

_Label `bug` added by @byeblack on 2025-10-30 01:40_

---

_Label `build-backend` added by @konstin on 2025-10-30 13:38_

---

_Comment by @konstin on 2025-10-30 13:52_

For builds involving a compilation step, the uv build backend is currently not suited, as it doesn't (yet) have build script support. One of the core assumptions of the build backend is that we're adding a directory of Python files, so the includes and excludes aren't build for supporting excluding most Python files.

---

_Label `bug` removed by @konstin on 2025-10-30 13:52_

---

_Label `question` added by @konstin on 2025-10-30 13:52_

---

_Comment by @byeblack on 2025-10-30 14:06_

Thank you for the response!

However, I think this is actually a bug in the glob implementation itself, unrelated to my specific use case.

**The issue**: When I use `[!w]*.txt` or `[!__]*.py`, uv throws an error saying `Invalid character '!' in range`. But the error message itself documents that `[!...]` syntax should be supported:

```
[!...] is the negation of [...], i.e. it matches any characters not in the brackets.
```

So uv's own documentation says `[!...]` should work, but it doesn't. This seems like a bug where the documented glob syntax isn't actually implemented.

Additionally, `source-include` patterns are completely ignored when they overlap with `source-exclude` patterns, which makes it impossible to create exceptions to exclusion rules.

Should these be fixed, or is the error message documentation incorrect?

https://docs.rs/glob/latest/glob/struct.Pattern.html


---

_Comment by @konstin on 2025-10-30 14:14_

> **The issue**: When I use `[!w]*.txt` or `[!__]*.py`, uv throws an error saying `Invalid character '!' in range`. But the error message itself documents that `[!...]` syntax should be supported:
> 
> ```
> [!...] is the negation of [...], i.e. it matches any characters not in the brackets.
> ```
> 
> So uv's own documentation says `[!...]` should work, but it doesn't. This seems like a bug where the documented glob syntax isn't actually implemented.

That's not used by the uv build backend, which uses https://packaging.python.org/en/latest/specifications/glob-patterns/ (https://docs.astral.sh/uv/concepts/build-backend/#include-and-exclude-syntax). Did you get the cited message from the build backend error?

---

_Comment by @byeblack on 2025-10-30 14:20_

Thank you for the clarification and the link to the specification!

I understand now that the `[!...]` syntax is not part of the [PyPA glob spec](https://packaging.python.org/en/latest/specifications/glob-patterns/), so uv is correctly implementing the standard. The error message was just showing general glob concepts, not uv's actual supported syntax.

I appreciate you taking the time to explain this. I'll work with the current limitations.

Thanks again!


---

_Closed by @byeblack on 2025-10-30 14:32_

---
