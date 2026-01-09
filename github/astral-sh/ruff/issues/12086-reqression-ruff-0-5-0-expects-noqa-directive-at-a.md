---
number: 12086
title: "Reqression: ruff 0.5.0 expects noqa directive at a different line"
type: issue
state: closed
author: anz-ableton
labels:
  - documentation
assignees: []
created_at: 2024-06-28T06:47:26Z
updated_at: 2024-06-28T11:41:47Z
url: https://github.com/astral-sh/ruff/issues/12086
synced_at: 2026-01-07T13:12:15-06:00
---

# Reqression: ruff 0.5.0 expects noqa directive at a different line

---

_Issue opened by @anz-ableton on 2024-06-28 06:47_

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

I've searched for the following keywords before opening this issue:

- `noqa`,
- `S603`, `S602`
- `subprocess`

Ruff in version `0.4.10` will not complain about the following snippet:

```python
def run(a_long_command_to_make_this_wrap_at_the_line_end, use_shell):
    subprocess.run(
        a_long_command_to_make_this_wrap_at_the_line_end,
        capture_output=True,
        check=True,
        shell=use_shell,  #  noqa: S603
    )
```

However, when using `0.5.0` it will complain with

```bash
$ ruff check --select S .
ruff_s603_regression.py:5:5: S603 `subprocess` call: check for execution of untrusted input
  |
4 | def run(a_long_command_to_make_this_wrap_at_the_line_end, use_shell):
5 |     subprocess.run(
  |     ^^^^^^^^^^^^^^ S603
6 |         a_long_command_to_make_this_wrap_at_the_line_end,
7 |         capture_output=True,
  |

Found 1 error.
```

Moving the `noqa` directive to the line where `subprocess.run` is called will make it succeed:

```diff
diff --git a/ruff_s603_regression.py b/ruff_s603_regression.py
index eda9e6c..306eb56 100644
--- a/ruff_s603_regression.py
+++ b/ruff_s603_regression.py
@@ -2,11 +2,11 @@ import subprocess
 
 
 def run(a_long_command_to_make_this_wrap_at_the_line_end, use_shell):
-    subprocess.run(
+    subprocess.run(  #  noqa: S603
         a_long_command_to_make_this_wrap_at_the_line_end,
         capture_output=True,
         check=True,
-        shell=use_shell,  #  noqa: S603
+        shell=use_shell,
     ) 
```

For completeness, below is the content of the full example file:

```python
import subprocess


def run(a_long_command_to_make_this_wrap_at_the_line_end, use_shell):
    subprocess.run(  #  noqa: S603
        a_long_command_to_make_this_wrap_at_the_line_end,
        capture_output=True,
        check=True,
        shell=use_shell,
    )


run("echo 'Hello, World!'", True)
```

---

_Comment by @MichaReiser on 2024-06-28 06:52_

Hi. 

It is documented in the changelog under rule changes but I should have mentioned it in the breaking changes section, to make it more visible. Let me update the changelog real quick

> [flake8-bandit] Modify diagnostic ranges for shell-related rules (https://github.com/astral-sh/ruff/pull/10667)

You can use `--add-noqa` and https://docs.astral.sh/ruff/rules/unused-noqa/ to automatically move the noqa comments (add the new once and remove the no longer necessary comments)

---

_Label `documentation` added by @MichaReiser on 2024-06-28 06:54_

---

_Comment by @anz-ableton on 2024-06-28 07:07_

@MichaReiser Thanks for the swift response and for updating the changelog! I did not know about the `--add-noqa` flag, that's pretty helpful! Will you take care of closing this issue once the changelog got updated?

---

_Referenced in [astral-sh/ruff#12090](../../astral-sh/ruff/pulls/12090.md) on 2024-06-28 11:38_

---

_Closed by @charliermarsh on 2024-06-28 11:41_

---

_Closed by @charliermarsh on 2024-06-28 11:41_

---
