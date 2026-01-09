---
number: 10268
title: "Unnecessary \"ANN101 Missing type annotation for `self` in method\""
type: issue
state: closed
author: artemiogr97
labels: []
assignees: []
created_at: 2024-03-07T08:38:06Z
updated_at: 2024-03-07T11:27:11Z
url: https://github.com/astral-sh/ruff/issues/10268
synced_at: 2026-01-07T13:12:15-06:00
---

# Unnecessary "ANN101 Missing type annotation for `self` in method"

---

_Issue opened by @artemiogr97 on 2024-03-07 08:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Given the following code contained in my_file.py:


```
class MyClass:
    def do_something(self, my_float) -> int:
        return int(my_float)
```

running "ruff check --select ANN ./my_file.py" gives the following output:

test_ann101.py:2:22: ANN101 Missing type annotation for `self` in method
Found 1 error.

This is useless in most cases, in fact ruff extension for vscode does not show this error.

---

_Comment by @trag1c on 2024-03-07 08:55_

ANN101 has been deprecated in [v0.2.0](https://github.com/astral-sh/ruff/releases/tag/v0.2.0), perhaps you're using a different version of Ruff in your command line?

---

_Comment by @dhruvmanila on 2024-03-07 09:00_

Hey, thanks for opening this issue. As mentioned by @trag1c the rule has been [deprecated](https://docs.astral.sh/ruff/rules/missing-type-self/) and will be removed in a future release. 

> in fact ruff extension for vscode does not show this error.

It must be that you're running a different Ruff version via the command-line and in VS Code. In case you need to use an old Ruff version, you can also [ignore the rule](https://docs.astral.sh/ruff/settings/#lint_ignore) with:

```toml
[tool.ruff.lint]
ignore = ["ANN101"]
```

---

_Closed by @dhruvmanila on 2024-03-07 09:00_

---

_Comment by @artemiogr97 on 2024-03-07 11:27_

I just verified, both versions match, but it seems like it is the vscode extension that somehow excludes deprecated rules, in both cases I'm using the same configuration file which contains:
```
[tool.ruff.lint]
select= ["ANN"]
```
So all rules starting with "ANN" are activated
Is there a way to automatically exclude deprecated rules in the CLI ?
Ideally I would like to keep the same configuration file for both cases, and modifying the file to explicitly ignore deprecated files could be annoying

---
