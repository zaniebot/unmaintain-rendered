---
number: 2899
title: pre-commit does not pickup black line-length from pyproject.toml
type: issue
state: closed
author: quassy
labels: []
assignees: []
created_at: 2023-02-14T20:03:40Z
updated_at: 2023-02-14T20:53:05Z
url: https://github.com/astral-sh/ruff/issues/2899
synced_at: 2026-01-07T13:12:14-06:00
---

# pre-commit does not pickup black line-length from pyproject.toml

---

_Issue opened by @quassy on 2023-02-14 20:03_

Since #1317 ruff should pick up `line-length` from the `tool.black` section of pyproject.toml, but for me it does not:

* A minimal code snippet that reproduces the bug: Any Python file with a line that is longer than 88 characters. 

* The command you invoked:
Run from the main directory where `pyproject.toml` is located:
```
$ black .
All done! ✨ 🍰 ✨
15 files left unchanged.

$ ruff . | grep E501
fastapi-backend/models.py:33:89: E501 Line too long (112 > 88 characters)
fastapi-backend/models.py:55:89: E501 Line too long (109 > 88 characters)
fastapi-backend/routers/giveboxes.py:39:89: E501 Line too long (109 > 88 characters)
fastapi-backend/routers/images.py:29:89: E501 Line too long (94 > 88 characters)
fastapi-backend/routers/images.py:58:89: E501 Line too long (91 > 88 characters)
fastapi-backend/routers/user_box_association.py:12:89: E501 Line too long (89 > 88 characters)
fastapi-backend/routers/user_box_association.py:13:89: E501 Line too long (97 > 88 characters)
fastapi-backend/routers/user_box_association.py:14:89: E501 Line too long (90 > 88 characters)
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`):
```
# Python
[tool.black]
line-length = 120
target_version = ["py311"]

[tool.ruff]
# line-length = 120
extend-ignore = [
    "ANN002", "ANN003", "ANN101", "ANN102", "D103", "D100", "D101",
    "D203", "D204", "D213", "D215", "D400", "D404", "D406", "D407", "D408", "D409", "D413",
]  # "E203", "B001", "W503", "R504"
exclude = [".git", "__pycache__", ".mypy_cache", ".pytest_cache", ".venv", ".cache", "test/"]
extend-select = ["E", "F", "ANN", "S", "B", "A", "RET", "D"]

[tool.ruff.flake8-annotations]
mypy-init-return = true

[tool.ruff.per-file-ignores]
"tests/test_" = ["D"]
"*/__init__.py" = ["D104"]
```

* The current Ruff version (`ruff --version`): 0.0.245 installed via Homebrew, alternatively 0.0.246 as a pre-commit.com hook.

* Similar issues: #1462 #1311

---

_Comment by @charliermarsh on 2023-02-14 20:42_

Ahhh sorry -- #1317 is actually about the `flake8-to-ruff` command-line tool, which we ship separately here: https://pypi.org/project/flake8-to-ruff/.

That tool will _generate_ a `pyproject.toml` for you, and it'll pick up the line-length settings that you've provided to Black, but you still have to repeat them `line-length`.

So I think this is working as intended, even if it's a little inconvenient to duplicate the two.


---

_Closed by @charliermarsh on 2023-02-14 20:42_

---

_Comment by @quassy on 2023-02-14 20:48_

Thanks! I missed that when reading. Then I'll just wait until ruff supports all black formatting to drop the black section one day 😉

---

_Comment by @charliermarsh on 2023-02-14 20:53_

I assume you're quassy7 from Twitter -- if so, hi!

---
