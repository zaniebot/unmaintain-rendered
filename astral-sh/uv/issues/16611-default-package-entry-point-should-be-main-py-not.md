```yaml
number: 16611
title: "Default package entry point should be `__main__.py` not `__init__.py`"
type: issue
state: open
author: vepain
labels:
  - enhancement
assignees: []
created_at: 2025-11-06T11:23:28Z
updated_at: 2025-11-06T15:29:31Z
url: https://github.com/astral-sh/uv/issues/16611
synced_at: 2026-01-12T16:02:34Z
```

# Default package entry point should be `__main__.py` not `__init__.py`

---

_@vepain_

### Summary

At least in the context of a package, the application entry point should be the file `__main__.py` (refer to https://docs.python.org/3.14/library/__main__.html#main-py-in-python-packages)

Changes:

- Add a `__main__.py` in the directory `src/module_name`

  ```py
  # src/module_name/__main__.py
  def main() -> None:
     """Run the application."""
     print("Hello world!")
  
  if __name__ == "__main__":
    main()
  ```

- Modify the project script path in the `project.toml` file

  ```toml
  [project.scripts]
    module_name = "module_name:__main__.main"
  ```

### Example

Default behaviour of `uv init --package my-package`

---

_Label `enhancement` added by @vepain on 2025-11-06 11:23_

---

_Comment by @zanieb on 2025-11-06 13:03_

I think it's quite common to do a `__main__.py` that imports a `main` from another module and executes it.

The `__main__` module is only the entry point for `python -m <module>`.

The idiomatic usage in the linked documentation says

>  [...] those files are kept short and import functions to execute from other modules. Those other modules can then be easily unit-tested and are properly reusable.

You could argue we should generate a `__main__.py`, but I'm not convinced the `main` function should move.

---

_Comment by @vepain on 2025-11-06 14:17_

I proposed this in fact to keep separated the library and the CLI entry point (for example) parts of a package.
I will follow the idiomatic usages.

---
