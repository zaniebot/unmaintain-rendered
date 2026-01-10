---
number: 9614
title: Allow single constraints and overrides to be passed via the CLI
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-12-03T18:08:29Z
updated_at: 2024-12-07T22:21:44Z
url: https://github.com/astral-sh/uv/issues/9614
synced_at: 2026-01-10T01:24:43Z
---

# Allow single constraints and overrides to be passed via the CLI

---

_Issue opened by @zanieb on 2024-12-03 18:08_

The `--constraint` and `--override` flags should be updated to allow single constraints (in addition to the current behavior of accepting a file path), e.g., `--constraint "foo>=0.1"`. In the `--constraint` case, this is mostly unambiguous — if there's a version specifier then we can treat it as a constraint. It is possible to have a file with that name, but it seems very unlikely in practice. In the `--override` case, we do allow overriding a dependency to have _no_ specifiers, e.g., `--override foo`. I'm not sure if we should allow this via this interface? It seems nice to avoid the ambiguity. Historically, we've avoided inspecting the file system to determine the behavior of arguments and I think that is a good goal to maintain.

Previous discussion at

- https://github.com/astral-sh/uv/pull/4973#issuecomment-2233540897
- https://github.com/astral-sh/uv/issues/4824#issuecomment-2211971741
- https://github.com/astral-sh/uv/pull/9547#discussion_r1866759370

---

_Label `cli` added by @zanieb on 2024-12-03 18:08_

---

_Comment by @zanieb on 2024-12-03 18:09_

This proposal applies to both the `uv pip` interface (though it would differ from the upstream) and top-level interface (where `--constraint` and `--override` is only implemented for `uv tool install` at the moment but should be added elsewhere)

---

_Comment by @notatallshaw-gts on 2024-12-03 18:16_

> It is possible to have a file with that name, but it seems very unlikely in practice

You could always check for a file first and then fall back to see if it's a valid version specifier (that includes an operator, e.g. `foo>=0.1` not just `foo`).

---

_Comment by @zanieb on 2024-12-03 18:37_

> You could always check for a file first and then fall back to see if it's a valid version specifier

This is the behavior we have historically avoided. We don't want the CLI behavior to change based on the state of the file system.

---

_Comment by @notatallshaw-gts on 2024-12-03 18:56_

> > You could always check for a file first and then fall back to see if it's a valid version specifier
> 
> This is the behavior we have historically avoided. We don't want the CLI behavior to change based on the state of the file system.

Okay but `constraints.txt` is a valid requirement 😉.

```python
>>> from packaging.requirements import Requirement
>>> Requirement('constraints.txt')
<Requirement('constraints.txt')>
```

And that is likely to be the name of a file, so I guess document this carefully on what the boundary conditions are between choosing a file or a requirement.

---

_Comment by @notatallshaw-gts on 2024-12-03 19:07_

> we do allow overriding a dependency to have _no_ specifiers, e.g., `--override foo`

One last thing, the [one use case](https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800) to be aware of here is when specifying an always falsey environment marker, e.g. `foo ; sys_platform == 'never'`.

---

_Comment by @zanieb on 2024-12-03 19:16_

> Okay but constraints.txt is a valid requirement 😉.

Yeah so that's why I said we'd require a version specifier. A constraint without a version specifier doesn't do anything.

> One last thing, the https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800 to be aware of here is when specifying an always falsey environment marker, e.g. foo ; sys_platform == 'never'.

Good point!

---

_Comment by @charliermarsh on 2024-12-04 00:54_

Yeah, I guess we'd say: if it includes `;`, `=`, `<`, or `>` (and _not_ `/` or `\`) we treat it as a specifier.

---

_Comment by @notatallshaw-gts on 2024-12-04 15:17_

> :

Do you mean `;`?

---

_Comment by @charliermarsh on 2024-12-04 15:18_

Yes, sorry. (Edited.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-07 20:28_

---

_Comment by @charliermarsh on 2024-12-07 22:01_

@zanieb -- Just noticed one problem here... The values are technically space-delimited, so you can do `UV_CONSTRAINTS="file1 file2"` etc. That was initially for pip compatibility. But if they're space-delimited, it becomes much harder to do this... Not impossible, but definitely harder.

---

_Comment by @zanieb on 2024-12-07 22:21_

Alas. I'm not sure what to do about that.

As a minor note, it's `UV_CONSTRAINT`.

One alternative option: we could align on the name in the `pyproject.toml`, which would be `--dependency-constraint` and `--dependency-override`. I don't love the setting name though (I find it non-obvious).

---

_Referenced in [astral-sh/uv#9502](../../astral-sh/uv/issues/9502.md) on 2024-12-27 13:58_

---
