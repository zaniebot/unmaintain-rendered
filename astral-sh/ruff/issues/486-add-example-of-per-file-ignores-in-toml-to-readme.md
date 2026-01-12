```yaml
number: 486
title: Add example of per-file-ignores in TOML to Readme
type: issue
state: closed
author: tgross35
labels: []
assignees: []
created_at: 2022-10-27T16:35:54Z
updated_at: 2022-10-27T17:10:27Z
url: https://github.com/astral-sh/ruff/issues/486
synced_at: 2026-01-12T15:54:40Z
```

# Add example of per-file-ignores in TOML to Readme

---

_@tgross35_

Hello,

It took quite a few attempts to get the syntax right for configuring `per-file-ignores` in toml. Would you be able to add a quick example to the readme to help guide usage?

```toml
[tool.ruff]
per-file-ignores = [
    "**/__init__.py:F401",
    "directory/file.py:F401"
]
```

Side note - for toml configuration, could you consider allowing whitespace after the colon? `"**/__init__.py: F401"` is ever so slightly more readable, and matches the flake8 ini config a little bit closer.

Alternatively, it could be nice to be able to specify ignores in a more toml-y way

```toml
[tools.ruff.per-file-ignores]
"**/__init__.py" = ["F401",  "D100"]
"directory/file.py" = ["F401", "C409",  "B014"]

# or the other way around
[tools.ruff.per-file-ignores]
F401 = [
    "**/__init__.py",
    "directory/file.py"
]
```

---

_Closed by @charliermarsh on 2022-10-27 16:58_

---

_Comment by @charliermarsh on 2022-10-27 17:00_

Good suggestions all around! I haven't changed the actual syntax (though I do like the suggestion)... though for now I went ahead and (1) supported whitespace as requested, and (2) added an example to the README.

As an aside, you _should_ be able to do:

```toml
[tool.ruff]
per-file-ignores = [
    "__init__.py:F401",  # No need for ** -- this should match any file named `__init__.py`
    "directory/file.py:F401"
]
```

---

_Comment by @tgross35 on 2022-10-27 17:05_

Awesome! Thanks for the super quick update

---

_Comment by @charliermarsh on 2022-10-27 17:09_

Always!

---

_Comment by @charliermarsh on 2022-10-27 17:10_

(Going out in v0.0.86, which is building now.)

---
