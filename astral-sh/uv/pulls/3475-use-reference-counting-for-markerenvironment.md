```yaml
number: 3475
title: use reference counting for MarkerEnvironment
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/arc-marker-environment
created_at: 2024-05-09T00:21:10Z
updated_at: 2024-05-09T14:06:03Z
url: https://github.com/astral-sh/uv/pull/3475
synced_at: 2026-01-12T16:05:40Z
```

# use reference counting for MarkerEnvironment

---

_@BurntSushi_

This PR makes the `MarkerEnvironment` type in the `pep508_rs` crate
fully encapsulated. It then changes its internal representation to use
an `Arc` so that clones are cheap.

This representation more closely matches how this type is commonly
used: it is constructed approximately once at startup from
interpreter state and then passed down into various routines for
requirement filtering. There are some parts of `uv` that mutate a
`MarkerEnvironment` (like setting the `python_full_version` based on
the `-p/--python` CLI flag) or create a new one (like from a target
triple), but these are generally "one time" things whose cost is
immaterial to the overall runtime of `uv`.

The impetus for this change was a loose desire to remove lifetimes from
some parts of the code. Namely, because `MarkerEnvironment` clones were
not cheap before this change, we typically used `&MarkerEnvironment`
everywhere. While not done in this PR, the idea is that in the future,
we can just use `MarkerEnvironment` instead.

I'm considering pursuing similar changes for other types. The change
for this type was somewhat gnarly due to how widely used it was.

The main downside of this change, and encapsulation in general, is that
it typically make the definition/implementation of the type itself
more verbose. For example, we have a new inner type, a new builder
type, getters, setters and the pyo3 `get_all` convenience function
no longer works, so we have to write that out ourselves too. But,
importantly, calling code can now clone without worry, and the calling
code generally doesn't get more much more complicated.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-09 00:32_

---

_Review requested from @konstin by @BurntSushi on 2024-05-09 00:32_

---

_@charliermarsh approved on 2024-05-09 00:59_

---

_Comment by @charliermarsh on 2024-05-09 05:13_

Mostly just trusting you on this one :)

---

_Merged by @BurntSushi on 2024-05-09 14:06_

---

_Closed by @BurntSushi on 2024-05-09 14:06_

---

_Branch deleted on 2024-05-09 14:06_

---
