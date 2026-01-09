---
number: 4759
title: D403 not being automatically fixed
type: issue
state: closed
author: alexdauenhauer
labels:
  - question
assignees: []
created_at: 2023-05-31T15:39:16Z
updated_at: 2023-06-01T01:15:17Z
url: https://github.com/astral-sh/ruff/issues/4759
synced_at: 2026-01-07T13:12:14-06:00
---

# D403 not being automatically fixed

---

_Issue opened by @alexdauenhauer on 2023-05-31 15:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

According to the [pydocstyle section](https://beta.ruff.rs/docs/rules/#pydocstyle-d) code D403 is fixable, but it does not get fixed for me using the vs code extension or command line

**code**
```python
    @abstractproperty
    def features(self) -> Dict[str, Any]:
        """get features required for model inference and wrap in a dictionary"""
```

**expected**
I expected the first word in the docstring to be capitalized.

When I run flake8 it flags D403 on this line. when I run ruff either with the VS Code extension or on the command line via `ruff check` it does not flag this line and it does not auto fix it

**ruff version**
vs code extension v2023.16.0

**ruff settings**
```json
"ruff.args": [
        "--line-length=100",
],
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-31 15:48_

---

_Comment by @charliermarsh on 2023-05-31 15:50_

The `D` rules aren't enabled by default, is it possible that the `D` rules haven't been enabled at all for your project?

For example, if I run with `--isolated`, I can get Ruff to fix that violation:

```shell
❯ ruff check foo.py --isolated --select D403 --fix
Found 1 error (1 fixed, 0 remaining).
```

---

_Label `question` added by @charliermarsh on 2023-05-31 15:50_

---

_Comment by @alexdauenhauer on 2023-05-31 21:30_

aha I see. Ok I think the docs say D is enabled by default [here](https://beta.ruff.rs/docs/settings/#fixable). At least I think that is what it says, since I see "D" in the list of default enabled rules. What settings would I add to the vs code extension to autofix this? When I add the args in your comment I get a `JSONDecodeError`

---

_Comment by @alexdauenhauer on 2023-05-31 21:51_

@charliermarsh oh I see I was misunderstanding the flags. `select` is what I should have been looking at instead of `fixable`. I was able to achieve the autofix with vs code by setting `--extend-select=D403`. If that looks right to you then I'll close this issue

---

_Comment by @charliermarsh on 2023-06-01 01:15_

Ahhh yes, sorry about that! It does sound right.

---

_Closed by @charliermarsh on 2023-06-01 01:15_

---
