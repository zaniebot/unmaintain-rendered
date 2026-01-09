---
number: 3649
title: Disable options specifically for .pyi files
type: issue
state: closed
author: PedroPerpetua
labels:
  - question
assignees: []
created_at: 2023-03-21T15:24:24Z
updated_at: 2023-03-21T18:54:41Z
url: https://github.com/astral-sh/ruff/issues/3649
synced_at: 2026-01-07T13:12:14-06:00
---

# Disable options specifically for .pyi files

---

_Issue opened by @PedroPerpetua on 2023-03-21 15:24_

Currently I'm having an issue in my project where I have a `./typing` folder that contains only stubs (`.pyi` files) alongside my project folders, in a structure similar to this:
```
src/
├── ...
├── ...
└── ...
typing/
└── module/
    └── file.pyi
pyproject.toml
```

I'm facing an issue specifically with a clash of _ruff's isort_ options and _black_; By setting `lines-after-imports = 2` under _ruff_, _black_ and _ruff_ will clash on the `.pyi` files, where _ruff_ will apply 2 lines between the imports and stubs, and _black_ will apply 1 line, conforming to the relevant PEP. This does not happen to the normal python files under `src`, where both tools leave 2 blank lines between the imports and code.

Is there a way to configure _ruff_ to also leave only one line in the `.pyi` files? If not, I think it would be useful to be able to configure for these files specifically, the same way you can set configurations specifically for `__init__.py` files.

Relevant configurations I use:
```toml
# pyproject.toml
[tool.ruff]
src = ["src", "typing"]
fix = true
select = ["F", "I", "W", "E"]

[tool.ruff.isort]
lines-after-imports = 2
no-lines-before = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]
```

Currently, my fix is to have a second `pyproject.toml` file under `./typing` with the following contents:
```toml
[tool.ruff]
extend = "../pyproject.toml"

[tool.ruff.isort]
# Stub files have a different default lines between imports
lines-after-imports = -1
no-lines-before = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]
```

Extra information:
- Ruff version: `0.0.257`

---

_Comment by @charliermarsh on 2023-03-21 16:20_

I don't think this is possible right now.

If you use `lines-after-imports = -1` in both places, we do treat `.pyi` files differently for defaults, and I believe we're consistent with Black too (in `.py` files, we add two lines-after-import if the trailing content is a function or class, etc). But there's no way to provide different settings for different file types.


---

_Comment by @PedroPerpetua on 2023-03-21 18:53_

Gotcha. I was having git differences when applying ruff to a project that was already running black; Turns out black was allowing (not enforcing) 2 blank lines after imports for ALL files, while ruff enforces only one blank line if there's not top level function / class definition.

---

_Closed by @PedroPerpetua on 2023-03-21 18:53_

---

_Comment by @charliermarsh on 2023-03-21 18:54_

Yeah I've become intimately familiar with Black's rules :joy: At the top-level, they _allow_ up to two blank lines, but they only _enforce_ two blank lines in certain contexts.

---

_Label `question` added by @charliermarsh on 2023-03-21 18:54_

---
