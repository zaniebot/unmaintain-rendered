---
number: 204
title: "Add `.gitignore` with `*` as contents to `.ruff_cache`"
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-15T16:48:40Z
updated_at: 2023-02-03T18:28:28Z
url: https://github.com/astral-sh/ruff/issues/204
synced_at: 2026-01-07T13:12:14-06:00
---

# Add `.gitignore` with `*` as contents to `.ruff_cache`

---

_Issue opened by @Jackenmen on 2022-09-15 16:48_

This is done by a lot of popular tools such as pytest (`.pytest_cache/.gitignore`), mypy (`.mypy_cache/.gitignore`), or virtualenv (`venv-name/.gitignore`) to make it unnecessary to exclude these folders in the repository root's .gitignore and to make it easier to use those tools for projects whose maintainers don't use it themselves as well as for new projects who are just adopting the new tool and want to be able to do so with minimal effort.

All of these tools only do it if the folder didn't already exist so I guess that's the most logical choice for this.

---

_Label `enhancement` added by @charliermarsh on 2022-09-15 16:53_

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-15 16:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-09-15 22:04_

---

_Referenced in [astral-sh/ruff#208](../../astral-sh/ruff/pulls/208.md) on 2022-09-16 00:39_

---

_Closed by @charliermarsh on 2022-09-16 00:40_

---

_Referenced in [github/gitignore#4289](../../github/gitignore/pulls/4289.md) on 2023-07-03 07:36_

---

_Referenced in [astral-sh/uv#4219](../../astral-sh/uv/issues/4219.md) on 2024-06-10 21:32_

---
