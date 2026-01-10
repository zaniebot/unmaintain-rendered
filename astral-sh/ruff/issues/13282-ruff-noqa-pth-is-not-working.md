```yaml
number: 13282
title: "# ruff: noqa: PTH is not working"
type: issue
state: open
author: jolaf
labels:
  - suppression
  - wish
assignees: []
created_at: 2024-09-08T11:20:07Z
updated_at: 2024-09-12T00:21:44Z
url: https://github.com/astral-sh/ruff/issues/13282
synced_at: 2026-01-10T11:09:55Z
```

# # ruff: noqa: PTH is not working

---

_Issue opened by @jolaf on 2024-09-08 11:20_

I have a large old module using `os.path` API.
I'm not ready to convert to `pathlib` yet.
However, I'd like to use `ruff` to check the module for other issues and avoid getting tons of messages about using old API.

For now, the best solution I have is to write `# ruff: noqa: PTH100, PTH101, PTH102, PTH103, ...` comment in the beginning of the file. That's working, but looks cumbersome.

It would be nice if `# ruff: noqa: PTH` would work the same way as `ignore = [ "PTH", ]` works in the config file.

The same thing would be nice for `--select` and `--ignore` options in the command line.

Here's the simple test:
```python3
# ruff: noqa: PTH

from os.path import exists, getsize, isfile

if exists(__file__) and isfile(__file__):
    print(getsize(__file__))
```
```shell
$ python test.py
141

$ ruff check test.py 
test.py:5:4: PTH110 `os.path.exists()` should be replaced by `Path.exists()`
  |
3 | from os.path import exists, getsize, isfile
4 | 
5 | if exists(__file__) and isfile(__file__):
  |    ^^^^^^ PTH110
6 |     print(getsize(__file__))
  |

test.py:5:25: PTH113 `os.path.isfile()` should be replaced by `Path.is_file()`
  |
3 | from os.path import exists, getsize, isfile
4 | 
5 | if exists(__file__) and isfile(__file__):
  |                         ^^^^^^ PTH113
6 |     print(getsize(__file__))
  |

test.py:6:11: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
  |
5 | if exists(__file__) and isfile(__file__):
6 |     print(getsize(__file__))
  |           ^^^^^^^ PTH202
  |

Found 3 errors.

$ ruff --version
ruff 0.6.4
```


---

_Label `suppression` added by @MichaReiser on 2024-09-08 18:16_

---

_Label `wish` added by @MichaReiser on 2024-09-08 18:17_

---

_Comment by @MichaReiser on 2024-09-08 18:19_

Hi. Overall, this makes sense to me. The question IMO is whether we want to encourage disabling entire sets of rules using noqa comments.

