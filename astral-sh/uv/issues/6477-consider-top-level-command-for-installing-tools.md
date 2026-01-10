---
number: 6477
title: Consider top level command for installing tools
type: issue
state: closed
author: daviewales
labels:
  - cli
  - uv tool
assignees: []
created_at: 2024-08-23T00:11:20Z
updated_at: 2024-08-23T12:53:36Z
url: https://github.com/astral-sh/uv/issues/6477
synced_at: 2026-01-10T01:24:01Z
---

# Consider top level command for installing tools

---

_Issue opened by @daviewales on 2024-08-23 00:11_

Currently with `pipx`, I can run:

    pipx install black

And I get `black` on my path.

I can also do:

    pipx run black

And it runs `black`, but doesn't install it to my path.
`uv tool install` appears to do the same thing as `pipx install`.
And `uvx black` appears to do the same as `pipx run black`.
But I wonder if it would make more sense to mirror the `pipx` command structure more closely?

Perhaps:

    uvx --install black

could be equivalent to

    uv tool install black

This allows you to keep the current functionality of `uvx black` to run `black without installing.

There are also currently no Python packages called `install`, so you could get away with:

    uvx install black

But this is a bit confusing, so maybe don't do that unless you completely adopt the `pipx` semantics.

Alternatively, if you really want to keep the simplicity of `uvx black`, perhaps you could add a similar shortcut for installing?
Something like:

    uvi black

Or

    uvxi black

---

_Comment by @zanieb on 2024-08-23 00:15_

Thanks for the detailed description!

I don't see a particular value in mirroring `pipx` closely, we already have `uv tool` which roughly mirrors this with `uv tool run` and `uv tool install`. What's the benefit in a shorter installation invocation? If it's critical in your workflow maybe `alias uvi="uv tool install"` makes sense?

Note that we match other tools here, for example, `npx` also optimizes for running a command quickly.

---

_Label `cli` added by @zanieb on 2024-08-23 00:15_

---

_Label `uv tool` added by @zanieb on 2024-08-23 00:15_

---

_Comment by @daviewales on 2024-08-23 00:57_

I guess my thought is that for a tool in the Python ecosystem, it makes sense to match existing Python tools.

For example:

```
pip install
pip uninstall
pipx install
pipx uninstall
```

`uv` is the odd one out here.

`pipx` is a gentle adaptation of my existing mental model. I know how to `pip install` things. Now I can `pipx` install things, and it's exactly like `pip install`, but with isolated environments, etc.

`uv` breaks that mental model by copying a package manager from a different ecosystem.

I understand that you are trying to do a lot with a single tool, so I understand why you need an extra command level in the base `uv` tool.

But `uvx` is intended to be a shortcut for this tool.

You ask "What's the benefit in a shorter installation invocation?"

For me, yes it's nice to type a few less characters.
But more importantly, it matches the existing mental model of everyone who's ever used `pip`.
You just replace `pip` with `pipx`, `uvx`, etc, and carry on with faster installs, dependency isolation, and so on.

---

_Comment by @zanieb on 2024-08-23 12:53_

> uv breaks that mental model by copying a package manager from a different ecosystem.

To be clear, we didn't copy this other package manager. We assessed interfaces like this in every ecosystem and then decided on an interface that makes the most sense for us.

We have to worry about people being confused about if they should use `uvx install`, `uv tool install`, `uv pip install`, and `uv add` — it doesn't sound worth it.

Sorry, but we have to focus on correctness things right now and even if we were amenable to this it'd be low priority.

---

_Closed by @zanieb on 2024-08-23 12:53_

---
