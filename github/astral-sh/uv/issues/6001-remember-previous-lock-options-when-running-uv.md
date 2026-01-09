---
number: 6001
title: "Remember previous lock options when running `uv sync`"
type: issue
state: open
author: nikhilweee
labels: []
assignees: []
created_at: 2024-08-11T03:02:17Z
updated_at: 2024-08-20T18:21:38Z
url: https://github.com/astral-sh/uv/issues/6001
synced_at: 2026-01-07T13:12:17-06:00
---

# Remember previous lock options when running `uv sync`

---

_Issue opened by @nikhilweee on 2024-08-11 03:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
**Problem Description**

`uv sync`, by itself, does not install optional dependencies. Which is fine, I believe it's how it should be. 

But let's say I am running `uv sync` for the first time. I wish to include two extras, `extra1` and `extra2`. I run the following:
```
uv sync --extra extra1 --extra extra2
```
Great. Now I have the package with extras installed. But what about subsequent syncs?

**Suggestion**

One argument is to remember the two extras (and other lock options) from before, and use them as the new defaults. In other words, subsequent syncs should imply `--extra extra1 --extra extra2`. If an extra argument is provided explicitly during sync, these new extras should override the defaults. I believe this is how it currently works in `rye` and `pdm`. Those tools store lock options in their lockfiles.

The PDM lockfile has a metadata section which stores these implicit extras
```toml
[metadata]
groups = ["extra1", "extra2"]
```
Similarly, Rye stores these implicit extras in its lockfile
```
# last locked with the following flags:
#   features: ["extra1", "extra2"]
```
AFAIK I don't think UV stores lock options in its lockfile. Should we think about remembering previous lock options?

**Related**

I am aware that that [PEP-751](https://peps.python.org/pep-0751/) is under discussion and from a brief glance I'm not sure if package managers will be able to store any kind of metadata in there. That said, it's still a draft and maybe we can bring this up.


---

_Comment by @zanieb on 2024-08-11 14:55_

The discussion at https://github.com/astral-sh/uv/issues/4730 may be helpful

---

_Comment by @nikhilweee on 2024-08-11 17:09_

Thanks for the pointer @zanieb, I think the following comment comes close to capturing the gist of this issue.

https://github.com/astral-sh/uv/issues/4730#issuecomment-2203338364
> The huge drawback here: Modulo default-{packages,extras,command}, you have to specify all arguments in every uv run every time.

I'd also like to add that remembering lockfile options would make it easy for users switching from other package managers.

---

_Comment by @charliermarsh on 2024-08-11 18:51_

To clarify one thing, the extras have no impact on the lockfile -- we lock for all extras at once. `uv lock` doesn't even accept extras as an argument :) The extras only impact the installation phase (which subset of packages should we install), not the locking phase (which packages should we include when resolving). This is different from Rye which can't produce a lockfile that supports switching out extras at install-time, so has to encode them like that.

---

_Label `preview` added by @charliermarsh on 2024-08-11 18:51_

---

_Comment by @nikhilweee on 2024-08-20 16:10_

Ah, sorry my understanding of uv internals is not the best. That said, the spirit of this issue is to remember previous lock options when running `uv install`. If this is something of interest, I'm sure the implementation details can be discussed further.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Referenced in [pulumi/pulumi#17853](../../pulumi/pulumi/issues/17853.md) on 2024-12-05 11:46_

---
