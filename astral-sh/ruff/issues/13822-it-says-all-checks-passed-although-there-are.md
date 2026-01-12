```yaml
number: 13822
title: "It says \"All checks passed!\" although there are errors"
type: issue
state: closed
author: limafresh
labels:
  - question
assignees: []
created_at: 2024-10-19T14:45:50Z
updated_at: 2025-01-08T15:33:26Z
url: https://github.com/astral-sh/ruff/issues/13822
synced_at: 2026-01-12T15:54:53Z
```

# It says "All checks passed!" although there are errors

---

_@limafresh_

```
$ tree
.
├── LICENSE
├── MANIFEST.in
├── pyproject.toml
├── README.md
├── screenshots
│   ├── screenshot1.png
│   ├── screenshot2.png
│   ├── screenshot3.png
│   └── screenshot4.png
└── src
    └── pyqulator
        ├── __init__.py
        ├── locales
        │   ├── ui_ru.qm
        │   ├── ui_ru.ts
        │   ├── ui_uk.qm
        │   └── ui_uk.ts
        ├── main.py
        ├── ui.py
        └── ui.ui

5 directories, 16 files
host@antix1:~/GitHub/pyqulator
$ ruff check
All checks passed!
host@antix1:~/GitHub/pyqulator
```
It says "All checks passed!", although I deliberately made a lot of errors in the `main.py` file. Configuration in pyproject.toml, but even if you remove it, it's the same.

---

_Comment by @zanieb on 2024-10-19 18:26_

What kind of errors? This is pretty basic functionality that works for other users so there has to be something specific going on in your setup and we'll need more details to help.

---

_Comment by @zanieb on 2024-10-19 18:27_

Can you reproduce your `main.py` errors in https://play.ruff.rs ?

---

_Label `needs-info` added by @MichaReiser on 2024-10-19 18:42_

---

_Comment by @limafresh on 2024-10-22 07:32_

Doesn't react to string lengths and different types of quotes, although I specified this in pyproject.toml

---

_Comment by @dhruvmanila on 2024-10-22 07:46_

@limafresh Can you provide the contents of the `pyproject.toml` config and that of the Python file in which you expect the violations to show up but isn't?

---

_Comment by @limafresh on 2024-10-22 09:57_

**pyproject.toml**
```
line-length = 100
[tool.ruff.format]
quote-style = "double"
```

**hello.py**
```
hello = 'hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello, hello'
print(hello)
```

```
$ ruff check
All checks passed!
```

---

_Comment by @dhruvmanila on 2024-10-22 10:11_

Thanks for providing the file contents.

