```yaml
number: 15705
title: "allowed-unused-imports doesn't work with relative imports"
type: issue
state: open
author: dhduvall
labels:
  - rule
  - configuration
assignees: []
created_at: 2025-01-24T00:57:42Z
updated_at: 2025-08-29T12:25:47Z
url: https://github.com/astral-sh/ruff/issues/15705
synced_at: 2026-01-10T11:09:57Z
```

# allowed-unused-imports doesn't work with relative imports

---

_Issue opened by @dhduvall on 2025-01-24 00:57_

### Description

If an "unused" import is done with a relative path, `allowed-unused-imports` can't be configured to to allow it:
```shell
$ uv init --lib --name mod
$ cat >> pyproject.toml <<EOF

[tool.ruff]
lint.pyflakes.allowed-unused-imports = [
  "mod.foo.bar",
]
EOF
$ echo "from .foo import bar" > src/mod/junk.py
$ echo "bar = None" > src/mod/foo.py
$ uvx ruff check src
src/mod/junk.py:1:18: F401 [*] `.foo.bar` imported but unused
  |
1 | from .foo import bar
  |                  ^^^ F401
  |
  = help: Remove unused import: `.foo.bar`

Found 1 error.
[*] 1 fixable with the `--fix` option.
$ uvx ruff version
ruff 0.9.3 (90589372d 2025-01-23)
```
I was unable to find a spelling of `mod.foo.bar` that would work—none of `.foo.bar`, `foo.bar`, `foo` work.
If `junk.py` contains `from mod.foo import bar` (i.e., absolute path for the import), `ruff` allows the import as expected.

---

_Label `rule` added by @MichaReiser on 2025-01-24 08:14_

---

_Label `configuration` added by @MichaReiser on 2025-01-24 08:14_

---

_Comment by @MichaReiser on 2025-01-24 08:15_

Thanks for opening this issue. 

I'm not very familiar with that part of Ruff but it does seem that we don't support resolving relative imports in bindings? There's a comment here that suggest we would

https://github.com/astral-sh/ruff/blob/555b3a6a2c8120c405719cabf7444f32136775e1/crates/ruff_linter/src/checkers/ast/mod.rs#L646-L658

but we never prepend `self.module`. So I don't think we actually do. @charliermarsh might know more about the why. 

(Note, you may have to add a `__init__.py` to `mod` or add it to `namespace-packages` even after ruff recognizes relative imports to make `mod` a proper package)

---

_Comment by @TaKO8Ki on 2025-08-29 09:46_

@ntBre I have confirmed that my PR #20115 does not solve this issue, so I will work on this.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-08-29 12:25_

---
