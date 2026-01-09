---
number: 11682
title: "Support \"only-binary\"configuration and UV_ONLY_PROJECT environment variable at project level"
type: issue
state: closed
author: jdputschadi
labels:
  - question
assignees: []
created_at: 2025-02-21T02:41:40Z
updated_at: 2025-04-01T20:33:23Z
url: https://github.com/astral-sh/uv/issues/11682
synced_at: 2026-01-07T13:12:18-06:00
---

# Support "only-binary"configuration and UV_ONLY_PROJECT environment variable at project level

---

_Issue opened by @jdputschadi on 2025-02-21 02:41_

### Summary

Please allow using "only-binary" via persistent configuration, similar to the way "no-binary" is supported at that level, allowing it to be used/applied with "uv sync" and "uv run".

"only-binary" has the helpful feature that it will fall back to an older wheel during resolution if needed instead of attempting to build a package.

Thanks,

Jeff.

### Example

_No response_

---

_Label `enhancement` added by @jdputschadi on 2025-02-21 02:41_

---

_Comment by @sanmai-NL on 2025-03-11 14:59_

Related to https://github.com/astral-sh/uv/issues/11963

---

_Comment by @sanmai-NL on 2025-03-11 15:04_

@lengau Am I correct to find that uv doesn't support configuring to not build dependencies from source, but allow to build the current `pyproject.toml` packages?

---

_Comment by @sanmai-NL on 2025-03-11 15:09_

Something like this doesn't work with a `pyproject.toml` that defines "mypackage".

```toml
[tool.uv]
no-binary-package = [
  "mypackage",
  "parse",
]
no-build = true
```

---

_Comment by @sanmai-NL on 2025-04-01 08:02_

@lengau Your implementation in #11963 seems to only allow setting no-build, not switching it:

```console
$ env UV_NO_BUILD=0 uv build
Building source distribution...
  × Failed to build `/Users/myuser/myproduct`
  ╰─▶ Building source distributions is disabled
```

I think it would be more useful if it's a real option (and documented clearly).

---

_Comment by @sanmai-NL on 2025-04-01 08:05_

Aside, I find this UI rather confusing that a subcommand that signals an intent then must be made to work by overriding the config using a subcommand specific option with the same name. This work-around to the issue I'm reporting here does work though.

```console
$ uv --build build
error: unexpected argument '--build' found

  tip: 'build --build' exists

Usage: uv [OPTIONS] <COMMAND>

For more information, try '--help'.
```

---

_Comment by @zanieb on 2025-04-01 20:28_

That error message is because `--build` / `--no-build` are available on most commands

https://github.com/astral-sh/uv/blob/ac2dcd658e7e4d526b069c879325f37afb77d1f2/crates/uv-cli/src/lib.rs#L4974-L5027

I don't quite follow the rest of your question @sanmai-NL ?

---

_Referenced in [astral-sh/uv#12607](../../astral-sh/uv/issues/12607.md) on 2025-04-01 20:30_

---

_Comment by @zanieb on 2025-04-01 20:32_

We won't support `UV_ONLY_PROJECT` in the `pyproject.toml` as it's a sync option, but `only-binary` is a thing as `no-build` and `no-build-package` https://docs.astral.sh/uv/reference/settings/#no-build 

---

_Label `enhancement` removed by @zanieb on 2025-04-01 20:33_

---

_Label `question` added by @zanieb on 2025-04-01 20:33_

---

_Closed by @zanieb on 2025-04-01 20:33_

---
