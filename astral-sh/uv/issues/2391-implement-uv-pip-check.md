---
number: 2391
title: "Implement `uv pip check`"
type: issue
state: closed
author: danielhollas
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-03-12T19:48:07Z
updated_at: 2024-03-15T17:39:56Z
url: https://github.com/astral-sh/uv/issues/2391
synced_at: 2026-01-10T01:23:16Z
---

# Implement `uv pip check`

---

_Issue opened by @danielhollas on 2024-03-12 19:48_

Heya. :wave:  Small feature request for implementing the functionality of `pip check`

```console
pip check -h

Usage:   
  pip check [options]

Description:
  Verify installed packages have compatible dependencies.

General Options:
  -h, --help                  Show help.
  --debug                     Let unhandled exceptions propagate outside the main subroutine, instead of logging them to stderr.
...
...
```

This is extremely useful for verifying that there are no broken dependencies in a given environment.
(In our specific use case, we are in the (very) unfortunate situation where we install into the same environment via separate pip invocation so this is crucial).

I believe `uv` has everything it needs to implement this, it should be essentially the same thing as the `--strict` option for `uv pip install`

```
      --strict
          Validate the virtual environment after completing the installation, to detect packages with missing dependencies or other issues
```

Example output when problems are found

```console
$ pip check
jupyterlab-server 2.25.4 has requirement jsonschema>=4.18.0, but you have jsonschema 3.2.0.
```

There's no output if all is fine, exit code 0.

---

_Label `enhancement` added by @charliermarsh on 2024-03-12 19:49_

---

_Label `cli` added by @charliermarsh on 2024-03-12 19:49_

---

_Comment by @charliermarsh on 2024-03-12 19:50_

Seems reasonable to me (and yes, looks like exposing `--strict` as its own command).

---

_Referenced in [astral-sh/uv#2397](../../astral-sh/uv/pulls/2397.md) on 2024-03-13 00:03_

---

_Closed by @zanieb on 2024-03-15 17:39_

---
