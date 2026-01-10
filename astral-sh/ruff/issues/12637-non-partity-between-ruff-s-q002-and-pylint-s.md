```yaml
number: 12637
title: "Non-partity between ruff's Q002 and pylint's C0198 (bad-docstring-quotes)"
type: issue
state: closed
author: nwiltsie
labels:
  - question
  - docstring
assignees: []
created_at: 2024-08-02T17:28:40Z
updated_at: 2024-08-05T16:24:38Z
url: https://github.com/astral-sh/ruff/issues/12637
synced_at: 2026-01-10T11:09:54Z
```

# Non-partity between ruff's Q002 and pylint's C0198 (bad-docstring-quotes)

---

_Issue opened by @nwiltsie on 2024-08-02 17:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

#970 reports that pylint's [C0198](https://pylint.readthedocs.io/en/stable/user_guide/messages/convention/bad-docstring-quotes.html) / `bad-docstring-quotes` rule has been implemented as ruff's [Q002](https://docs.astral.sh/ruff/rules/bad-quotes-docstring/). However, Q002 only checks that triple-quoted docstrings use the appropriate symbol; C0198 _also_ checks that docstrings are triple-quoted.

Using this as an example...
```python
"This is a bad docstring"

class MyError(Exception):
    '''This is also a bad docstring'''
```

```console
$ pylint --version
pylint 3.2.2
astroid 3.2.2
Python 3.11.4 | packaged by conda-forge | (main, Jun 10 2023, 18:08:41) [Clang 15.0.7 ]
$ pylint demo.py
************* Module demo
demo.py:1:0: C0198: Bad docstring quotes in module, expected """, given " (bad-docstring-quotes)
demo.py:3:0: C0198: Bad docstring quotes in class, expected """, given ''' (bad-docstring-quotes)

------------------------------------------------------------------
Your code has been rated at 0.00/10 (previous run: 0.00/10, +0.00)
```

```console
$ ruff --version
ruff 0.5.5
$ ruff check demo.py --isolated --config 'lint.select = ["Q"]'
demo.py:4:5: Q002 [*] Single quote docstring found but double quotes preferred
  |
3 | class MyError(Exception):
4 |     '''This is also a bad docstring'''
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Q002
  |
  = help: Replace single quotes docstring with double quotes

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

Other keywords I couldn't find results for:
* docstring single quote
* add triple quote

---

_Comment by @charliermarsh on 2024-08-02 21:40_

I believe that's enforced by [`D300`](https://docs.astral.sh/ruff/rules/triple-single-quotes/). Does that work for your use-case?

---

_Comment by @charliermarsh on 2024-08-02 21:40_

https://play.ruff.rs/6253b93e-ac15-4a95-a745-7e467f451e66

---

_Label `question` added by @charliermarsh on 2024-08-02 21:40_

---

_Label `docstring` added by @charliermarsh on 2024-08-02 21:40_

---

_Comment by @nwiltsie on 2024-08-05 16:24_

@charliermarsh Spectacular, that solves it! Thank you!

Leaving notes for my future self stumbling across this again:

* There's a difference between `ruff format` and `ruff check --fix` (https://github.com/astral-sh/ruff/discussions/9048)
* The formatter won't add the triple-quotes ([example](https://play.ruff.rs/6253b93e-ac15-4a95-a745-7e467f451e66?secondary=Format)).




---

_Closed by @nwiltsie on 2024-08-05 16:24_

---
