---
number: 12555
title: Compatibility with Poetry
type: issue
state: closed
author: Lyranile
labels:
  - question
assignees: []
created_at: 2025-03-30T08:07:22Z
updated_at: 2025-03-31T01:50:11Z
url: https://github.com/astral-sh/uv/issues/12555
synced_at: 2026-01-10T01:25:21Z
---

# Compatibility with Poetry

---

_Issue opened by @Lyranile on 2025-03-30 08:07_

# Question

I need uv to work alongside poetry using pyproject configuration
Also to manage installations, meaning making venv use either uv or poetry when necessary 
What's the best way to do that

## Background 
I have a lot of projects in my organization that are using poetry
To introduce uv I want to keep poetry dependencies in pyproject and gradually introduce uv as a new tool without breaking anything
It has to keep poetry working, I need both to function for the docker image to build until I change it everywhere
Then once they are onboard I'll need the switch to uv to happen seamlessly and then remove all references to poetry 

### Platform

macos 14

### Version

uv 0.6.10

---

_Label `question` added by @Lyranile on 2025-03-30 08:07_

---

_Comment by @FishAlchemist on 2025-03-30 08:41_

From what I remember, UVs don't usually specifically accommodate settings from third-party tools unless it's a standard.

**Related issues**:
* https://github.com/astral-sh/uv/issues/5200 (There are some suggestions related to your question in the comments.)

---

_Comment by @charliermarsh on 2025-03-30 15:03_

It's going to be challenging to use both at once, but at minimum you would need to upgrade to the latest Poetry and move all of your dependency definitions and metadata into `[project.dependencies]` rather than `[tool.uv.poetry]` so that they can read from the same source.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-30 15:08_

---

_Comment by @charliermarsh on 2025-03-30 15:09_

In other words, you want to migrate your Poetry setup such that you're using the standardized `[project]` section rather than anything Poetry-specific. Then `uv sync` would be able to do the same thing as `poetry install`.

---

_Comment by @Lyranile on 2025-03-30 16:24_

I want to have both configuration active so that I would be able to do poetry install and uv sync without conflicts, to make migration easier

---

_Comment by @charliermarsh on 2025-03-31 01:50_

Yeah, you can do that! Just put the configuration in a single `pyproject.toml` and you can use uv and Poetry interchangeably. I suggested migrating from `[tool.poetry]` to the standardized `[project]` table so that you don't have to duplicate your configuration, but it's not a requirement.

---

_Closed by @charliermarsh on 2025-03-31 01:50_

---
