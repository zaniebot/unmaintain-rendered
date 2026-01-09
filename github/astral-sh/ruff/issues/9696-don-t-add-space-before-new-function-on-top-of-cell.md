---
number: 9696
title: "Don't add space before new function on top of cell"
type: issue
state: closed
author: yasirroni
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-01-30T04:19:36Z
updated_at: 2024-07-12T08:45:47Z
url: https://github.com/astral-sh/ruff/issues/9696
synced_at: 2026-01-07T13:12:15-06:00
---

# Don't add space before new function on top of cell

---

_Issue opened by @yasirroni on 2024-01-30 04:19_

Consider `ipynb` file.

```py
# some code here, `.py` cell

# %% change cell

## Markdown cell

# %% change cell

# comment describing a function
def foo():
    pass
````

Ruff will add two new line between the comment and foo function.

___

Here is the `.ipynb` example, look at the new `\n` before `def foo3`,

before
```ipynb
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Hello"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo2():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# comment\n",
    "def foo3():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
```

after
```ipynb
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Hello"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo2():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# comment\n",
    "\n",
    "\n",
    "def foo3():\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
```

___
My setting:
`pre-commit run --all-files`

```
  # autofix using ruff
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.15
    hooks:
      # Run the linter.
      - id: ruff
      # Run the formatter.
      - id: ruff-format
```

```
[tool.ruff]
fix = true
line-length = 147
indent-width = 4
select = [
    "E",  # pycodestyle errors
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "C",  # flake8-comprehensions
    "B",  # flake8-bugbear
]
ignore = [
    "B90",  # support <3.10
    "E402",  # module level import not at top of file
    "E741",  # ambiguous variable name
    "F403",  # 'from module import *' used; unable to detect undefined names
    "C901",  # function is too complex (TEMPORARILY IGNORE)
    "B006",  # use mutable data structures for argument defaults (TEMPORARILY IGNORE)
]
extend-include = ["*.ipynb"]

[tool.ruff.format]
quote-style = "single"
indent-style = "space"
line-ending = "auto"
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `notebook` added by @MichaReiser on 2024-01-30 07:06_

---

_Comment by @MichaReiser on 2024-01-30 07:10_

The difference that I see comparing *before* and *after* is that there are two blank lines between the comment and `foo3` where there was no blank line before. 

This looks incorrect to me. Leading comments of a function should not be separated by blank lines. @dhruvmanila do you have an idea why this is happening? 

---

_Label `bug` added by @MichaReiser on 2024-01-30 07:10_

---

_Comment by @yasirroni on 2024-01-30 07:26_

Hmm, I don't know why, but this bug suddenly disappear. I can;t reproduce again (even though I was able to reproduce it multiple times before reporting the issue here).

Maybe should close it right now?

---

_Comment by @MichaReiser on 2024-01-30 07:36_

Oh no, I'm sure there's a bug somewhere... I'll close the issue for now but please comment here if you're able to reproduce or open a new issue if you face the problem again

---

_Closed by @MichaReiser on 2024-01-30 07:36_

---

_Comment by @yasirroni on 2024-01-30 07:38_

Okay, after further testing, it sometimes appears (yeah it back) and sometimes disappear. What happen exactly, I still don't know. I can manually fix the bug, and ruff did not cause the bug again. I'm truly confused here. I will give further report if I found it again.

I'm only using ruff and nbqa. I will remove nbqa and check from there.

---

_Reopened by @MichaReiser on 2024-01-30 07:41_

---

_Comment by @MichaReiser on 2024-01-30 07:41_

Okay. Let's keep this open. Maybe @dhruvmanila has an idea of what's happening. He's our notebook expert. 

---

_Comment by @yasirroni on 2024-01-30 07:43_

Added context.

Here is the full pre-commit

```
default_stages: [commit]

repos:
  # check yaml and end of file fixer
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: check-yaml
      - id: end-of-file-fixer
        exclude: LICENSE

  # nbqa autopep8 and ruff
  - repo: https://github.com/nbQA-dev/nbQA
    rev: 1.7.1
    hooks:
    - id: nbqa-ruff
      additional_dependencies: [ruff==0.1.15]

  # autofix using ruff
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.15
    hooks:
      # Run the linter.
      - id: ruff
      # Run the formatter.
      - id: ruff-format
```

