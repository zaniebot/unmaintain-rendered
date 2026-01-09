---
number: 16892
title: Script overwriting behavior not aligned with pip
type: issue
state: open
author: ywang96
labels:
  - question
  - compatibility
  - needs-decision
assignees: []
created_at: 2025-11-29T07:18:34Z
updated_at: 2025-12-11T09:54:31Z
url: https://github.com/astral-sh/uv/issues/16892
synced_at: 2026-01-07T13:12:19-06:00
---

# Script overwriting behavior not aligned with pip

---

_Issue opened by @ywang96 on 2025-11-29 07:18_

### Question

Hi there! Thanks for the great project and we have been adopting `uv` for various projects under `vllm-project` and prompting it as our main environment setup framework!

We recently run into an issue where one behavior is not aligned between `uv` and `pip`. 

Suppose I have a project `abc-new` that depends on an existing library `abc`, and in the `pyproject.toml` of `abc-new` I have something like this
```
...
dependencies = [
    ...
    "abc==x.xx.x",
]
[project.scripts]
dummy = "abc-new.entrypoints.cli.main:main"
```

Here `dummy` is actually already a command of the library `abc`, which means we're overwriting its behavior to point to `abc-new.entrypoints.cli.main:main`. 

I have verified this works with `pip install` as expected, but unfortunately it does not with `uv` (the `dummy` command isn't getting overwritten) and the current workaround is to first install `abc==x.xx.x` itself, then install `abc-new`.

I've tried to search in some github issues and couldn't really find any solution, so I'm wondering if there'll be a better way to resolve this issue other than install the two packages separately, especially when users won't need to do so when using `pip`.

Thanks!

### Platform

Linux 6.14.0-29-generic x86_64 GNU/Linux

### Version

uv 0.9.2

---

_Label `question` added by @ywang96 on 2025-11-29 07:18_

---

_Comment by @ywang96 on 2025-12-03 10:18_

Hello! Gentle ping regarding this question/issue 😄 

To provide more context, the project in question here is `vllm-omni` and as you can see we [have to comment out the dependency on vLLM](https://github.com/vllm-project/vllm-omni/blob/598d26580e0f5c05bcd1e8ab49a0dfdf902d500e/pyproject.toml#L41) because of this issue, but it would be great if we can include it so that users won't need to install two libraries. Thanks!

---

_Comment by @zanieb on 2025-12-03 12:54_

I think you're in unstandardized behavior here. The install order is arbitrary and tool specific, as far as I know.

I'm not sure we should do a topological install order. I presume that's what you'd need to resolve this. I'd generally recommend _not_ using a pattern which shadows artifacts of another package.

cc @konstin 

---

_Label `compatibility` added by @zanieb on 2025-12-03 12:54_

---

_Label `needs-decision` added by @zanieb on 2025-12-03 12:54_

---

_Comment by @notatallshaw on 2025-12-03 15:37_

FWIW pip does intentionally do a best attempt at topologically ordering the installs: https://github.com/pypa/pip/blob/25.3/src/pip/_internal/resolution/resolvelib/resolver.py#L218

I say best because it gives up after 5 cycles to avoid exponential blow up in highly cyclical dependencies. 

This is all of course tool specific behavior, it's a matter of what the tool thinks is best for users. 

---

_Comment by @ywang96 on 2025-12-05 19:12_

Thank you both for the explanation and pointers here!

@zanieb Totally understood if this is not a standardized behavior - would you say the current workaround (i.e, ask users to first install `vllm`, then `vllm-omni`) will be the best way to do it in the foreseeable future?

We really want users to be able to use a single command line entrypoint (e.g, `vllm ...`) for the two projects while being able manager them separately, (and, of course, still use `uv` as their environment manager), so if there's a better way to do it please let me know! Thanks!

---

_Comment by @konstin on 2025-12-11 09:54_

It doesn't seem correct that one dependency can implictly overwrite the entrypoints of another dependency. Since both are vllm projects, can you do the dispatching inside the entrypoint?

---
