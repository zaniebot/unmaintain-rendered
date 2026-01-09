---
number: 1950
title: "W605 isn't being detected in a monorepository with legacy code."
type: issue
state: closed
author: ljesparis
labels:
  - question
assignees: []
created_at: 2023-01-18T08:55:38Z
updated_at: 2023-01-18T19:35:30Z
url: https://github.com/astral-sh/ruff/issues/1950
synced_at: 2026-01-07T13:12:14-06:00
---

# W605 isn't being detected in a monorepository with legacy code.

---

_Issue opened by @ljesparis on 2023-01-18 08:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Hello i'm comparing **flake8 output** vs **ruff output** in a monorepository with legacy code.

#### Flake8 output 

```shell
src/../../../file.py:14:68: W605 invalid escape sequence '\w'
src/../../../file.py:9:39: W605 invalid escape sequence '\{'
src/../../../file.py:9:46: W605 invalid escape sequence '\}'
src/../../../file.py:10:53: W605 invalid escape sequence '\W'
```

#### Ruff output 

nothing

#### A minimal code snippet that reproduces the bug.

```python
def test_function(value: str) -> None:
    re.findall("\{(.*?)\}", value)
    re.findall("({\w*.|{)", value)
    re.findall("(.\W*}|})", value)
```

#### The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```shell
ruff \ 
    --ignore E501,F821 \
    --target-version py38 \
    --line-length 120 \
    --respect-gitignore \
    --exclude 'src/*/tests/*','src/*/migrations/','src/**/_dependency_injection/*','__init__.py','models.py','admin.py' \
    src
```

#### The current Ruff version (`ruff --version`).
```shell
$ ruff --version                                                                                                                                                                                   !7148
ruff 0.0.224
```

---

_Label `bug` added by @charliermarsh on 2023-01-18 12:07_

---

_Comment by @charliermarsh on 2023-01-18 12:48_

I actually do see the same thing between tools! Maybe a configuration issue? We don't enable the `W` rules by default, so you may need `--select E,F,W` in your command.

![Screen Shot 2023-01-18 at 7 48 03 AM](https://user-images.githubusercontent.com/1309177/213175472-1bc93a50-e31b-4ec9-829e-3fffcab99960.png)


---

_Label `question` added by @charliermarsh on 2023-01-18 12:48_

---

_Label `bug` removed by @charliermarsh on 2023-01-18 12:48_

---

_Comment by @charliermarsh on 2023-01-18 19:35_

Tentatively closing, but please let me know if you continue to see this.

---

_Closed by @charliermarsh on 2023-01-18 19:35_

---
