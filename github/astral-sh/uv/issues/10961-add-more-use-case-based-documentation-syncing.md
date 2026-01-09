---
number: 10961
title: Add more use-case based documentation - syncing projects across machines
type: issue
state: open
author: bjornasm
labels: []
assignees: []
created_at: 2024-10-30T11:55:14Z
updated_at: 2025-01-25T16:47:11Z
url: https://github.com/astral-sh/uv/issues/10961
synced_at: 2026-01-07T13:12:18-06:00
---

# Add more use-case based documentation - syncing projects across machines

---

_Issue opened by @bjornasm on 2024-10-30 11:55_

First of all, thank you for a great tool!

I have not used anything else than .venv and pip before, so I may have different needs from the documentation than most. With that in mind, could we get more use-case based documentation? 

My specific use case was that I had a project in a repo that I wanted to set up on a different computer. Reading the documentation, about lockfiles: `This file should be checked into version control, allowing for consistent and reproducible installations across machines.`, I figured that the uv.lock file should be checked into version control, and used to reproduce the environment on the differenct computer. However `git clone (..) & uv init & uv sync` overwrites the `uv.lock` file. I think that means that both `pyproj.toml` and `uv.lock` should be checked into version control? 

Just a small proposal:


> Syncing projects across machines
> 
> To sync projects between machines using version control, check in both the uv.lock file and pyproj.toml into version control. On the new machine, clone your repository. Run uv init (?) / run uv sync (...)
> 

Related to this is the use case of adding uv on already existing projects (uv init will generate a folder structure and hello.py and thus doesn't like the right way of doing this.)

I think for us that are new to tooling systems like this, it will help a lot to follow specific use-case as all the different concepts can be a bit overwhelming when trying to piece it together.

---

_Renamed from "Add more use-case based documentation" to "Add more use-case based documentation - syncing projects across machines" by @bjornasm on 2024-10-30 11:56_

---

_Comment by @zanieb on 2025-01-25 16:47_

Missed this because it's actually a `uv` issue — transferring there.

---
