---
number: 14212
title: "ignores `# fmt: off` for `E501`"
type: issue
state: closed
author: beltekylevi
labels: []
assignees: []
created_at: 2024-11-08T22:29:37Z
updated_at: 2024-11-08T22:50:06Z
url: https://github.com/astral-sh/ruff/issues/14212
synced_at: 2026-01-07T13:12:16-06:00
---

# ignores `# fmt: off` for `E501`

---

_Issue opened by @beltekylevi on 2024-11-08 22:29_

A minimal code snippet that reproduces the bug.
```py
# zeus/urls.py
def path(path: str, handler):
    return

def calculate_project_request_proposed_mentors():
    return

urlpatterns = [
    # fmt: off
    path("project-request/<int:project_request_id>/calculate-proposed-mentors/", calculate_project_request_proposed_mentors),  # noqa: E501
    # fmt: on
]
```

 The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```shell
> poetry run ruff format zeus/urls.py --isolated
1 file reformatted
```

🙂
```py
def path(path: str, handler):
    return


def calculate_project_request_proposed_mentors():
    return


urlpatterns = [
    # fmt: off
    path(
        "project-request/<int:project_request_id>/calculate-proposed-mentors/",
        calculate_project_request_proposed_mentors,
    ),  # noqa: E501
    # fmt: on
]

```

The current Ruff settings (any relevant sections from your `pyproject.toml`).
```shell
> pyproject.toml | grep line-length      
line-length = 120
```

The current Ruff version (`ruff --version`).
```shell
> poetry run ruff --version
ruff 0.7.3


---

_Comment by @beltekylevi on 2024-11-08 22:38_

```py
# fmt: off
def path(path: str, handler):
    return


def calculate_project_request_proposed_mentors():
    return


urlpatterns = [
    path("project-request/<int:project_request_id>/calculate-proposed-mentors/", calculate_project_request_proposed_mentors),  # noqa: E501
]
```
```shell
> poetry run ruff format zeus/urls.py       
1 file left unchanged
```

---

_Comment by @beltekylevi on 2024-11-08 22:50_

I'm stupid. But your documentation doesn't emphasize that you are writing examples that are not working. Probably you should improve on that.

---

_Closed by @beltekylevi on 2024-11-08 22:50_

---
