---
number: 13594
title: "`uv` should remember if user previously ran `uv venv project-name`"
type: issue
state: closed
author: kdheepak
labels:
  - enhancement
assignees: []
created_at: 2025-05-22T13:53:17Z
updated_at: 2025-05-23T06:15:08Z
url: https://github.com/astral-sh/uv/issues/13594
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv` should remember if user previously ran `uv venv project-name`

---

_Issue opened by @kdheepak on 2025-05-22 13:53_

### Summary

Hi all,

`uv` has been great and I've been incorporating it into more of my workflows. 

There have been a couple of instances where the default is a little confusing for me, and it comes down to creating custom environments.

As an example, there are instances when I don't want to create a `.venv` in the current folder (the current folder is being synced and backed up for example, OR it is a read only folder location, OR I have limited disk space where the folder is located). In these cases, I've been able to run `uv venv ~/.virtualenvs/project-name` and that works great.

However after running `uv venv ~/.virtualenvs/project-name`, if a user ever happens to run `uv sync` or `uv run main.py` it recreates the `.venv` folder in the current location. 

I've accidentally done this enough times that I think there's probably a better way to deal with this. 

1) `uv venv project-name` should prompt to update `.env` so that dotenv handles it going forward by setting `VIRTUAL_ENV`. This would work great for developers who have a dotenv / direnv setup, however I think it is not the case for the average user
2) `uv venv project-name` should update a global `~/config/uv/sync` file with the name of the project and the location of the virtual environment. And then going forward, `uv sync` or `uv run` should use that information. This would be ideal because then tooling that builds on top of `uv` (e.g. marimo notebooks) will "do the right thing" automatically for every user using `uv.

There may be other ways to deal with this and I wanted to open a feature request to discuss. Feel free to close this issue if this is not in scope for this project.

### Example

_No response_

---

_Label `enhancement` added by @kdheepak on 2025-05-22 13:53_

---

_Comment by @konstin on 2025-05-22 17:11_

The uv project mode (the non-`uv pip` commands like `uv sync`) is built around uv managing the venv for the user, ideally the user shouldn't have to be concerned with `.venv` at all. For manually managing dependencies, there's `uv pip`, and for having dependencies without changing the project dependencies, there's `--with` and inline script metadata, both of which are isolated. With uv's venv creation, package remove and install performance, `uv run` can change the venv before running code without creating much overhead.

Managing multiple or switching between venvs on behalf of the user is currently out of scope for uv.

---

_Comment by @zanieb on 2025-05-22 18:33_

We've tried to avoid making uv operations "stateful" in this way as it adds a lot of complexity to the user experience.

I think you want #1495 

---

_Closed by @kdheepak on 2025-05-23 06:15_

---
