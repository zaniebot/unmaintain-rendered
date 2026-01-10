---
number: 12636
title: "How do I do the equivalent of `python -u` with `uv run`?"
type: issue
state: closed
author: nbelakovski
labels:
  - question
assignees: []
created_at: 2025-04-02T19:07:02Z
updated_at: 2025-04-02T20:46:17Z
url: https://github.com/astral-sh/uv/issues/12636
synced_at: 2026-01-10T01:25:22Z
---

# How do I do the equivalent of `python -u` with `uv run`?

---

_Issue opened by @nbelakovski on 2025-04-02 19:07_

### Question

`python -u` will "Force the stdout and stderr streams to be unbuffered.", this has the effect of calling all print statements with `flush=True`

I need this for a script where I'm using subprocess and some of those commands are not buffering their output. I've looked at the documentation, but it doesn't even contain the word "buffer", nor any reference to PYTHONUNBUFFERED.

I'm sure this is not the only option that the python interpreter has that uv might want to replicate, so maybe this is a can of worms but probably worth starting the discussion on it, if there's no existing solution?

A partial solution might be `uv run python -c`, but this won't work for scripts that use the script metadata for their dependencies.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @nbelakovski on 2025-04-02 19:07_

---

_Comment by @zanieb on 2025-04-02 19:22_

We don't support arbitrary Python flags when running scripts right now. Related to https://github.com/astral-sh/uv/issues/6542

---

_Comment by @zanieb on 2025-04-02 19:22_

You can set the `PYTHONUNBUFFERED` variable though, right?

---

_Comment by @nbelakovski on 2025-04-02 19:42_

Oh, yea I guess I could do that! Why didn't I think of that? 🙃 

---

_Comment by @zanieb on 2025-04-02 20:46_

That's the general pattern we recommend for now. Lmk if you have more questions!

---

_Closed by @zanieb on 2025-04-02 20:46_

---