Currently working notebook:
```
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Hello"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo(bar):\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def foo2(bar):\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "lines_to_next_cell": 2
   },
   "outputs": [],
   "source": [
    "# comment\n",
    "def foo3(bar):\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# comment\n",
    "def foo3(bar):\n",
    "    return bar"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "jupytext": {
   "formats": "notebooks//ipynb,scripts//py:percent",
   "main_language": "python"
  },
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
```

---

_Comment by @yasirroni on 2024-01-30 07:49_

`ruff format .`: no bug
`pre-commit run --all-files`: bug

---

_Comment by @yasirroni on 2024-01-30 07:53_

If I did not use nbqa on pre-commit:

`ruff format .`: fixing all
`pre-commit run --all-files`: ruff didn't check any notebooks (weird)

---

_Comment by @yasirroni on 2024-01-30 07:58_

Solution: Change pre-commit to also target notebook.

```
  # autofix using ruff
  - repo: https://github.com/astral-sh/ruff-pre-commit
    # Ruff version.
    rev: v0.1.15
    hooks:
      # Run the linter.
      - id: ruff
        types_or: [ python, pyi, jupyter ]
        args: [ --fix ]
      # Run the formatter.
      - id: ruff-format
        types_or: [ python, pyi, jupyter ]
```

It's weird that it requires `types_or: [ python, pyi, jupyter ]` when I already set it on toml `extend-include = ["*.ipynb"]`

I will close this issue. Maybe this is due to nbqa and ruff interaction.

---

_Closed by @yasirroni on 2024-01-30 07:58_

---

_Comment by @dhruvmanila on 2024-01-30 10:01_

Hey, thanks for providing a lot of context on the issue.

So, the way `pre-commit` works is by invoking the command on the file directly i.e., pre-commit is responsible for finding all the configured files (`types_or`) and pass it to Ruff. Now, if the Ruff command is invoked directly with the file path, it won't try to find some other files (`.ipynb` in your case) which is why you need to add the `jupyter` for pre-commit to pass the notebook files to Ruff.

For example, suppose you have `test.py`, `test.ipynb` in the current folder. Without the `jupyter` entry in pre-commit, it'll invoke Ruff only with `test.py`:

```console
$ ruff check test.py
```

So, you need to add the `jupyter` entry which will make pre-commit include `test.ipynb` as well:

```console
$ ruff check test.py test.ipynb
```

Now, I'm not exactly sure about the semantics of how pre-commit passes the file - does it pass them all at once or does it pass them one at a time? But, nevertheless you'll need to configure pre-commit for it to find notebook files.

That said, you don't necessarily need to update your `pyproject.toml` file if you're using Ruff only through pre-commit because it takes care of finding all the files. But, if you use Ruff from the CLI / editor with notebooks, then you need to update your `pyproject.toml` file as well.

---

_Referenced in [astral-sh/ruff-pre-commit#54](../../astral-sh/ruff-pre-commit/issues/54.md) on 2024-01-30 10:41_

---

_Comment by @NIvo172 on 2024-07-11 19:40_

I had the same problem and it was solved by your comment at [astral-sh/ruff#54 (comment)](https://github.com/astral-sh/ruff-pre-commit/issues/54#issuecomment-1916561394).
Maybe not the right place for my question, but if what you write here about pre-commit is true,
why does it work with nbQA, since I wouldn't specify the types_or parameter or the pass_filenames there either?

If you are right and pre-commit decides which files are called, noQA should not work without the parameter either.
And shouldn't `pre-commit run --all-files` just invoke all files and therefore Ruff should run on the Notebooks?

The answer to how pre-commit passes files is:
`pre-commit run` -> All **staged** files.
`pre-commit run --all-files` -> All **tracked** files.

I took a look into the pre-commit code and found:

``` python
def get_all_files() -> list[str]:
    return zsplit(cmd_output('git', 'ls-files', '-z')[1])
```
If I run `git ls-files` I can clearly see, that the Notebook is passed without to adjust any parameters in the pre-commit config.

This is a Bug in my opinion in the Ruff hook.

---