First, the [`line-length`](https://docs.astral.sh/ruff/settings/#line-length) config should go under the `[tool.ruff]` heading:
```toml
[tool.ruff]
line-length = 100

[tool.ruff.format]
quote-style = "double"
```

Now, I'm guessing that you must be looking for a [`line-too-long`](https://docs.astral.sh/ruff/rules/line-too-long/) violation to be highlighted, is that correct? If so, then you'll need to select the rule because it isn't included in the default rule set. The rules that are enabled by default are "E4", "E7", "E9", "F" (https://github.com/astral-sh/ruff/#configuration). So, let's do that:

```toml
[tool.ruff]
line-length = 100

[tool.ruff.lint]
extend-select = ["E501"]

[tool.ruff.format]
quote-style = "double"
```

And, regarding the quotes, that would be handled by the formatter which is a separate subcommand. So, running `ruff format` should update the quote style to use double quotes.

---

_Label `needs-info` removed by @dhruvmanila on 2024-10-22 10:11_

---

_Label `question` added by @dhruvmanila on 2024-10-22 10:11_

---

_Comment by @limafresh on 2024-10-22 11:23_

Why can't you make everything in check? Check says everything is fine, but format still fixes errors. It's wildly inconvenient. I thought it was a good replacement for flake8, but I was disappointed.

---

_Comment by @zanieb on 2024-10-22 11:30_

See #8232 

Please be kind to project maintainers.

---

_Comment by @limafresh on 2024-10-22 11:49_

How can I see what exactly will be formatted before formatting?

---

_Comment by @MichaReiser on 2024-10-22 11:55_

You can run `ruff format --diff` to see the formatting changes.

I take from your response that the original issue has been resolved. If not, feel free to comment here and I can reopen.

---

_Closed by @MichaReiser on 2024-10-22 11:55_

---

_Comment by @limafresh on 2024-10-22 12:04_

Thank you very much! I just learned for the first time what a formatter is and what they are for, and I thought it was an unfinished linter! Thank you very much, I'm switching to Ruff😊

---

_Comment by @szilvesztercsab on 2025-01-08 15:22_

For some reason, i'm hitting the same kind of issue and can't figure out why:

```console
❯ eza -aTI '.git|.venv'
.
├── .editorconfig
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── .ruff_cache
│   ├── .gitignore
│   ├── 0.8.6
│   │   ├── 5085134478066165372
│   │   ├── 5361439220955715206
│   │   ├── 6089338595112545736
│   │   └── 15102853139710753150
│   └── CACHEDIR.TAG
├── hello.py
├── pyproject.toml
├── README.md
└── uv.lock

❯ uv run ruff check -v
[2025-01-08][17:15:05][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/csaba/Code/py-experiments/pyproject.toml
[2025-01-08][17:15:05][ruff_linter::settings::types][DEBUG] Detected minimum supported `requires-python` version: 3.13
[2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.gitignore
[2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.git/info/exclude
[2025-01-08][17:15:05][ruff_linter::settings::types][DEBUG] Detected minimum supported `requires-python` version: 3.13
[2025-01-08][17:15:05][ignore::walk][DEBUG] ignoring /Users/csaba/Code/py-experiments/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/csaba/Code/py-experiments/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
[2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/csaba/Code/py-experiments/.git"
[2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/csaba/Code/py-experiments/pyproject.toml"
[2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.ruff_cache/.gitignore
[2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/csaba/Code/py-experiments/.ruff_cache"
[2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/csaba/Code/py-experiments/hello.py"
[2025-01-08][17:15:05][ruff::commands::check][DEBUG] Identified files to lint in: 3.080125ms
[2025-01-08][17:15:05][ruff::diagnostics][DEBUG] Checking: /Users/csaba/Code/py-experiments/pyproject.toml
[2025-01-08][17:15:05][ruff::commands::check][DEBUG] Checked 2 files in: 122.625µs
All checks passed!
```

`pyproject.toml`
```toml
[project]
dependencies = []
description = "Add your description here"
name = "py-experiments"
readme = "README.md"
requires-python = ">=3.13"
version = "0.1.0"

[dependency-groups]
dev = [
  "pre-commit>=4.0.1",
  "pytest>=8.3.4",
  "ruff>=0.8.6",
]

[tool.ruff]
fix = true
preview = true
required-version = ">=0.8.6"
show-fixes = true

[tools.ruff.lint]
fixable = ["ALL"]
select = ["ALL"]

[tool.ruff.format]
docstring-code-format = true

```

`hello.py`
```py
def foo(a, b, c):
    if a:
        if b:
            if c:
                return 1
            else:
                return 2
        else:
            return 3
    else:
        return 4

```

The above raises hell [on the playground](https://play.ruff.rs/3ff21bfd-1829-4552-84d6-aabd144f2608), but is a-okay on my machine.

What gives?

Also, should this be a seperate issue?

---

_Comment by @szilvesztercsab on 2025-01-08 15:33_

> For some reason, i'm hitting the same kind of issue and can't figure out why:
> 
> ❯ eza -aTI '.git|.venv'
> .
> ├── .editorconfig
> ├── .gitignore
> ├── .pre-commit-config.yaml
> ├── .python-version
> ├── .ruff_cache
> │   ├── .gitignore
> │   ├── 0.8.6
> │   │   ├── 5085134478066165372
> │   │   ├── 5361439220955715206
> │   │   ├── 6089338595112545736
> │   │   └── 15102853139710753150
> │   └── CACHEDIR.TAG
> ├── hello.py
> ├── pyproject.toml
> ├── README.md
> └── uv.lock
> 
> ❯ uv run ruff check -v
> [2025-01-08][17:15:05][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/csaba/Code/py-experiments/pyproject.toml
> [2025-01-08][17:15:05][ruff_linter::settings::types][DEBUG] Detected minimum supported `requires-python` version: 3.13
> [2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.gitignore
> [2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.git/info/exclude
> [2025-01-08][17:15:05][ruff_linter::settings::types][DEBUG] Detected minimum supported `requires-python` version: 3.13
> [2025-01-08][17:15:05][ignore::walk][DEBUG] ignoring /Users/csaba/Code/py-experiments/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/csaba/Code/py-experiments/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
> [2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/csaba/Code/py-experiments/.git"
> [2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/csaba/Code/py-experiments/pyproject.toml"
> [2025-01-08][17:15:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/csaba/Code/py-experiments/.ruff_cache/.gitignore
> [2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/csaba/Code/py-experiments/.ruff_cache"
> [2025-01-08][17:15:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/csaba/Code/py-experiments/hello.py"
> [2025-01-08][17:15:05][ruff::commands::check][DEBUG] Identified files to lint in: 3.080125ms
> [2025-01-08][17:15:05][ruff::diagnostics][DEBUG] Checking: /Users/csaba/Code/py-experiments/pyproject.toml
> [2025-01-08][17:15:05][ruff::commands::check][DEBUG] Checked 2 files in: 122.625µs
> All checks passed!
> `pyproject.toml`
> 
> [project]
> dependencies = []
> description = "Add your description here"
> name = "py-experiments"
> readme = "README.md"
> requires-python = ">=3.13"
> version = "0.1.0"
> 
> [dependency-groups]
> dev = [
>   "pre-commit>=4.0.1",
>   "pytest>=8.3.4",
>   "ruff>=0.8.6",
> ]
> 
> [tool.ruff]
> fix = true
> preview = true
> required-version = ">=0.8.6"
> show-fixes = true
> 
> [tools.ruff.lint]
> fixable = ["ALL"]
> select = ["ALL"]
> 
> [tool.ruff.format]
> docstring-code-format = true
> `hello.py`
> 
> def foo(a, b, c):
>     if a:
>         if b:
>             if c:
>                 return 1
>             else:
>                 return 2
>         else:
>             return 3
>     else:
>         return 4
> The above raises hell [on the playground](https://play.ruff.rs/3ff21bfd-1829-4552-84d6-aabd144f2608), but is a-okay on my machine.
> 
> What gives?
> 
> Also, should this be a seperate issue?

I have a nasty typo in `pyproject.toml#L22` which just cost me at least an hour of head scratching. God-damn.

It should say `[tool.ruff.lint]` but instead it sais `[tools.ruff.lint]`.

Sorry for bothering you all.

Kudos for the super awesome tool. Love it. ❤ 

---
