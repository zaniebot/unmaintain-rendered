```yaml
number: 16089
title: "Error on dependency groups with incompatible requires-python in `pip compile`"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: grouped-error-messages
created_at: 2025-10-01T17:36:17Z
updated_at: 2025-10-07T18:55:17Z
url: https://github.com/astral-sh/uv/pull/16089
synced_at: 2026-01-12T16:12:06Z
```

# Error on dependency groups with incompatible requires-python in `pip compile`

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/15889

This PR updates the `dependency-group` metadata to carry each group’s `Requires-Python`, and `uv pip compile` now aborts when a requested group can’t run on the active interpreter. This mirrors the project-level behavior, preventing the confusing “empty lockfile” outcome users were seeing. 

I've also added a new integration test that covers the regression scenario that prompted the change.

---

_Comment by @terror on 2025-10-01 17:50_

Test failure seems to be unrelated? 🤔 

---

_Marked ready for review by @terror on 2025-10-01 17:50_

---

_@zanieb reviewed on 2025-10-01 20:08_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:16099 on 2025-10-01 20:08_

We might want to include a call to action here, e.g., suggesting they use `--python 3.14`? I'm not sure how easy it will be to generalize that though.

---

_@zanieb reviewed on 2025-10-01 20:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:244 on 2025-10-01 20:08_

Did you derive this error message from a similar existing one?

---

_Assigned to @zanieb by @zanieb on 2025-10-01 20:09_

---

_Review comment by @terror on `crates/uv/src/commands/pip/operations.rs`:244 on 2025-10-01 20:17_

I took some inspiration from the 'The requested interpreter resolved to Python {}, which is incompatible with the `pylock.toml`'s Python requirement: `{}`' message from [`sync.rs`](https://github.com/astral-sh/uv/blob/d51a1e74567422f9ea32b40a31634a84d04f0c00/crates/uv/src/commands/pip/sync.rs#L411), but this is ultimately a different context so I re-framed it.

---

_@terror reviewed on 2025-10-01 20:17_

---

_@zanieb reviewed on 2025-10-01 20:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:244 on 2025-10-01 20:21_

My concern here is that we're deriving a range from the current interpreter version, e.g., `>=3.13` from `3.13`, which I don't think will be obvious to users encountering this error message.

---

_@terror reviewed on 2025-10-01 21:14_

---

_Review comment by @terror on `crates/uv/tests/it/pip_compile.rs`:16099 on 2025-10-01 21:14_

Hmm, for groups that provide a lower bound (e.g. in this case), we could use the minimum required python version, i.e we could add ```Re-run with `--python 3.14` to target a compatible Python version```. For other cases we could add something more generic. What do you think?

---

_@terror reviewed on 2025-10-01 21:20_

---

_Review comment by @terror on `crates/uv/src/commands/pip/operations.rs`:244 on 2025-10-01 21:20_

Ah interesting, yeah I see why there's concern there. I've pushed up some changes to make things a bit more clear:

```
error: Dependency group `ml1` in `pyproject.toml` requires Python `>=3.14`, but uv is resolving for Python `>=3.13.[X]` (current interpreter: `3.13.[X]`). Re-run with `--python 3.14` to target a compatible Python version.
```

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:16132 on 2025-10-02 08:57_

We should make the Python version available through the test context

---

_@konstin reviewed on 2025-10-02 08:57_

---

_@zanieb reviewed on 2025-10-07 18:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:244 on 2025-10-07 18:55_

Sorry for the delay, the blocker here was me looking into this a bit more. Basically, I think we can do better with the error message here given the information on `PythonRequirement`

https://github.com/astral-sh/uv/blob/50218409191c55587e832e649699ef5d07a611e8/crates/uv-resolver/src/python_requirement.rs#L9-L19

Specifically, the `PythonRequirementSource` is helpful

https://github.com/astral-sh/uv/blob/50218409191c55587e832e649699ef5d07a611e8/crates/uv-resolver/src/python_requirement.rs#L179-L186

So we can avoid showing a range at all in the typical case, e.g., instead of "uv is resolving for Python `>=3.13.[X]` ..." we can say "uv is resolving for Python 3.13". I think only in the `RequiresPython` case do we need to show a range? and then we can just indicate that it comes from `requires-python`.

---
