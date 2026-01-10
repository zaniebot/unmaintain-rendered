---
number: 16513
title: "Document whether `uv-build` backend respects lock file"
type: issue
state: closed
author: arthur-tacca
labels: []
assignees: []
created_at: 2025-10-30T15:15:10Z
updated_at: 2025-10-30T16:27:19Z
url: https://github.com/astral-sh/uv/issues/16513
synced_at: 2026-01-10T01:26:07Z
---

# Document whether `uv-build` backend respects lock file

---

_Issue opened by @arthur-tacca on 2025-10-30 15:15_

Looking at the [documentation for the new build backend](https://docs.astral.sh/uv/concepts/build-backend/) I have two questions:

1. Does it respect the package versions in the lock file?
2. Does it make use of the existing `.venv` package environment? (Part (a): does it use it if it already exists; (b) does it create it if not)

These are overlapping but not exactly the same. For example, it could be 1. yes, 2. no if it creates an isolated environment but using the lock file. (It could also be 1. no, 2. yes but that seems very unlikely.)

In principle, these questions apply separately to whether the frontend is `uv` or something else (e.g. `python -m build`) but hopefully the answers are the same either way.

I haven't marked this as "question" because I'm only 10% posting this to get the actual answer. My main point here is that this should be explicit in the documentation. Apologies if it is and I missed it. If it's not present because it's so totally obvious if you really understood Python build frontend/backend separation then I'd suggest it's still worth a mention for those like me who don't find it obvious.

---

_Comment by @arthur-tacca on 2025-10-30 15:15_

I don't have permission to add labels but I intend the labels to be [build-backend](https://github.com/astral-sh/uv/issues?q=state%3Aopen%20label%3Abuild-backend) and [documentation](https://github.com/astral-sh/uv/issues?q=state%3Aopen%20label%3Adocumentation)

---

_Comment by @zanieb on 2025-10-30 15:22_

1. No, it does not.
2. No, it creates an isolated virtual environment per PEP 517.

I'm not quite sure what the question means though. Like, what packages do you expect the build backend to be using that would be installed in a Python environment and pinned?

---

_Comment by @arthur-tacca on 2025-10-30 15:23_

I've just realised that this is a probably very silly point: if I've understood right, the uv build backend never runs any Python code, so it never uses *any* virtual environment and it doesn't even make sense to talk about a lock file for it. I'm coming from a Hatch/Hatchling background where these things do make sense because you might perform some build-time action in Python. Is that right? If so it might still be worth highlighting - but I'm not so sure.

---

_Comment by @zanieb on 2025-10-30 16:07_

If you use `python -m build` that build frontend will create a virtual environment, then install `uv-build` from PyPI in it, then run `uv-build`. This is the standard way to do a build.

If you use `uv build`, we'll do that if the `uv-build` requirement is incompatible with the current uv version, but often we can skip all that work and just run the Rust code directly.

In either case, the rest of the dependencies in your `uv.lock` should not be relevant though.

---

_Comment by @arthur-tacca on 2025-10-30 16:27_

OK, I see. In summary, my original question was about how build-time dependencies are installed, and the answer is "there aren't any" (except `uv-build` sometimes, but that's not what I meant anyway). It may be that a comment could be worth it in the documentation, but since my main point was moot I'll just close this. Thanks @zanieb 

---

_Closed by @arthur-tacca on 2025-10-30 16:27_

---
