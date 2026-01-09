---
number: 13903
title: How to test my project against multiple major library versions?
type: issue
state: closed
author: dimaqq
labels:
  - question
assignees: []
created_at: 2025-06-08T21:01:22Z
updated_at: 2025-09-22T06:56:40Z
url: https://github.com/astral-sh/uv/issues/13903
synced_at: 2026-01-07T13:12:18-06:00
---

# How to test my project against multiple major library versions?

---

_Issue opened by @dimaqq on 2025-06-08 21:01_

### Question

My library is supposed to work against several major versions of a dependency/tool.

While I can declare that, I'm struggling to come up with a workflow to choose a specific dep/tool major version form the lock file at install time.

I'm using `uv + tox-uv + tox` with `dependency_groups` in tox.ini.

Here's roughly what I'd like to do:

```toml
[dependency-groups]
...
j2 = [
    "juju~=2.9",
]
j3 = [
    "juju~=3.6",
]
```

And then either `dependency_group = j2` or possibly `uv sync --all-groups --constraint "juju~=2.9"`

### Platform

macos, linux

### Version

uv 0.7.12 (Homebrew 2025-06-06)

---

_Label `question` added by @dimaqq on 2025-06-08 21:01_

---

_Comment by @konstin on 2025-06-11 12:11_

Does `--resolution lowest-direct` work for this? By once using the regular resolution and once `--resolution lowest-direct`, you can test the oldest and newest supported versions.

If you want to use regular dependencies/dependency-groups, you need to define conflict groups so uv knows they are mutually exclusive.

---

_Comment by @zanieb on 2025-06-11 13:18_

https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies

---

_Comment by @dimaqq on 2025-06-12 01:24_

Yes, I think I can work around this with conflicts, thank you!

---

_Comment by @dimaqq on 2025-06-12 01:26_

P.S. resolution lowest is probably not the answer.

What I originally envisioned was smth like `juju ~= 2.9 OR juju ~= 3.6` where versions like 3.3 and 3.4 are explicitly not allowed. But then I'd probably want to test against both *latest* 2.x and latest 3.x; so "lowest" is not the answer.

---

_Referenced in [canonical/operator#1814](../../canonical/operator/issues/1814.md) on 2025-06-12 01:28_

---

_Comment by @tapetersen on 2025-06-13 10:38_

I also find myself wanting this and especially being able to lock the resolutions for the different "profiles" for reproducible ci-builds.
Using conflicting dependency-groups for some explicit cases works but I find myself wanting to do the same with `--resolution {lowest-direct,highest}` and have only been able to figure out how to do that with exporting to `requirements` or `pylock.toml` (I can open a separate issue for that if desired but I already see a couple that are touching on the subject of multiple lockfiles/resolutions indirectly)

---

_Comment by @dimaqq on 2025-09-22 06:56_

https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies is a workable solution.

---

_Closed by @dimaqq on 2025-09-22 06:56_

---

_Referenced in [gerrymanoim/exchange_calendars#501](../../gerrymanoim/exchange_calendars/issues/501.md) on 2025-10-03 11:45_

---
