---
number: 8856
title: Sync virtual environment from script inline metadata
type: issue
state: closed
author: tcoliver
labels:
  - duplicate
assignees: []
created_at: 2024-11-06T06:22:43Z
updated_at: 2024-11-06T15:38:03Z
url: https://github.com/astral-sh/uv/issues/8856
synced_at: 2026-01-07T13:12:18-06:00
---

# Sync virtual environment from script inline metadata

---

_Issue opened by @tcoliver on 2024-11-06 06:22_

I'd love to see a way to directly create a venv from a script's inline metadata. Perhaps a `uv sync --script myscript.py` option would work. 

The use case is to simplify setting up development environments when working in a repository with one or more scripts with inline metadata. Commonly, IDE's have support for utilizing a project's .venv when resolving import paths and running dev tooling. This is easy to configure if your repository is a standard Python project. However, for a directory of scripts (which may each have distinct dependencies), venvs must be manually configured before working on any particular script in order to gain the benefit of these tools. 

My current work around is to define a top level pyproject.toml in virtual project mode with a union of all the scripts dependencies. However, this is error prone in a repo with more than a trivial number of scripts. Also, it's possible to run into version conflicts between script dependency requirements. 

Being able to quickly sync the .venv with a scripts requirements would give a quick and easy way to ensure a compatible IDE experience when working in project-less repositories.

---

_Comment by @zanieb on 2024-11-06 15:38_

This is basically a duplicate of https://github.com/astral-sh/uv/issues/6637

Thanks for explaining your use-case!

---

_Closed by @zanieb on 2024-11-06 15:38_

---

_Label `duplicate` added by @zanieb on 2024-11-06 15:38_

---
