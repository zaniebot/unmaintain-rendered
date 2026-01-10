```yaml
number: 3684
title: Ruff runs all pydocstyle rules when the convention is specified in the config toml files
type: issue
state: closed
author: felix-cw
labels:
  - bug
assignees: []
created_at: 2023-03-23T12:15:08Z
updated_at: 2023-03-23T17:01:41Z
url: https://github.com/astral-sh/ruff/issues/3684
synced_at: 2026-01-10T11:09:46Z
```

# Ruff runs all pydocstyle rules when the convention is specified in the config toml files

---

_Issue opened by @felix-cw on 2023-03-23 12:15_

This behaviour is new in 0.0.258

When running ruff with the `--select` option in a folder with a `pyproject.toml` or `ruff.toml` which specifies the pydocstyle convention,
ruff always runs all the pydocstyle rules.

Example:

```shell
# I want to check for unsorted imports
ruff check ruff_doc.py --select=I
```

`ruff_doc.py`

```python
"""Useful mathematical functions."""


def add_numbers(number1, number2):
    """Add 2 numbers.

    Args:
        number1 (Number): first number
        number2 (Number): second number

    Returns:
        Number: result of addition
    """
    return number1 + number2

```

`pyproject.toml`

```toml
[tool.ruff.pydocstyle]
convention = "google"
```

The result is 

```
ruff_doc.py:5:5: D213 [*] Multi-line docstring summary should start at the second line
ruff_doc.py:5:5: D407 [*] Missing dashed underline after section ("Args")
ruff_doc.py:5:5: D407 [*] Missing dashed underline after section ("Returns")
```

I do not expect any output, as I didn't select `D` in my command, and these error codes should be turned off in the google convention.

This behaviour is particularly bad in e.g. VSCode with the organize imports functionality, as it also tries to fix all docstring issues in all the conventions, which results in more errors.


---

_Comment by @JonathanPlasse on 2023-03-23 12:29_

`convention` enables the `D` rules corresponding to it implicitly. You should not set `convention`if you do not want to check the `D` rules.

For vscode, you should check you are not using fix all in your configuration.

---

_Comment by @felix-cw on 2023-03-23 12:38_

I am getting errors for rules D213 and D407, which are not included in the google convention per http://www.pydocstyle.org/en/stable/error_codes.html#default-conventions

Furthermore I am using `--select=I` but the 'D' rules are getting enabled as well. If I set `convention`, does that mean there is no way to prevent 'D' rules from being checked?

---

_Comment by @charliermarsh on 2023-03-23 13:48_

This seems like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-23 13:48_

---

_Comment by @charliermarsh on 2023-03-23 13:48_

Lemme look into it.

---

_Label `bug` added by @charliermarsh on 2023-03-23 13:51_

---

_Comment by @charliermarsh on 2023-03-23 13:56_

@MichaReiser - In case the bug here is obvious to you, it looks like this diff prevents us from accidentally enabling the `pydocstyle` rules:

```rs
diff --git a/crates/ruff/src/settings/mod.rs b/crates/ruff/src/settings/mod.rs
index 1fb944959..df3d11765 100644
--- a/crates/ruff/src/settings/mod.rs
+++ b/crates/ruff/src/settings/mod.rs
@@ -386,7 +386,9 @@ impl From<&Configuration> for RuleTable {
             .and_then(|pydocstyle| pydocstyle.convention)
         {
             for rule in convention.rules_to_be_ignored() {
-                rules.disable(*rule);
+                if rules.enabled(*rule) {
+                    rules.disable(*rule);
+                }
             }
         }
```

---

_Comment by @charliermarsh on 2023-03-23 13:57_

(In other words, calling `disable` on a rule that's not currently enabled seems to enable it.)

---

_Comment by @janosh on 2023-03-23 14:05_

This is definitely a bug since setting `pydocstyle.convention` in `pyproject.toml` even overrides CLI flags.

```
ruff --version                 
    ruff 0.0.257
ruff check . --ignore D | wc -l
       0

pip install ruff==0.0.258 && ruff check . --ignore D | wc -l
    19446
```

---

_Comment by @charliermarsh on 2023-03-23 14:42_

Yeah I've confirmed that it's a regression. I'll ship a fix today.

---

_Closed by @MichaReiser on 2023-03-23 17:01_

---
