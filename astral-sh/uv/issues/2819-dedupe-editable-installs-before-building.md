---
number: 2819
title: Dedupe editable installs before building
type: issue
state: closed
author: jaap3
labels:
  - bug
assignees: []
created_at: 2024-04-04T14:47:38Z
updated_at: 2024-04-04T17:19:45Z
url: https://github.com/astral-sh/uv/issues/2819
synced_at: 2026-01-10T01:23:22Z
---

# Dedupe editable installs before building

---

_Issue opened by @jaap3 on 2024-04-04 14:47_

I'm trying `uv` in a monorepo setup that has a few apps and many shared (interdependent) packages. When installing an application all it's dependencies (and their sub-dependencies etc.) that live in this monorepo are installed in editable mode. This is done by chaining `requirements-dev.txt` files. i.e. given the following directory structure:

```
.
├── apps
│   └── frobulator
│       ├── pyproject.toml
│       └── requirements-dev.txt
└── packages
    ├── bar
    │   ├── pyproject.toml
    │   └── requirements-dev.txt
    ├── foo
    │   ├── pyproject.toml
    │   └── requirements-dev.txt
    ├── foobar
    │   ├── pyproject.toml
    │   └── requirements-dev.txt
    └── qa
        └── pyproject.toml

```

`frobulator/requirements-dev.txt` might look like:

```
# This project
-e ../../apps/frobulator

# Local packages
-r ../../packages/foo/requirements-dev.txt
-r ../../packages/foobar/requirements-dev.txt

# QA
-e ../../packages/qa
```

and `foobar/requirements-dev.txt` might look like:

```
# This project
-e ../../packages/foobar

# Local packages
-r ../../packages/foo/requirements-dev.txt
-r ../../packages/bar/requirements-dev.txt

# QA
-e ../../packages/qa
```

Now when I run `uv pip install -r requirements-dev.txt` in the `apps/frobulator` directory `uv` will show something like this:

```
   Built file:///code/apps/frobulator
   Built file:///code/packages/foo
   Built file:///code/packages/qa
   Built file:///code/packages/foobar
   Built file:///code/packages/foo
   Built file:///code/packages/qa
   Built file:///code/packages/bar
   Built file:///code/packages/qa
   Built file:///code/packages/qa
   Built file:///code/packages/qa       
Built 10 editables in 793ms
Resolved 6 packages in 7ms
Downloaded 1 package in 665ms
Installed 6 packages in 246ms
```

As you can see, some packages are built multiple times. In this example this is still reasonably fast, but in our real world repository this step takes a long time.

Am I right in concluding that these packages are really built multiple times in the same step? Is it possible to dedupe the list of editable installs *before* building?

I was also able to reproduce this by repeating the `-e` flag multiple times with the same paths, i.e.:

```
uv pip install -e . -e . -e ../../packages/qa/ -e ../../packages/qa/ -e ../../packages/foo -e ../../packages/bar ../../packages/bar ../../packages/foo ../../packages/foobar
   Built file:///code/apps/frobulator
   Built file:///code/apps/frobulator
   Built file:///code/packages/qa
   Built file:///code/packages/qa
   Built file:///code/packages/foo
   Built file:///code/packages/bar
Built 6 editables in 1.01s
Resolved 6 packages in 120ms
   Built foobar @ file:///code/packages/foobar
Downloaded 2 packages in 501ms
Installed 6 packages in 237ms
 + bar==0.1.0 (from file:///code/packages/bar)
 + foo==0.1.0 (from file:///code/packages/foo)
 + foobar==0.1.0 (from file:///code/packages/foobar)
 + frobulator==0.1.0 (from file:///code/apps/frobulator)
 + qa==0.1.0 (from file:///code/packages/qa)
 + ruff==0.3.5
```

Here it seems some repeated packages are not built multiple time, not sure why?

```
uv --version
uv 0.1.28
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-04 14:54_

---

_Label `bug` added by @charliermarsh on 2024-04-04 14:54_

---

_Comment by @charliermarsh on 2024-04-04 14:54_

Makes sense, thanks.

---

_Referenced in [astral-sh/uv#2820](../../astral-sh/uv/pulls/2820.md) on 2024-04-04 16:46_

---

_Closed by @charliermarsh on 2024-04-04 17:19_

---
