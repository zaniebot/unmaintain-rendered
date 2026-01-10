```yaml
number: 3199
title: Include a method to create machine readable output
type: issue
state: closed
author: pfmoore
labels:
  - cli
assignees: []
created_at: 2024-04-22T21:09:18Z
updated_at: 2025-06-13T12:05:16Z
url: https://github.com/astral-sh/uv/issues/3199
synced_at: 2026-01-10T03:32:44Z
```

# Include a method to create machine readable output

---

_Issue opened by @pfmoore on 2024-04-22 21:09_

When automating `uv`, having to parse the command output (which is intended for human readability) is not ideal. It would be useful to have an option to produce machine readable (probably JSON) output from the various `uv` commands.

* For `uv pip install` something along the lines of the [pip installation report](https://pip.pypa.io/en/stable/reference/installation-report/) would probably be a reasonable format.
* For `uv pip list`, the `pip list --format=json` command is probably a good model.
* `uv pip show` and `uv pip freeze` are machine readable, although having a `--format=json` equivalent for consistency might be worthwhile.
* `uv pip check` probably doesn't need an output option, assuming the exit code is success or failure as appropriate.
* `uv pip compile` and `uv pip sync` can probably have an installation report similar to `uv pip install`. I've not used these myself, though, so I don't know how useful they would be in practice.
* `uv venv` could produce JSON output showing details of the created environment - path to the environment, Python executable path, scripts and site-package directories. Basically, the values that the stdlib `venv` module captures in the "context" object documented [here](https://docs.python.org/3.12/library/venv.html#venv.EnvBuilder.ensure_directories).

To make automation easier, it's probably worth having some additional command line capabilities:

* An isolated mode, to prevent user config affecting results (`uv` doesn't have as many possibilities for problems here as pip does, but that may change, I guess...)
* A "guaranteed quiet" mode that ensures that *just* the JSON data will be printed to stdout, with any user messages either going to stderr or being suppressed. (Writing the data to a file is another possibility, but IMO it tends to be clumsy to manage).
* I assume `uv` always writes its output as UTF-8, but if it doesn't (for example it follows the system defined encoding) then a "guaranteed UTF-8 output" mode would be important to avoid encoding problems.

---

_Label `cli` added by @zanieb on 2024-04-22 22:06_

---

_Comment by @ChannyClaus on 2024-04-24 03:30_

should we note in the description that `uv pip list --format json` already exists? https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/pip_list.rs#L115

```
$ python -m uv pip list --format json | jq .
[
  {
    "name": "certifi",
    "version": "2024.2.2"
  },
  {
    "name": "charset-normalizer",
    "version": "3.3.2"
  },
  {
    "name": "idna",
    "version": "3.6"
  },
  {
    "name": "requests",
    "version": "2.31.0"
  },
  {
    "name": "urllib3",
    "version": "2.2.1"
  }
]
```

---

_Comment by @pfmoore on 2024-04-24 08:08_

Thanks, I’d missed that.

---

_Comment by @sfc-gh-jcarroll on 2024-04-25 12:37_

We also need it for equivalent of `--dry-run` without doing the install.

Related:
* https://github.com/astral-sh/uv/issues/411
* https://github.com/astral-sh/uv/issues/1442

---

_Comment by @zanieb on 2025-06-13 12:05_

Let's track this in https://github.com/astral-sh/uv/issues/411 — it's confusing to have two issues.

---

_Closed by @zanieb on 2025-06-13 12:05_

---
