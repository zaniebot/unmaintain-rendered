---
number: 12256
title: "`pyproject.toml` template manager"
type: issue
state: open
author: sarpuser
labels:
  - enhancement
assignees: []
created_at: 2025-03-18T01:19:38Z
updated_at: 2025-09-12T18:34:34Z
url: https://github.com/astral-sh/uv/issues/12256
synced_at: 2026-01-07T13:12:18-06:00
---

# `pyproject.toml` template manager

---

_Issue opened by @sarpuser on 2025-03-18 01:19_

### Summary

Would it be possible to add some sort of template manager for pyproject.toml? I find myself copying and pasting my tools settings for things like ruff, mypy, etc a lot. It would be really nice if we could have a template manager of sorts that we could pass into `uv init`. 

### Example

Here is how I think this should work.

- There should be a default uv templates folder. I am thinking `~/.config/uv/templates` would be a good default.
- The templates would have all the sections besides the things that `uv init` controls. For example, it might have the ruff config, the mypy config etc. The filename of these templates would be the template name.
- When running `uv init`, we could pass in a template by doing something like `uv init --template <template_name>`. Instead of passing in a template name, we could also pass in a filepath to a template.
- It would also be nice to pass in multiple templates so we can selectively add config options.
- When running init, uv would take the information from the templates, and add it to the generated pyproject.toml. Any options such as build-system, project, and dependencies should be ignored if present within the template.

Example:
```toml
# ~/.config/uv/templates/pytest.toml
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
addopts = "-rA --tb=line"
``` 

```toml
# ~/example/ruff.toml
[tool.ruff]
# Target version
target-version = "py311"

extend-exclude = ["__init__.py"]

# Line length
line-length = 88
```

Then, running `uv init example-project --template pytest --template ~/example/ruff.toml` would generate the following pyproject.toml (not sure what the best approach is for multiple templates):
```toml
[project]
name = "example-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
addopts = "-rA --tb=line"

[tool.ruff]
# Target version
target-version = "py311"

extend-exclude = ["__init__.py"]

# Line length
line-length = 88
```

I think this would be a massive quality of life increase. Thank you for your consideration and all your effort into this amazing project.


---

_Label `enhancement` added by @sarpuser on 2025-03-18 01:19_

---

_Comment by @sebastianw on 2025-09-12 18:34_

I would love this, I'm copying ruff config by hand for new projects all the time.

---
