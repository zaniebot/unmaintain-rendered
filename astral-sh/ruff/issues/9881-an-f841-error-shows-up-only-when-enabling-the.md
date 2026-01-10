```yaml
number: 9881
title: An F841 error shows up only when enabling the preview mode
type: issue
state: closed
author: suhaibmujahid
labels: []
assignees: []
created_at: 2024-02-07T21:36:16Z
updated_at: 2024-02-07T21:55:03Z
url: https://github.com/astral-sh/ruff/issues/9881
synced_at: 2026-01-10T11:09:52Z
```

# An F841 error shows up only when enabling the preview mode

---

_Issue opened by @suhaibmujahid on 2024-02-07 21:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Currently, `F841` is not in preview. However, it will catch the following unused variable (i.e., `secund`) only when the preview mode is enabled.

## Minimal code snippet
```py
class Foo:
    def bar(self):
        return 1, 2

    def main(self):
        first, secund = self.bar()

        print(first)
```

## Invoked commands

The following command shows no errors:
```sh
ruff ruff_F841.py --isolated
```

While enabling the preview shows an `F841` error:
```sh
ruff ruff_F841.py --isolated --preview
```
```
ruff_F841.py:6:16: F841 Local variable `secund` is assigned to but never used
  |
5 |     def main(self):
6 |         first, secund = self.bar()
  |                ^^^^^^ F841
7 | 
8 |         print(first)
  |
  = help: Remove assignment to unused variable `secund`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

## The current Ruff settings 

Just the defaults with no customization

## The current Ruff version
```
ruff 0.2.1
```


---

_Comment by @charliermarsh on 2024-02-07 21:49_

Hey sorry -- this is intended and it's covered in the rule documentation: https://docs.astral.sh/ruff/rules/unused-variable/. Right now, we don't lint against unused variables in tuples if at least one of them is used, but we're considering changing that behavior (hence why the revised logic is present in preview).

---

_Closed by @charliermarsh on 2024-02-07 21:49_

---

_Comment by @charliermarsh on 2024-02-07 21:50_

(For completeness, there's also a separate issue here discussing whether / how to stabilize that change: https://github.com/astral-sh/ruff/issues/8884.)

---

_Comment by @suhaibmujahid on 2024-02-07 21:54_

I appreciate the clarification. 

BTW, this behaviour does not respect the `explicit-preview-rules` option. When having `explicit-preview-rules = true`, I was expecting to not have any preview behaviour unless the rule is explicitly selected.

---
