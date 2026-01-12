```yaml
number: 5484
title: "`uv pip install -e .` does not recompute dynamic dependencies anymore"
type: issue
state: closed
author: sbidoul
labels:
  - documentation
  - question
assignees: []
created_at: 2024-07-26T15:51:13Z
updated_at: 2024-08-16T21:01:13Z
url: https://github.com/astral-sh/uv/issues/5484
synced_at: 2026-01-12T15:58:56Z
```

# `uv pip install -e .` does not recompute dynamic dependencies anymore

---

_@sbidoul_

I have a project with dynamic dependencies.

With uv 0.2.29 `uv pip install -e .` does not install new dynamic dependencies when I add some to my project, while it did reinstall with uv < 0.2.28. 

This is likely due to https://github.com/astral-sh/uv/pull/5206. 

I understand the rationale for `uv run`, but for the scenario of reinstalling a project in editable mode, this behavior is surprising. Was this change intended?

---

_Comment by @charliermarsh on 2024-07-26 15:57_

It was an intentional change. The decision was that if you want your editable project to "always reinstall", you should set `reinstall-package = ["foo"]` in your persistent configuration.

We could consider changing the behavior for explicit cases like `pip install -e .` though, and always reinstall those (as opposed to in `uv run` where it's sort of implicit). I don't love have separate semantics which is why I avoided it, and instead made it opt-in...


---

_Comment by @sbidoul on 2024-07-26 16:11_

Ok, thanks.

And if I have a git dependency `foo @ git+https://github.com/org/foo@shaX` that I change to `foo @ git+https://github.com/org/project@shaY` and the `pyproject.toml` of `foo` over there has not changed between `shaX` and `shaY` but the dynamic dependencies of foo have changed, will it recompute the dependencies ?

---

_Comment by @charliermarsh on 2024-07-26 16:52_

Yes, in that case it should recompute dependencies.

---

_Comment by @charliermarsh on 2024-07-26 16:52_

I will test it to be sure.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 16:52_

---

_Comment by @sbidoul on 2024-07-26 16:53_

I just did my homework and it does indeed recompute in that case. 

---

_Comment by @sbidoul on 2024-07-26 17:01_

But I'm a little confused about when dynamic metadata is recomputed and when not.

---

_Comment by @charliermarsh on 2024-07-26 17:22_

No worries. The rules only changed for source trees (i.e., directories), and they're the same regardless of whether it's editable or not, and regardless of whether it's dynamic or not: we recompute and reinstall if (1) a metadata file changes (`pyproject.toml`, `setup.py`, or `setup.cfg` -- this is a heuristic) or (2) the user passes `--reinstall`.

A few examples:

```shell
❯ cargo run pip install ./scripts/packages/black_editable
Resolved 1 package in 9ms
   Built black @ file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable
Prepared 1 package in 345ms
Installed 1 package in 1ms
 + black==0.1.0 (from file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable)

❯ cargo run pip install ./scripts/packages/black_editable
Audited 1 package in 6ms

❯ touch scripts/packages/black_editable/pyproject.toml

❯ cargo run pip install ./scripts/packages/black_editable
Resolved 1 package in 8ms
   Built black @ file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable
Prepared 1 package in 109ms
Uninstalled 1 package in 0.59ms
Installed 1 package in 1ms
 - black==0.1.0 (from file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable)
 + black==0.1.0 (from file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable)

❯ cargo run pip install ./scripts/packages/black_editable
Audited 1 package in 6ms

❯ cargo run pip install ./scripts/packages/black_editable --reinstall black
Resolved 1 package in 7ms
   Built black @ file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable
Prepared 1 package in 178ms
Uninstalled 1 package in 0.71ms
Installed 1 package in 1ms
 - black==0.1.0 (from file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable)
 + black==0.1.0 (from file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable)
```


---

_Label `question` added by @charliermarsh on 2024-07-26 18:34_

---

_Comment by @sbidoul on 2024-07-28 08:51_

Thanks for the explanation. So it's not only about metadata, but the whole local directory, correct?

If I do a non-editable install of a local directory, then modify it, I have to use `--reinstall` (which reinstalls everything) or `--reinstall-package myprojectname` (for which I have to remember the project name).

When used from the command line this feels slightly awkward to me. When used from uv run or for a multi project workspace the new default may be better. I don't know if it's my own habits influencing my perception here or if there is something else.

BTW, in the example above it should be `--reinstall-package black` instead of `--reinstall black`, right? Perhaps this is indicative of a UX intuitiveness issue?


---

_Comment by @Nomelas on 2024-08-01 18:59_

Thanks, this solution by @charliermarsh seems to work and suit my needs

---

_Comment by @zanieb on 2024-08-01 19:23_

@charliermarsh can you own including this in the documentation somewhere?

---

_Label `documentation` added by @zanieb on 2024-08-01 19:23_

---

_Comment by @charliermarsh on 2024-08-01 20:36_

Yes for sure.

---

_Closed by @charliermarsh on 2024-08-16 21:01_

---

_Closed by @charliermarsh on 2024-08-16 21:01_

---
