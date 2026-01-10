---
number: 14118
title: "Support `uv alias` to create a project-specific command aliases"
type: issue
state: closed
author: saroad2
labels:
  - enhancement
assignees: []
created_at: 2025-06-17T19:28:03Z
updated_at: 2025-06-18T14:30:31Z
url: https://github.com/astral-sh/uv/issues/14118
synced_at: 2026-01-10T01:25:42Z
---

# Support `uv alias` to create a project-specific command aliases

---

_Issue opened by @saroad2 on 2025-06-17 19:28_

### Summary

Inspired by [Git aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases), one should be able to define a project-specific command alias in *pyproject.toml* that UV can use.

### Example

Let's say that I have a project with a few tests that I want to run "pytest". Moreover, let's say that I want to be able to run just tests marked with "webtest".

Currently the way to do this with UV is:

```bash
# Run tests using pytest with UV
uv run pytest my_app.py

# Run all tests with marker "webtest" using pytest with UV
uv run pytest -m webtest my_app.py 
```

It could be nice if I could define in *pyproject.toml*:

```toml
[tool.uv.alias]
test = "run pytest"
webtest  = "run pytest -m webtest"
```

and then I could simply run: 
```bash
# Run tests using pytest with UV
uv test my_app.py

# Run all tests with marker "webtest" using pytest with UV
uv webtest my_app.py 
```

In the same way I could define aliases such as:

```toml
[tool.uv.alias]
upgrade = "sync --upgrade"
stree = "tree -d 2 --no-dev"
```

and then I could simply run:
and then I could simply run: 
```bash
uv upgrade  # Same as "uv sync --upgrade"

uv stree  # Same as "uv tree -d 2 --no-dev"
```

---

_Label `enhancement` added by @saroad2 on 2025-06-17 19:28_

---

_Comment by @nathanjmcdougall on 2025-06-17 19:52_

This seems to be similar in spirit to #5903 

---

_Comment by @zanieb on 2025-06-17 20:17_

Yes let's track this in the linked issue.

---

_Closed by @zanieb on 2025-06-17 20:17_

---

_Comment by @saroad2 on 2025-06-18 14:25_

Hi @nathanjmcdougall and @zanieb, thank you for the prompt response!

In my opinion, there is a big difference between this suggestion and the other suggestion you linked.

- `uv alias` is referring to aliasing **UV specific commands** such as `uv run`, `uv sync`, etc.
- `uv task` is referring to running **non-pythonic commands from within a UV environment**, things like `echo`, `cat`, etc.

While there is a similarity, they are not the same and I think it would be beneficial to support both.

-----

I think it will be clearer if I share my own use-case.

I have a python project that I'm managing with UV, let's call it "bambi". In this project, I need to run a server using `gunicorn`. Unfortunately, gunicorn only support running the server via a command-line and not via code, so in order to run my server within the uv-created environment, I need to run something like:

```bash
uv run --no-sync gunicorn -w 8 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000 --timeout 86400 'bambi.app:web_app'
```

I think it could be nice if I could just add this to the config:

```toml
[tool.uv.alias]
serve = "run --no-sync gunicorn -w 8 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000 --timeout 86400 'bambi.app:web_app'"
```

and then simply run:
```bash
uv serve
```

I'm not sure that #5903 really refers to this use-case.

What do you think?

---

_Comment by @zanieb on 2025-06-18 14:30_

I think we're unlikely to introduce both this _and_ #5903, as I think it'd be confusing to have multiple overlapping interfaces and #5903 would encompass the use-cases here.

---
