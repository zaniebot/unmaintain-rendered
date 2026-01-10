```yaml
number: 19664
title: "`allowed-unused-imports` doesn't work for top-level modules"
type: issue
state: closed
author: zeevro
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-07-31T17:39:41Z
updated_at: 2025-08-28T13:02:52Z
url: https://github.com/astral-sh/ruff/issues/19664
synced_at: 2026-01-10T11:09:59Z
```

# `allowed-unused-imports` doesn't work for top-level modules

---

_Issue opened by @zeevro on 2025-07-31 17:39_

### Summary

```console
$ ruff version
ruff 0.12.7

$ ruff check --isolated --config 'lint.pyflakes.allowed-unused-imports = ["os"]' <(echo 'import os')
/dev/fd/63:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
  |
  = help: Remove unused import: `os`

Found 1 error.
[*] 1 fixable with the `--fix` option.

$ ruff check --isolated --config 'lint.pyflakes.allowed-unused-imports = ["os"]' <(echo 'import os.path')
/dev/fd/63:1:8: F401 [*] `os.path` imported but unused
  |
1 | import os.path
  |        ^^^^^^^ F401
  |
  = help: Remove unused import: `os.path`

Found 1 error.
[*] 1 fixable with the `--fix` option.

$ ruff check --isolated --config 'lint.pyflakes.allowed-unused-imports = ["os.path"]' <(echo 'import os.path')
All checks passed!

```

### Version

ruff 0.12.7

---

_Comment by @ntBre on 2025-07-31 19:12_

Wow, thanks for the report! I started trying to bisect this, but I couldn't find any version of Ruff where this worked, despite the examples in the [documentation](https://docs.astral.sh/ruff/settings/#lint_pyflakes_allowed-unused-imports):

> Modules in this list are expected to be fully-qualified names (e.g., hvplot.pandas). Any submodule of a given module will also be ignored (e.g., given hvplot, hvplot.pandas will also be ignored).

I only found one test for this setting too, and it only checks the submodule case, which kind of explains the oversight:

https://github.com/astral-sh/ruff/blob/2617d56014076ab91c6c090090b0851898c3c4be/crates/ruff_linter/src/rules/pyflakes/mod.rs#L363-L371

I think the issue is that this `QualifiedName` check doesn't account for the empty segment at the start of a non-dotted path:

https://github.com/astral-sh/ruff/blob/2617d56014076ab91c6c090090b0851898c3c4be/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L336-L339

https://github.com/astral-sh/ruff/blob/2617d56014076ab91c6c090090b0851898c3c4be/crates/ruff_python_ast/src/name.rs#L229-L236

Something like that, at least. I think I've run into that before.


---

_Label `bug` added by @ntBre on 2025-07-31 19:12_

---

_Label `help wanted` added by @ntBre on 2025-07-31 19:13_

---

_Comment by @danparizher on 2025-08-02 01:55_

Is the solution not this simple? I'm not sure if I'm missing something major, but based on some testing, this change appears to work for me:

```diff
- let allowed_unused_import = QualifiedName::from_dotted_name(allowed_unused_import);
+ let allowed_unused_import = QualifiedName::user_defined(allowed_unused_import);
```

*Found in: `ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L337`*

-----

When processing an allowed unused import like `"os"`, which lacks a dot, the method calls `Self::builtin("os")`. This generates a qualified name with the segments `["", "os"]`.

However, an actual import statement like `import os.path` has a qualified name of `["os", "path"]`.

The resulting comparison fails:

  * **Import's Qualified Name:** `["os", "path"]`
  * **Allowed Import's Qualified Name:** `["", "os"]`
  * **Result:** `["os", "path"].starts_with(["", "os"])` returns `false`.

By switching to `QualifiedName::user_defined("os")`, the allowed import's qualified name is correctly generated as `["os"]`.

This leads to the correct comparison:

  * **Import's Qualified Name:** `["os", "path"]`
  * **Allowed Import's Qualified Name:** `["os"]`
  * **Result:** `["os", "path"].starts_with(["os"])` now returns `true`.

---

_Comment by @MichaReiser on 2025-08-02 08:58_

Nice analysis. That's quite possible. 

---

_Closed by @ntBre on 2025-08-28 13:02_

---
