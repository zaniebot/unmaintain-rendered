---
number: 5178
title: "Should `uv add` set lower version bounds?"
type: issue
state: closed
author: ibraheemdev
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-18T04:01:24Z
updated_at: 2024-08-01T17:42:12Z
url: https://github.com/astral-sh/uv/issues/5178
synced_at: 2026-01-07T13:12:17-06:00
---

# Should `uv add` set lower version bounds?

---

_Issue opened by @ibraheemdev on 2024-07-18 04:01_

Currently, `uv add foo` adds `"foo"` as a requirement with no version constraint. Should it instead perform a resolution and require `"foo>X"`?

There's a lot of resistance against upper bounds so we probably don't want to add them by default (though I don't have much context about this problem myself), but lower bounds are reasonable to include.

---

_Label `preview` added by @ibraheemdev on 2024-07-18 04:01_

---

_Comment by @zanieb on 2024-07-18 04:02_

I think this is a great default. cc @konstin 

---

_Comment by @baggiponte on 2024-07-18 12:33_

Most package managers add lower bounds (though I don't know how they select the version to solve for). I was bitten by this today because in CI I cannot use uv lockfiles and I resorted to export with `--resolution=lowest-direct` and a version 0.1.0 of some library was installed (lol).

My 2 cents: no upper boundaries. Python is not JS: we don't have peer dependencies. Distributing a library with `<=` can have unintended consequences. I always cite these [two](https://iscinumpy.dev/post/bound-version-constraints/) [articles](https://iscinumpy.dev/post/poetry-versions/) since the author probably needs no introduction.

---

_Comment by @zanieb on 2024-07-18 14:34_

We've already had extensive discussion on upper bounds and feel similarly. 

We also added a warning for missing lower bounds with that resolution mode in #2797 / https://github.com/astral-sh/uv/pull/5142

---

_Comment by @charliermarsh on 2024-07-19 01:47_

What do we want the lower bound to be... The version we resolve to, or the minimum compatible version in the resolution?

---

_Comment by @nathanjmcdougall on 2024-07-19 02:43_

Adding a lower bound carries the implication that the codebase isn't compatible with earlier versions. If we resolve to the latest version of the package, this ends up being a quite strict assumption and can force a dependent installing your package to upgrade unnecessarily. This might be desirable initially until the codebase has actually been tested on older versions of packages; but it is arguably too cautious, especially when using `uv add` on mature packages which are relatively feature complete.

On the other hand, if a user starts by adding a single dependency (e.g. `uv add numpy`) then the minimum compatible version will often be a very old release indeed. If this is used as a lower bound, it could be misleading, since it would imply the package has been tested/confirmed to work correctly down to that lower bound. It also becomes subject to the arbitrariness of the order in which the user adds dependencies one-by-one.

Personally, I think using the version we resolve to is more sensible. Perhaps only include the Major.Minor parts of a SemVer version.

---

_Comment by @konstin on 2024-07-19 07:52_

I've been thinking that as a follow-up to adding a lower bounds with `uv add`, we could have a script that bisects lower bounds for you:

Say you added `numpy`, which defaults to `numpy>=2.0.0`, and then run `uv losen-bounds --lower numpy "python -m pytest"`.

From the resolution, we know that there is a `>=1.25.0` from another dependency (this feature is more important when we don't have that lower bound, but i need to bound my example to not be too big to write out). numpy then has the following versions remaining:

2.0.0 2.0.0rc2 2.0.0rc1 2.0.0b1 1.26.4 1.26.3 1.26.2 1.26.1 1.26.0 1.26.0rc1 1.26.0b1 1.25.2 1.25.1 1.25.0

We remove prerelease versions since they are not selected:

2.0.0 1.26.4 1.26.3 1.26.2 1.26.1 1.26.0 1.25.2 1.25.1 1.25.0

We start with `1.25.0`, pytest fails. We try `1.26.1`, pytest succeeds. We try `1.25.2`, pytest fails. We try `1.26.0`, pytest suceeds. We write `>=1.26.0` to your pyproject.toml.

---

_Label `enhancement` added by @zanieb on 2024-07-22 23:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-23 02:45_

---

_Comment by @charliermarsh on 2024-07-23 02:45_

I can do it.

---

_Comment by @charliermarsh on 2024-07-24 13:38_

This is a bit of a pain because we need to add the requirement before resolving, then resolve, then update the `pyproject.toml` with the resolved version. But there are some tricky cases... like, what if we resolve multiple versions of the package, etc.

---

_Comment by @zanieb on 2024-07-24 14:27_

I wouldn't be hurt if we dropped this from the release-ready milestone.

---

_Comment by @charliermarsh on 2024-07-31 02:39_

I will try to do this tomorrow.

---

_Referenced in [astral-sh/uv#5688](../../astral-sh/uv/pulls/5688.md) on 2024-08-01 14:58_

---

_Closed by @charliermarsh on 2024-08-01 17:06_

---

_Comment by @notatallshaw on 2024-08-01 17:42_

I'm a bit late to the conversation, but one heuristic you might find as a helpful fallback if users of projects have not setup or do not use pytest, is if the dependency provides wheels, what's the earliest version of the dependency that has compatible wheels with some version of Python that matches those wheels. 

E.g. For numpy if your project is `Python-Requires >= 3.9`  you might be able to determine that `numpy >= 1.19.3`

---

_Referenced in [astral-sh/uv#6783](../../astral-sh/uv/issues/6783.md) on 2024-08-29 03:33_

---
