```yaml
number: 15749
title: "`format.exclude` doesn't respect directories, while global `extend-exclude` does"
type: issue
state: closed
author: DavisVaughan
labels:
  - duplicate
assignees: []
created_at: 2025-01-27T01:12:34Z
updated_at: 2025-01-27T01:33:05Z
url: https://github.com/astral-sh/ruff/issues/15749
synced_at: 2026-01-12T15:54:54Z
```

# `format.exclude` doesn't respect directories, while global `extend-exclude` does

---

_@DavisVaughan_

### Description

I've attached a zip with the following structure

```
test/
  ruff.toml
  python/
    test.py
```

If you set

```toml
extend-exclude = ["python"]
```

then you do get `python/test.py` excluded

```
ruff format -v
[2025-01-26][19:57:27][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/davis/Desktop/test/ruff.toml
[2025-01-26][19:57:27][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
[2025-01-26][19:57:27][ignore::gitignore][DEBUG] opened gitignore file: /Users/davis/.gitignore_global
[2025-01-26][19:57:27][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
[2025-01-26][19:57:27][ignore::gitignore][DEBUG] opened gitignore file: /Users/davis/Desktop/test/.ruff_cache/.gitignore
[2025-01-26][19:57:27][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/davis/Desktop/test/.ruff_cache"
[2025-01-26][19:57:27][ruff_workspace::resolver][DEBUG] Ignored path via `extend-exclude`: "/Users/davis/Desktop/test/python"
warning: No Python files found under the given path(s)
```

but if you set

```toml
[format]
exclude = ["python"]
```

then `python/test.py` is formatted!

```
ruff format -v
[2025-01-26][19:58:36][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/davis/Desktop/test/ruff.toml
[2025-01-26][19:58:36][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
[2025-01-26][19:58:36][ignore::gitignore][DEBUG] opened gitignore file: /Users/davis/.gitignore_global
[2025-01-26][19:58:36][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
[2025-01-26][19:58:36][ignore::gitignore][DEBUG] opened gitignore file: /Users/davis/Desktop/test/.ruff_cache/.gitignore
[2025-01-26][19:58:36][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/davis/Desktop/test/.ruff_cache"
[2025-01-26][19:58:36][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/davis/Desktop/test/python/test.py"
[2025-01-26][19:58:36][ruff::commands::format][DEBUG] format_path; path=/Users/davis/Desktop/test/python/test.py
[2025-01-26][19:58:36][tracing::span][DEBUG] Printer::print;
[2025-01-26][19:58:36][ruff::commands::format][DEBUG] Formatted 1 files in 593.50µs
1 file reformatted
```

I believe this is because when you process the global `extend-exclude`, that happens when you are _discovering_ files, and you get a chance to short circuit the discovery and tell `ignore` to refuse to recurse into `python/`. That happens here with `WalkState::Skip`: https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff_workspace/src/resolver.rs#L525. Because of that, you never even look at `test.py`.

But in the `format.exclude` case, you've already discovered `python/test.py`, and now you're applying ad-hoc format specific exclusions to the files you've discovered. That happens here:
https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff/src/commands/format.rs#L127

But `match_exclusion()` doesn't walk up the directory tree, so when it gets `path/to/python/test.py` (basename `test.py`) and tries to apply the `python` glob matcher, it never matches.

Notably, you do have some tools that do walk up the directory tree, like `is_file_excluded()` (https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff_workspace/src/resolver.rs#L692) and `match_any_exclusion()` (https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff_workspace/src/resolver.rs#L779) used by the LSP when deciding whether the open document should be formatted or not, so maybe it's a matter of slotting in or reworking one of those. I'd be mildly worried about the cost of looking up the directory tree of every single file you're getting ready to format though, so another alternative might be moving the formatter specific exclusions down into the discovery phase as well.

[test.zip](https://github.com/user-attachments/files/18552374/test.zip)

---

_Comment by @charliermarsh on 2025-01-27 01:24_

Thanks for the clear write-up, though I believe this is a duplicate of https://github.com/astral-sh/ruff/issues/8267

---

_Label `duplicate` added by @charliermarsh on 2025-01-27 01:24_

---

_Closed by @charliermarsh on 2025-01-27 01:33_

---