An alternative that is supported today is [`per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores). The downside is that it requires changing the configuration.

---

_Comment by @jolaf on 2024-09-09 00:20_

Maybe, `--select` and `--ignore` command line options are even more important in this matter.

Because they can be helpful for situations like "can I please see all issues in my code EXCEPT those messy `os.path` warnings`.

---

_Comment by @MichaReiser on 2024-09-09 10:46_

`ruff check` supports `--select`, and `--ignore`:

```
Rule selection:
      --select <RULE_CODE>
          Comma-separated list of rule codes to enable (or ALL, to enable all
          rules)
      --ignore <RULE_CODE>
          Comma-separated list of rule codes to disable
      --extend-select <RULE_CODE>
          Like --select, but adds additional rule codes on top of those already
          specified
```

---

_Comment by @jolaf on 2024-09-09 10:58_

> `ruff check` supports `--select`, and `--ignore`

Yes, but they don't support prefixes, only full codes.

So to disable, for example, all `PTH* ` warnings, you'd have to specify each of them manually, which is very inconvenient.

---

_Comment by @MichaReiser on 2024-09-09 11:07_

Do you have an example where passing a selector to `--select `and `--ignore` doesn't work for you because that is supposed to work

```shell
➜ uvx ruff check .
import.py:1:8: F401 [*] `typing` imported but unused
  |
1 | import typing
  |        ^^^^^^ F401
2 | 
3 | print(lemurs)
  |
  = help: Remove unused import: `typing`

import.py:3:7: F821 Undefined name `lemurs`
  |
1 | import typing
2 | 
3 | print(lemurs)
  |       ^^^^^^ F821
  |

main.py:1:18: F401 [*] `test.a` imported but unused
  |
1 | from test import a
  |                  ^ F401
  |
  = help: Remove unused import: `test.a`

main.py:13:5: F841 Local variable `x` is assigned to but never used
   |
12 | def f():
13 |     x = Test()
   |     ^ F841
   |
   = help: Remove assignment to unused variable `x`

main.py:16:5: F811 Redefinition of unused `f` from line 12
   |
16 | def f(): ...
   |     ^ F811
   |
   = help: Remove definition: `f`

main.py:19:5: F811 Redefinition of unused `f` from line 16
   |
19 | def f():
   |     ^ F811
20 |     """Docstring"""
21 |     ...
   |
   = help: Remove definition: `f`

symlink/foo.py:1:21: F401 [*] `bar.baz.Baz` imported but unused
  |
1 | from bar.baz import Baz
  |                     ^^^ F401
2 | 
3 | class Test:
  |
  = help: Remove unused import: `bar.baz.Baz`

test.ipynb:cell 4:1:1: F821 Undefined name `yyyyy`
  |
1 | yyyyy
  | ^^^^^ F821
  |

test.py:2:21: F401 [*] `bar.baz.Baz` imported but unused
  |
1 | import main
2 | from bar.baz import Baz
  |                     ^^^ F401
3 | 
4 | x = main.Test()
  |
  = help: Remove unused import: `bar.baz.Baz`

test.pyi:2:5: F811 Redefinition of unused `f` from line 1
  |
1 | def f(): ...
2 | def f(): ...
  |     ^ F811
3 | def f():
4 |   """Docstring"""
  |
  = help: Remove definition: `f`

test.pyi:3:5: F811 Redefinition of unused `f` from line 2
  |
1 | def f(): ...
2 | def f(): ...
3 | def f():
  |     ^ F811
4 |   """Docstring"""
5 |   ...
  |
  = help: Remove definition: `f`

Found 11 errors.
[*] 4 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).

~/astral/test via 🐍 3.12.1 
➜ uvx ruff check . --ignore F
All checks passed!
```

---

_Comment by @jolaf on 2024-09-09 11:12_

Woah, that's amazing!!

Well, then the documentation should be updated for this, including `--help`.
For now, it shows no idea that prefixes can be used.

In fact, it would be perfect if prefixes would work __anywhere__ a single code could be used, including single-line `# noqa:` comments.

---

_Comment by @MichaReiser on 2024-09-09 11:32_

> In fact, it would be perfect if prefixes would work anywhere a single code could be used, including single-line # noqa: comments.

Rule selectors should generally work everywhere, except in noqa comments. Whether we want to support selectors in noqa comments requires some thought. I can see downsides of allowing a rule selector. For example, it is easy to suppress more errors than you wanted by accidentally typing `F82` instead of `F821`. It's also unclear if `unused-noqa` should flag it or not

---

_Comment by @jolaf on 2024-09-09 11:55_

Yep, I see.

It seems to me that `unused-noqa` should only flag lines where there is no issues mentioned in a `# noqa:` comment. So if the line comment is `# noqa: PTH`, and there's at least one PTH* issue in that line, then `unsued-noqa` should not flag that line.

By the way, asterisk looks like a good idea. I mean, using comments like `# noqa: F82*`, not just `# noqa: F82`, that reduces the risk of accidentally suppressing more than you wanted.

---

_Comment by @msoloff on 2024-09-12 00:20_

I had a similar issue in that I wanted to start using Ruff on an existing codebase and didn't want to fix all the existing issues up front or litter my code with noqa directives. I initially planned to do the latter using `ruff check --noqa` but decided it seemed messy.

So instead I wrote a small Python script to convert the output of `ruff check` to the format of the per-file-ignores and just pasted that to the bottom of my ruff.toml. It's nice as a record of lints that I've suppressed and easy to remove everything for a file if I decide to bring it up to date.

I also use `select = ["ALL"]` with specific global ignores in the config and when a new version of Ruff comes out I can pipe them to the per-file-ignores if I want. 

This was useful  when a recent release added the ability to lint notebooks. I didn't want to fix the existing ones but new files will still be checked.

---
