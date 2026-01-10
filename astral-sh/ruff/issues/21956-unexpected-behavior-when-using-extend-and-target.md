```yaml
number: 21956
title: Unexpected behavior when using extend and target-version
type: issue
state: open
author: jbarnoud
labels:
  - bug
  - configuration
assignees: []
created_at: 2025-12-12T20:02:35Z
updated_at: 2025-12-14T19:45:49Z
url: https://github.com/astral-sh/ruff/issues/21956
synced_at: 2026-01-10T11:10:00Z
```

# Unexpected behavior when using extend and target-version

---

_Issue opened by @jbarnoud on 2025-12-12 20:02_

### Summary

My configuration contains `target-version` in a file referred to behind an `extend`, but it is not respected.

My projects contains a `ruff.toml` provided by a template that contains the `target-version` among other things, and a `.ruff.toml` with the configuration specific to the project which extends the `ruff.toml`. The `target-version` in `ruff.toml` is not respected and ruff falls back to the version in the `pyproject.toml`.

A similar issue was described in #20090, however, this issue was closed in favor of #8795 that focusses `per-ignore-file`.

I can reproduce the problem with a project containing a `ruff.toml`, a `.ruff.toml`, a `main.py`, and a `pyproject.toml`:

```yaml
# ruff.toml
target-version = "py310"
```

```yaml
# .ruff.toml
extend = "ruff.toml"
```

```python
# main.py
from typing import TypeAlias

A: TypeAlias = str | int
```

```yaml
# pyproject.toml
[project]
name = "repro-ruff"
version = "0.1.0"
requires-python = ">=3.13"
```

When running `ruff check --select UP040 --verbose`, I get the following output:

```
[2025-12-12][08:50:05][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/jon/dev/ruff-repro/.ruff.toml
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.13
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] Derived `target-version` from `requires-python` in `pyproject.toml`: Py313
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.13
[2025-12-12][08:50:05][ruff_workspace::pyproject][DEBUG] Derived `target-version` from `requires-python` in `pyproject.toml`: Py313
[2025-12-12][08:50:05][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.venv/.gitignore
[2025-12-12][08:50:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/pyproject.toml"
[2025-12-12][08:50:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/main.py"
[2025-12-12][08:50:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.venv"
[2025-12-12][08:50:05][ruff::commands::check][DEBUG] Identified files to lint in: 4.278326ms
[2025-12-12][08:50:05][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/main.py
[2025-12-12][08:50:05][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/pyproject.toml
[2025-12-12][08:50:05][ruff::commands::check][DEBUG] Checked 2 files in: 958.72µs
UP040 Type alias `A` uses `TypeAlias` annotation instead of the `type` keyword
 --> main.py:3:1
  |
1 | from typing import TypeAlias
2 |
3 | A: TypeAlias = str | int
  | ^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Use the `type` keyword

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

This is not the expected behavior as this error should not appear with `target-version` set to `py310`. The logs indeed indicate that ruff did not read the target version from the ruff toml files, but instead derived it from the `requires-python` field of the `pyproject.toml`.

On the same test project, I get the expected result if I explicitly point to the config with `ruff check --select UP040 --verbose --config .ruff.toml`:

```
[2025-12-12][08:48:54][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-12-12][08:48:54][ruff::resolve][DEBUG] Using user-specified configuration file at: .ruff.toml
[2025-12-12][08:48:54][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.venv/.gitignore
[2025-12-12][08:48:54][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/pyproject.toml"
[2025-12-12][08:48:54][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/main.py"
[2025-12-12][08:48:54][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.venv"
[2025-12-12][08:48:54][ruff::commands::check][DEBUG] Identified files to lint in: 5.186424ms
[2025-12-12][08:48:54][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/pyproject.toml
[2025-12-12][08:48:54][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/main.py
[2025-12-12][08:48:54][ruff::commands::check][DEBUG] Checked 2 files in: 1.215123ms
```

I traced part of the execution and figured out that, in the failing case, [`ruff_workspace::resolver::resolve_configuration`](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/resolver.rs#L335) calls [`ruff_workspace::pyproject::load_options`](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/pyproject.rs#L138) for `.ruff.toml` with `version_strategy` set to `TargetVersionStrategy::RequiresPythonFallback`. Because the file does not define `target-version`, the function [emits the log](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/pyproject.rs#L161) that says "No `target-version` found in `ruff.toml`" (which is misleading as the function is reading `.ruff.toml` and not `ruff.toml`) and [sets the target version to the fallback](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/pyproject.rs#L176). `resolve_configuration` then calls `load_options` on `ruff.toml` and correctly reads the target version. Finally, `resolve_configuration` converts the `Options` instances it got from `load_options` into `Configuration` instances and [merges them](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/resolver.rs#L375) by calling [`Configuration::combine`](https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_workspace/src/configuration.rs#L588). The `target_version` field of `Configuration` is an `Option<str>` and `combine` calls `Option::or` on them. Because the `Configuration` read from `.ruff.toml` has the target version sets to the fallback version, we get `Some(fallback_version).or(Some(version_read_in_the_other_file))` and ruff keeps the fallback version.

When passing the `--config` argument, `.ruff.toml` is read with `version_strategy` set to `TargetVersionStrategy::UseDefault`. When `load_options` does not find a target version in the file, [it keeps the `targer_version` attribute set to `None`](https://github.com/astral-sh/ruff/blob/a722df6a7309c6de2eaac1ba55abe8e741ea7e17/crates/ruff_workspace/src/pyproject.rs#L163) so the `combine` later works as expected.


### Version

ruff 0.14.9 and 1b44d7e2a7eb80cf22dd2e2612c1c6fc8756369c

---

_Comment by @ntBre on 2025-12-12 20:38_

Thank you for the great writeup! 

This does sound like a bug to me, and I think your diagnosis of the cause is correct too.

cc @dylwil3, I _think_ this is related to the changes in https://github.com/astral-sh/ruff/pull/16319 because the example works as expected before Ruff 0.11:

```shell
❯ uvx ruff@0.10.0 check --no-cache --select UP040
All checks passed!

❯ uvx ruff@0.11.0 check --no-cache --select UP040 --output-format concise
main.py:4:1: UP040 Type alias `A` uses `TypeAlias` annotation instead of the `type` keyword
Found 1 error.
```

---

_Label `bug` added by @ntBre on 2025-12-12 20:38_

---

_Label `configuration` added by @ntBre on 2025-12-12 20:38_

---

_Comment by @denyszhak on 2025-12-14 19:45_

@ntBre I took a shot at fixing this by deferring the fallback logic, would appreciate a review

---
