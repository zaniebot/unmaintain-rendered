```yaml
number: 989
title: Respect parent Python interpreter that invoked ty as a candidate Python env
type: issue
state: closed
author: carljm
labels:
  - cli
  - configuration
  - uv
assignees: []
created_at: 2025-08-14T16:28:07Z
updated_at: 2025-11-06T14:27:51Z
url: https://github.com/astral-sh/ty/issues/989
synced_at: 2026-01-10T02:06:24Z
```

# Respect parent Python interpreter that invoked ty as a candidate Python env

---

_Issue opened by @carljm on 2025-08-14 16:28_

This would make e.g. `uvx --with Foo ty check script_using_Foo_library.py` work as a one-liner. Today it doesn't work. uv creates an ephemeral env with Foo installed, and then invokes ty's Python shim from that env, but ty doesn't care about that. uv doesn't set `VIRTUAL_ENV`, and the ephemeral env isn't located anywhere ty's searching will find it, so ty never finds the env it was invoked from.

I suspect this would also make things "just work" for a significant number of end-user cases, where they are running ty from the same Python installation their dependencies are installed in, without `VIRTUAL_ENV` set. (Either because they have a venv and are running its binary directly without shell activation, or because they aren't using a venv at all.)

Uv has specific logic to do this (for itself), see https://github.com/astral-sh/uv/blob/a186fda2d27d74c631c50ffccb9413d31f95cc89/python/uv/__main__.py#L35 and https://github.com/astral-sh/uv/blob/2c54d3929c11fd9b5c8b0235ab201fe9d5501751/crates/uv-python/src/discovery.rs#L487-L493 -- we could borrow this approach.

We would still need to decide where in the priority this would fall. I think it would probably be last in fallback order, after explicit config and `.venv` and `VIRTUAL_ENV`.

---

_Label `cli` added by @carljm on 2025-08-14 16:28_

---

_Label `configuration` added by @carljm on 2025-08-14 16:28_

---

_Comment by @AlexWaygood on 2025-08-14 16:31_

this used to work with previous versions of uv after a PR I made to uv, but I think they changed the way they layer ephemeral environments over the top in a recent version of uv which means that it no longer works.

---

_Comment by @MichaReiser on 2025-08-14 16:32_

CC: @zanieb 

---

_Comment by @AlexWaygood on 2025-08-14 16:33_

for previous discussion, see https://github.com/astral-sh/ty/issues/320, https://github.com/astral-sh/uv/pull/13598 and https://github.com/astral-sh/ruff/pull/18335

---

_Comment by @zanieb on 2025-08-14 16:48_

That's for `uv run` not `uvx`, are you sure the behavior regressed here?

---

_Comment by @AlexWaygood on 2025-08-14 19:14_

> That's for `uv run` not `uvx`, are you sure the behavior regressed here?

Ah sorry, I'm getting mixed up between my uv executables -- yeah, I don't think `uvx` has ever worked to date; would be cool if it did!

I thought something with the `uv run` invocation broke recently but now I can't seem to find a minimal repro... I'll post a separate issue if I come across it again.

Sorry for the noise!! :-)

---

_Comment by @sharkdp on 2025-09-08 08:35_

I think we also need this to allow users to [check their PEP 723 scripts](https://github.com/astral-sh/ty/issues/691#issuecomment-2995041731) using [uv's new `--with-requirements` option](https://github.com/astral-sh/uv/pull/12763) using
```bash
uvx --with-requirements script.py ty check script.py
```

This already works with pyright, mypy, pyrefly, but not yet with ty.

---

_Added to milestone `GA` by @carljm on 2025-09-08 19:11_

---

_Comment by @zanieb on 2025-09-18 13:19_

This was discussed internally in [Discord](https://discord.com/channels/1039017663004942429/1343690517745111081/1417965051549188277).

The reason pyright and mypy work is that they add the virtual environment they are _installed_ in to the search path. ty does not do this, but should. There's some debate about which circumstances are appropriate for ty to do this, e.g.:

1. If an environment cannot be found (i.e., neither `.venv` and `VIRTUAL_ENV`)
2. If an environment is not active (i.e., only if `VIRTUAL_ENV` is unset)
3. If ty is invoked with `python -m ty` (this matches uv's behavior for respecting its own environment)
4. Always (with some opt-out)

---

_Assigned to @zanieb by @carljm on 2025-09-19 15:04_

---

_Removed from milestone `GA` by @carljm on 2025-09-19 15:04_

---

_Added to milestone `Beta` by @carljm on 2025-09-19 15:04_

---

_Comment by @MichaReiser on 2025-10-24 14:29_

@zanieb is this something you're still planning to work on (asking because you're assigned to the issue)?

---

_Label `uv` added by @MichaReiser on 2025-11-05 12:34_

---

_Closed by @Gankra on 2025-11-06 14:27_

---
