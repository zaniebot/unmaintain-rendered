```yaml
number: 22714
title: "Bug: docstring-code-line-length does not work."
type: issue
state: closed
author: vaibhavmano
labels:
  - question
assignees: []
created_at: 2026-01-19T08:36:16Z
updated_at: 2026-01-21T06:45:48Z
url: https://github.com/astral-sh/ruff/issues/22714
synced_at: 2026-01-21T06:53:55Z
```

# Bug: docstring-code-line-length does not work.

---

_@vaibhavmano_

### Summary

`docstring-code-line-length` is not working as intended even after enabling `docstring-code-format`


### Existing ruff configuration

This is my current ruff configuration mentioned in the root `pyproject.toml` file. 

```
[tool.ruff]
line-length = 120
extend-exclude = ["*/constants/api.py"]
exclude = ["test", "tests"]

[tool.ruff.format]
docstring-code-format = true
docstring-code-line-length = 120
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false

[tool.ruff.lint]
# Ruff Rules - https://docs.astral.sh/ruff/rules/#legend
select = ["E", "W", "F", "S", "I"]
ignore = ["F401", "W605", "W191"]
unfixable = ["F401", "S", "W605"]

[tool.ruff.lint.isort.sections]
kf-base = ["base"]  # base should always be on top
kf-libraries = [
    "analyticscore",
    "axiom",
    "botbase",
    "connectionbase",
    "core",
    "platformbase",
    "reportcore",
    "aibase",
]
kf-modules = [
    "constant",
    "constants",
    "engine",
    "error",
    "job",
    "main",
    "migration",
    "preview",
    "querybuilder",
    "route",
    "schema",
    "schemas",
    "server",
    "service_config",
    "services",
    "service",
    "util",
    "utils",
    "config"
]
kf-test-modules = ["tests", "test"]

[tool.ruff.lint.isort]
combine-as-imports = true
section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "kf-base",
    "kf-libraries",
    "kf-modules",
    "kf-test-modules",
    "local-folder",
]

[tool.ruff.lint.pycodestyle]
max-line-length = 120
```

As specified, I have mentioned both `docstring-code-format` as `true` and `docstring-code-line-length = 120`. The expectation is for the contents of the docstring to be wrapped within the line length.

Before formatting
```
    def validate_csv(
            self, form_id: str, payload: dict, max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,
    ):
        """
        Something about `f`. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
        """
```

Preview while running `uv run ruff format import_csv.py --diff`

```
     def validate_child_table_csv(
-            self, form_id: str, payload: dict, max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,
+        self,                                                                                                                              
+        form_id: str,                                                                                                                      
+        payload: dict,                                                                                                                     
+        max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,                                                                       
     ):   
```

I understand this formatting is due to the rule `skip-magic-trailing-comma` but I also assumed that the docstring formatting will show up.

### Version

ruff 0.14.13

---

_Label `question` added by @MichaReiser on 2026-01-19 08:44_

---

_Comment by @MichaReiser on 2026-01-19 08:44_

Can you say more about how you would expect

```py
    def validate_csv(
            self, form_id: str, payload: dict, max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,
    ):
        """
        Something about `f`. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam
        """
``` 

to be formatted? Ruff's `docstring-code-format` option enables formatting of code snippets within docstrings. It doesn't enable automatically breaking your docstring to fit into the line length.

For example:

```py
def test():
    """
    Something about `f`. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam

    ```
    class Test:
        def validate_csv(
            self, form_id: str, payload: dict, 
            max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,
        ): ...
    ```
    """
```


gets formatted as 

```py
def test():
    """
    Something about `f`. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam

    ```
    class Test:
        def validate_csv(
            self,
            form_id: str,
            payload: dict,
            max_limit: int = ChildTableImportConstants.MAX_RECORD_LIMIT,
        ): ...
    ```
    """

```

---

_Comment by @vaibhavmano on 2026-01-21 06:45_

Thanks for the reply @MichaReiser. Looks like I've got it confused with the line-length. I'm assuming all of this overrides the root `line-length` configuration.

---

_Closed by @vaibhavmano on 2026-01-21 06:45_

---
