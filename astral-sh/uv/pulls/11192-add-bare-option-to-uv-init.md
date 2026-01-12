```yaml
number: 11192
title: "Add `--bare` option to `uv init`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/bare
created_at: 2025-02-03T19:27:53Z
updated_at: 2025-02-05T16:12:29Z
url: https://github.com/astral-sh/uv/pull/11192
synced_at: 2026-01-12T16:09:43Z
```

# Add `--bare` option to `uv init`

---

_@zanieb_

People are looking for a less opinionated version of `uv init`. The goal here is to create a `pyproject.toml` and nothing else. With the `--lib` or `--package` flags, we'll still configure a build backend but we won't create the source tree. This disables things like the default `description`, author behavior, and VCS.

See

- https://github.com/astral-sh/uv/issues/8178
- https://github.com/astral-sh/uv/issues/7181
- https://github.com/astral-sh/uv/issues/6750


---

_Label `cli` added by @zanieb on 2025-02-03 19:27_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:252 on 2025-02-03 19:56_

This is a little dubious (implementation-wise), but I didn't want to get sucked into refactoring the `description` handling.

---

_@zanieb reviewed on 2025-02-03 19:56_

---

_Marked ready for review by @zanieb on 2025-02-03 19:56_

---

_Comment by @zanieb on 2025-02-03 19:56_

I'll want to add some prose documentation too, but let's see where this goes first.

---

_Review requested from @charliermarsh by @zanieb on 2025-02-03 19:57_

---

_Comment by @zanieb on 2025-02-03 20:19_

Also open to another name? I'm not sure what though.

Some people in the linked issues think this should be the default and everything else should be opt-in. I'm a little hesitant about that.

---

_Comment by @Gankra on 2025-02-03 21:02_

> Some people in the linked issues think this should be the default and everything else should be opt-in. I'm a little hesitant about that.

I do think this flag/mode is a good baseline for other flags to opt back into features. (`--bare --vcs=git ...`).

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2584 on 2025-02-03 21:05_

huh, TIL

---

_@Gankra approved on 2025-02-03 21:08_

Don't have a strong opinion of the name but the impl seems reasonable.

---

_@Gankra reviewed on 2025-02-03 21:12_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/init.rs`:878 on 2025-02-03 21:12_

Wait hold on shouldn't these be wrapped too (in a few places?)

---

_@edmorley reviewed on 2025-02-03 21:13_

---

_Review comment by @edmorley on `crates/uv-cli/src/lib.rs`:2523 on 2025-02-03 21:13_

I wonder whether `--bare` should still create a `.python-version` file (if one doesn't already exist), given how integral it is to the deterministic operation of uv? It seems like most of the feedback about unwanted files/content were about the `.py` files and directory nesting, rather than the `.python-version` file - so people might be fine with it left in?

---

_@zanieb reviewed on 2025-02-03 21:15_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2523 on 2025-02-03 21:15_

People are sort of constantly complaining about the `.python-version` file actually :/

It don't think it's **required** for reasonably deterministic action if you have a `requires-python` in your project. You may be on a different version than someone else, but the lockfile will be consistent.

---

_@zanieb reviewed on 2025-02-03 21:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:878 on 2025-02-03 21:16_

It happens up at https://github.com/astral-sh/uv/pull/11192/files#diff-a459ffa6a304d440657379d39bdef792a801589e6f6cbf1ae01b50754ab5fa98R262

A little questionable, but makes the `--vcs git` behavior easier.

---

_Review comment by @edmorley on `crates/uv-cli/src/lib.rs`:2523 on 2025-02-03 21:21_

> People are sort of constantly complaining about the `.python-version` file actually :/

Yeah I've seen quite a few comments, but from what I remember they were in other issues (not about `uv init` defaults), and generally were about lack of understanding why the seemingly duplicate concepts were needed. 

I think longer term perhaps campaigning for an additional Python version field to be added to the `pyproject.toml` spec would resolve the "why do I need another file" concerns, but for now I think having a pinned Python version via a `.python-version` file really is still important. Otherwise the day after uv adds support for eg Python 3.14, some developers on a project will end up using Python 3.14 (with lack of wheels), whereas others on their team (who may not have updated uv yet) could still be using Python 3.13 etc - leading to only some people seeing build failures (due to building from source), CI failures, lack of dev-prod parity etc.

It's very likely I'm going to make a missing `.python-version` file a hard error when deploying a uv app on Heroku for example.

---

_@edmorley reviewed on 2025-02-03 21:21_

---

_@zanieb reviewed on 2025-02-03 21:25_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2523 on 2025-02-03 21:25_

Yeah, I think your take is fair.

My primary concern is that it doesn't make much sense to teach `--bare` if it does more than just create a `pyproject.toml` with standards-compliant fields. I'd rather add opt-in support via `--pin-python` and suggest using it.

It seems feasible that there'd be interest in a standard for a Python version pin in the `pyproject.toml`; especially now that there are dependency groups that do not require `[project]` metadata. I'm not excited about trying to pitch it though :D I agree this is the obvious long-term solution over a non-standard `.python-version` file.

---

_Merged by @zanieb on 2025-02-05 16:12_

---

_Closed by @zanieb on 2025-02-05 16:12_

---

_Branch deleted on 2025-02-05 16:12_

---
