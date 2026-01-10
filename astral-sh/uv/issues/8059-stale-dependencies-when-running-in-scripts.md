---
number: 8059
title: Stale dependencies when running in scripts
type: issue
state: closed
author: aaronsteers
labels:
  - question
assignees: []
created_at: 2024-10-09T21:59:14Z
updated_at: 2025-02-16T19:13:56Z
url: https://github.com/astral-sh/uv/issues/8059
synced_at: 2026-01-10T01:24:23Z
---

# Stale dependencies when running in scripts

---

_Issue opened by @aaronsteers on 2024-10-09 21:59_

## Context

I love the `uv` tool and am trying to adopt it in more of my workflows.

I'm trying to use the "scripts" feature for utility scripts in my python projects, where those utility scripts might have very different python library dependencies, which otherwise might have version conflicts with my project's code.

## Problem

I seem to get stale dependencies sometimes in my virtualenv, and I'm not sure what is causing this. For instance, I got a stale version of `pip` in one case, which is problematic because my tool does some automated installation of other apps in their own virtual environments. (A bit meta, I know. 😄 )

In a more recent example, I was getting complaints of `Optional[int]` not being a valid type, meaning some part of the dependency tree was a different version than I was getting when installing with poetry or pip. I didn't trace this to any specific version conflict, but I confirmed I wasn't getting the error when I used other install methods, and 

## Ask

I'd like to better understand how the `uv` script feature creates virtual environments, to understand how this is working, and what might be causing the stale dependencies. 

Is there a way to locate the venv and/or to update transitive or stdlib dependencies within it? And/or print a list of versions (like pip freeze) within that virtualenv?

## Caveat

I've only experienced this when using the "script" feature (with dependencies defined inline within the file), but it might be a broader issue with uv-created virtual environments. (Although I've not seen it when installing with `uv pip install`.)


---

_Comment by @charliermarsh on 2025-02-16 19:13_

uv scripts now use "stable" environments that should be more reliable and easier to reason about. If you run `uv sync --script /path/to/script.py`, we'll tell you exactly where the environment is (and install the script's dependencies there). You can also run `uv sync --script /path/to/scripy.py --active` to use the currently-activated virtual environment for the script, rather than a path in the cache. I hope this helps :)

---

_Closed by @charliermarsh on 2025-02-16 19:13_

---

_Label `question` added by @charliermarsh on 2025-02-16 19:13_

---
