```yaml
number: 1812
title: How to not parse a nested pyproject.toml?
type: issue
state: closed
author: gbolmier
labels:
  - bug
assignees: []
created_at: 2023-01-12T10:06:57Z
updated_at: 2023-01-13T08:33:51Z
url: https://github.com/astral-sh/ruff/issues/1812
synced_at: 2026-01-12T15:54:41Z
```

# How to not parse a nested pyproject.toml?

---

_@gbolmier_

I love this project, keep up the good work ❤️ 

```sh
$ ruff --version
ruff 0.0.218
$ python --version
Python 3.8.15
```

I'm developing a Cookiecutter template which gives me the following tree structure:

```
.
├── ...
├── pyproject.toml
└── {{ cookiecutter.repo_name }}
    ├── pyproject.toml
    └── ...
```

where the nested `pyproject.toml` has Jinja2 placeholders causing parsing errors:

```sh
$ ruff .
error TOML parse error at line 26, column 2
   |
26 | {% if cookiecutter.use_mypy == "Yes" %}
   |  ^
Invalid inline table
Expected `}`
```

Using the `config` flag to only use the root level `pyproject.toml` doesn’t prevent the nested `pyproject.toml` to be parsed. Is there a way to not parse it?


---

_Label `bug` added by @charliermarsh on 2023-01-12 12:15_

---

_Comment by @charliermarsh on 2023-01-12 12:15_

Ahh interesting. Yeah, we should fix this. I have to look back at the code. We’re probably parsing it out of convenience (due to some specific details related to the abstractions).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-12 15:41_

---

_Closed by @charliermarsh on 2023-01-12 18:15_

---

_Comment by @charliermarsh on 2023-01-12 18:21_

(This should be fixed in the next release (v0.0.220), which will go out later today. Would appreciate if you could test and confirm once it's live!)

---

_Comment by @gbolmier on 2023-01-13 08:33_

Thank you so much for the blazing fast fix @charliermarsh 🙏
`ruff --config=pyproject.toml .` now works like a charm 🚀 

---
