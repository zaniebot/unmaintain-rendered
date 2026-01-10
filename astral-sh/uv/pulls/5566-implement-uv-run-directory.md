```yaml
number: 5566
title: "Implement `uv run --directory`"
type: pull_request
state: merged
author: bluss
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: run
created_at: 2024-07-29T17:41:33Z
updated_at: 2024-07-29T20:01:18Z
url: https://github.com/astral-sh/uv/pull/5566
synced_at: 2026-01-10T13:37:23Z
```

# Implement `uv run --directory`

---

_Pull request opened by @bluss on 2024-07-29 17:41_

## Summary

uv run --directory <path> means that one doesn't have to change to a
project's directory to run programs from it. It makes it possible to use
projects as if they are tool installations.

To support this, first the code reading .python-version was updated so that
it can read such markers outside the current directory. Note the minor
change this causes (if I'm right), described in the commit.

## Test Plan

One test has been added.

## --directory

Not sure what the name of the argument should be, but it's following uv
sync's directory for now.

Other alternatives could be "--project". Uv run and uv tool run should
probably find common agreement on this (relevant for project-locked tools).

I've implemented this same change in Rye, some time ago, and then we went
with --pyproject `<`path to pyproject.toml file`>`. I think using
pyproject.toml file path and not directory was probably a mistake, an
overgeneralization one doesn't need.


---

_@charliermarsh approved on 2024-07-29 17:43_

---

_Comment by @bluss on 2024-07-29 18:25_

A test had to be fixed for windows but is now done.

Edit - oh, needs preview label probably.

---

_Label `enhancement` added by @charliermarsh on 2024-07-29 18:59_

---

_Label `preview` added by @charliermarsh on 2024-07-29 18:59_

---

_Renamed from "Implement uv run --directory" to "Implement `uv run --directory`" by @charliermarsh on 2024-07-29 18:59_

---

_Merged by @charliermarsh on 2024-07-29 19:53_

---

_Closed by @charliermarsh on 2024-07-29 19:53_

---

_Branch deleted on 2024-07-29 20:01_

---
