```yaml
number: 21264
title: ruff check --fix ignores ruff.toml when --stdin-filename is present on the command line
type: issue
state: open
author: mgedmin
labels:
  - cli
assignees: []
created_at: 2025-11-03T21:05:20Z
updated_at: 2025-11-03T21:27:30Z
url: https://github.com/astral-sh/ruff/issues/21264
synced_at: 2026-01-12T15:54:57Z
```

# ruff check --fix ignores ruff.toml when --stdin-filename is present on the command line

---

_@mgedmin_

### Summary

I would like to use Vim to edit my code.  I use a Vim plugin called ALE to show me lint messages, and also to fix certain things.  ALE can use various tools, including Ruff.

I've noticed that when I use vim's `:ALEFix` command with ruff configured as the fixer, ruff will ignore custom configuration in a `ruff.toml`.  Specifically, I want ruff to sort my imports, so I created a `ruff.toml` in the parent directory with

```toml
[lint]
extend-select = ["I"]

[lint.isort]
lines-after-imports = 2
```

I can test this in shell and it works fine: `ruff check --fix - < myfile.py`.

I used vim's `:ALEDetails` to see what exactly ALE is executing, and saw that it was also passing `--stdin-filename myfile.py`.

I've tested in shell and `ruff check --stdin-filename myfile.py --fix - < myfile.py` ignores my ruff.toml and doesn't sort my imports.

I don't understand why the presence of `--stdin-filename` would affect the configuration file lookup logic, and I didn't see anything like this mentioned in the documentation next to `--stdin-filename` or in the section about configuration file lookup logic, so I assume it must be a bug.  Hence this report.

### Version

ruff 0.14.3

---

_Comment by @ntBre on 2025-11-03 21:23_

I can reproduce this. It looks like a relative `--stdin-filename`, which is used here:

https://github.com/astral-sh/ruff/blob/f64bbb45b9d7249138d65cc9b9ac6b87f74eef1b/crates/ruff/src/resolve.rs#L65

has no ancestors and so doesn't enter the code here:

https://github.com/astral-sh/ruff/blob/f64bbb45b9d7249138d65cc9b9ac6b87f74eef1b/crates/ruff_workspace/src/pyproject.rs#L92-L94

One workaround then is to use an absolute `--stdin-filename` argument:

```shell
$ ruff check --no-cache --stdin-filename $(realpath try.py) - <<EOF
type X[T] = list[T]
EOF
try.py:1:1: SyntaxError: Cannot use `type` alias statement on Python 3.10 (syntax was added in Python 3.12)
  |
1 | type X[T] = list[T]
  | ^^^^
  |

Found 1 error.
$ ruff check --no-cache --stdin-filename try.py - <<EOF
type X[T] = list[T]
EOF
All checks passed!
$ ruff check --no-cache - <<EOF
type X[T] = list[T]
EOF
-:1:1: SyntaxError: Cannot use `type` alias statement on Python 3.10 (syntax was added in Python 3.12)
  |
1 | type X[T] = list[T]
  | ^^^^
  |

Found 1 error.
```

with a `ruff.toml` file in a parent directory setting the Python version to 3.10, so the diagnostic is correct.

But hopefully we can fix something here too because this does seem surprising.

---

_Label `cli` added by @ntBre on 2025-11-03 21:23_

---
