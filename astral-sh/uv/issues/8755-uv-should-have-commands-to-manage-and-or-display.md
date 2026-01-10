---
number: 8755
title: uv should have commands to manage and/or display the version of the current project
type: issue
state: closed
author: tconbeer
labels:
  - duplicate
assignees: []
created_at: 2024-11-01T16:30:33Z
updated_at: 2024-11-02T02:08:53Z
url: https://github.com/astral-sh/uv/issues/8755
synced_at: 2026-01-10T01:24:32Z
---

# uv should have commands to manage and/or display the version of the current project

---

_Issue opened by @tconbeer on 2024-11-01 16:30_

In CD workflows, it's handy to have a tool that can read and write the version of the project in its `pyproject.toml` file.

For example, [`poetry version`](https://python-poetry.org/docs/cli/#version) (with no arguments) prints the project's version (in one of a few formats), and `poetry version x.y.z` updates the `pyproject.toml` file and sets `tool.poetry.version = x.y.z`.

I use both of these features in Harlequin's CD workflows, first to[ set the version](https://github.com/tconbeer/harlequin/blob/29faeb7f9d8ef4558269b1d8df5a5c34bf9de3a2/.github/workflows/release.yml#L54-L55) from the release branch name, and then to [create a GitHub release](https://github.com/tconbeer/harlequin/blob/29faeb7f9d8ef4558269b1d8df5a5c34bf9de3a2/.github/workflows/publish.yml#L50-L63) after the package is published. I would love to use uv instead.

(Poetry also includes a feature to bump the version based on a "bump rule" like `minor`, but I don't use that feature personally).

Today `uv version` and `uv --version` both display uv's version, not the project version.

This feature is similar to #5975 , and might be a subcommand of `uv info` or `uv show` (or `uv project`): `uv info version --short` etc. For CD use cases, it's important that the version number is easy to extract from the output.

---

_Comment by @zanieb on 2024-11-01 23:21_

This is a duplicate of https://github.com/astral-sh/uv/issues/6298

---

_Closed by @zanieb on 2024-11-01 23:21_

---

_Label `duplicate` added by @zanieb on 2024-11-01 23:21_

---

_Comment by @zanieb on 2024-11-01 23:21_

In brief, we want to support this.

---

_Comment by @tconbeer on 2024-11-02 02:08_

Sorry about the dupe. I searched and couldn't find it

---
