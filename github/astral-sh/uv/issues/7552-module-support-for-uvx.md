---
number: 7552
title: Module support for uvx
type: issue
state: open
author: nmichlo
labels:
  - needs-design
  - uv tool
assignees: []
created_at: 2024-09-19T14:25:11Z
updated_at: 2025-05-07T15:31:05Z
url: https://github.com/astral-sh/uv/issues/7552
synced_at: 2026-01-07T13:12:17-06:00
---

# Module support for uvx

---

_Issue opened by @nmichlo on 2024-09-19 14:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently `uvx` or `uv tool` only seems to support packages that provide binaries. However, many packages haven't properly published executables within them and you often need to call `python -m package` on these to run them.

- A notable example is `setuptools_scm` or `grpc_tools.protoc`

It would be great if we could do something like:

- `uvx setuptools-scm -m setuptools_scm`

   instead of:
   `uv pip install setuptools-scm && python -m setuptools_scm`

- `uvx grpcio-tools -m grpc_tools.protoc -I. --python_out=. example.proto`

   instead of:
   `uv pip install grpcio-tools && python -m grpc_tools.protoc -I. --python_out=. example.proto`

Both of the above then pollute the env.

--------

Context:

I've looked at `uv run` but this doesn't seem to be applicable because this is not for installed modules, rather it is for local scripts, and it also interacts with the current pyproject.toml (it seems to want to install things even if they are listed as extras, which may also be a bug?)

I specifically don't want to install the package I am running in the current env, e.g. I need to build a project using some deps, like running grpc compilation. But I don't want the extra grpc installed deps to conflict with later builds steps, e.g. PyInstaller.


---

_Comment by @bluss on 2024-09-19 15:12_

`uvx --from setuptools-scm python -m setuptools_scm` is available today, but that is just a sidenote.

---

_Comment by @nmichlo on 2024-09-19 21:41_

@bluss 

Thank you for this

---

_Comment by @zanieb on 2024-09-19 22:29_

It seems sort of awkward to support this, I think it'd have to be `uvx -m setuptools_scm setuptools` so the arguments aren't ambiguous? I'm not sure if that offers much over the more verbose version.

---

_Label `needs-design` added by @zanieb on 2024-09-19 22:29_

---

_Label `uv tool` added by @zanieb on 2024-09-19 22:29_

---

_Comment by @nmichlo on 2024-09-19 23:18_

@zanieb I think you are right. Some shorter syntax would be great, but the syntax I proposed could conflict with arguments for the tools themselves or seem confusing in relation to that. Order would have to be respected. So this maybe isn't as intuitive.

Maybe the immediate win is just an addition to the docs with an example of @bluss's answer?
https://docs.astral.sh/uv/guides/tools/#installing-tools

-----------

EDIT:

current
```bash
# current tools without uv
<tool> <args>
# current tools with uv
uvx <tool> <args>

# current modules without uv
python -m <module> <args>
# LONGHAND: current modules with uv
uvx --from <tool> python -m  <module> <args>
```

The pattern should be that without uv we should have `<cmd> <args>`, and with uv we should have `uvx <uvx-args> <cmd> <args>`

Thus, the prior suggestions result in ambiguous/conflicting shorthand:
```bash
# PROPOSED SHORTHAND 1: conflicts with `uvx <tool> <args>`, args ambiguous
uvx <tool> -m <module> <args>

# PROPOSED SHORTHAND 2: conflicts with `... -m <module> <args>` syntax, as inserts `<tool>` inbetween
uvx -m <module> <tool> <args>
```

A possibly not conflicting solution, with a definite marker/branch in command behaviour, is:
```bash
# PROPOSED SHORTHAND 3, behaviour change with flags, expanding to longhand & not ambiguous
uvx -m <tool/module> <args>    # if the same name for both of them (assuming normalisation on the module name), expands to longhand
uvx --from <tool> -m <module> <args>  # if differing names, expands to longhand
uvx --from <tool> python -m <module> <args>  # current longhand

# example 1:
uvx -m setuptools-scm --help
# * expands to / is the same as (note normalisation):
uvx --from setuptools-scm -m setuptools_scm --help
# * expands to / is the same as (although now shorthand isn't really worthwhile anymore as its just one extra word):
uvx --from setuptools-scm python -m setuptools_scm --help
```

-----------

EDIT 2:

The shorthand, is nice for quick common scenarios, but the shorthand only really works if package and module names when normalised are the same. For most projects this is the case, so still useful then. Although when not the same then it's only 1 arg less than the longhand.


---

_Comment by @bluss on 2024-09-20 11:17_

Same issue as with uv run for the module argument, everything that follows -m is an external command, should not be parsed as arguments to uvx.

---

_Comment by @theonewolf on 2025-05-07 15:31_

This would be a useful feature.

---

_Referenced in [astral-sh/uv#15455](../../astral-sh/uv/issues/15455.md) on 2025-08-22 16:15_

---
