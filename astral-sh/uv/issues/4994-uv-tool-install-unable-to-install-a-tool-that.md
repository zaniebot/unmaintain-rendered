---
number: 4994
title: "`uv tool install` unable to install a tool that depends on intermediate dependencies with entrypoints"
type: issue
state: closed
author: j178
labels:
  - question
assignees: []
created_at: 2024-07-11T15:38:04Z
updated_at: 2025-05-30T04:21:21Z
url: https://github.com/astral-sh/uv/issues/4994
synced_at: 2026-01-10T01:23:44Z
---

# `uv tool install` unable to install a tool that depends on intermediate dependencies with entrypoints

---

_Issue opened by @j178 on 2024-07-11 15:38_

For instance, `fastapi` doesn't have any entrypoints but it depends on `fastapi-cli`, which provides an entrypoint named `fastapi`. 
`uv tool install fastapi` would result in a failure, displaying the error message `No executables found for fastapi`.

```
$ uv tool install fastapi
warning: `uv tool run` is experimental and may change without warning.
Resolved 36 packages in 729ms
Installed 36 packages in 92ms
...
 + fastapi==0.111.0
 + fastapi-cli==0.0.4
...
error: No executables found for `fastapi`
```

But at the same time `uv tool run fastapi` works:
```
$ uv tool run fastapi --version
warning: `uv tool run` is experimental and may change without warning.
Resolved 36 packages in 18ms
FastAPI CLI version: 0.0.4
```

## Environment
```
os: Windows 11 Pro 23H2 (Build 22631.3737)
uv version: 0.2.24
```

---

_Comment by @zanieb on 2024-07-11 15:40_

Interesting. I don't think we should install executables from dependencies by default, but we should allow opt-in.

---

_Assigned to @zanieb by @zanieb on 2024-07-11 15:41_

---

_Comment by @j178 on 2024-07-11 15:56_

`rye tools install` has a flag `--include-dep <INCLUDE_DEP>` to include scripts from a given dependency, maybe `uv` can allow something likes this.

---

_Unassigned @zanieb by @zanieb on 2024-07-11 16:17_

---

_Comment by @zanieb on 2024-07-11 16:23_

I actually won't take this on right away, I've got to take care of some other things first if someone is interested. I think we need something like:

- `--executable <name>`: Only install the given executable name
- `--include-package <package name>`: Include executables from the given dependency package

I'm a little unsure of the name for the second one. Maybe:

- `--include-dep`
- `--include-executables`
- `--executables-from`

For `--executable`, we need to decide if we'll scan dependencies if it's not found (we probably shouldn't for safety and performance but we _could_). If not, it's kind of separate from this issue.

We'll need to add included packages to the receipt, which is where most of the complexity is here.

---

_Comment by @charliermarsh on 2024-07-11 22:48_

Why would this not be uv tool install fastapi-cli?

---

_Comment by @charliermarsh on 2024-07-12 00:51_

And if `fastapi-cli` doesn't depend on `fastapi`, couldn't you do: `uv tool install fastapi-cli --with fastapi`?

---

_Label `question` added by @charliermarsh on 2024-07-12 00:51_

---

_Comment by @charliermarsh on 2024-07-12 00:59_

Although it seems like `uvx fastapi --from fastapi-cli` just works?

---

_Referenced in [astral-sh/uv#4997](../../astral-sh/uv/pulls/4997.md) on 2024-07-12 03:31_

---

_Comment by @zanieb on 2024-07-12 16:20_

Yeah `uvx --from fastapi-cli fastapi ...` does seem like the proper invocation here — but we would need to support including dependency entry points for parity with Rye and pipx.

---

_Comment by @charliermarsh on 2024-07-12 16:23_

Yeah it just seems like it's kind of... the wrong way around, to include a dependency entrypoint. And it complicates the CLI a lot.

---

_Referenced in [astral-sh/uv#5017](../../astral-sh/uv/issues/5017.md) on 2024-07-12 16:25_

---

_Comment by @zanieb on 2024-07-12 16:32_

I agree it's a bit awkward. I think we should pursue #5017 and #5018 to help guide users to the correct path here and we can consider implementing this more complicated API based on additional feedback.


---

_Comment by @charliermarsh on 2024-07-21 23:31_

Ok, `uv tool run jupyter` is a counterexample here. Right now, we suggest `uv tool run --from jupyter-core jupyter`. And it's true that `jupyter-core` contains the Jupyer executable... but you _also_ need `jupyer` installed in order to do, like, `jupyter notebook`. Otherwise, you get: `Jupyter command `jupyter-notebook` not found.`

So the command in that case would really need to be... `uv tool run --from jupyter-core --with jupyter jupyter notebook`.


---

_Referenced in [astral-sh/uv#6329](../../astral-sh/uv/issues/6329.md) on 2024-08-21 14:16_

---

_Closed by @zanieb on 2024-10-21 21:23_

---
