```yaml
number: 5074
title: add split to cover marker universe when existing splits are incomplete
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/incomplete-markers
created_at: 2024-07-15T14:48:26Z
updated_at: 2024-07-16T09:00:36Z
url: https://github.com/astral-sh/uv/pull/5074
synced_at: 2026-01-12T16:06:37Z
```

# add split to cover marker universe when existing splits are incomplete

---

_@BurntSushi_

In some cases, it's possible for the marker expressions on conflicting
dependency specification to be disjoint but *incomplete*. That is, if
one unions the disjoint markers, the result is not the complete set
of marker environments possible. There may be some "gap" of marker
environments not covered by the markers.

This is a problem in practice because, before this commit, we only
created forks in the resolver for specific marker expressions. So if a
dependency happened to fall in a "gap," our resolver would never see
it.

This commit fixes this by adding a new split covering the negation of
the union of all marker expressions in a set of forks for a specific
package.

Originally, I had planned on only creating this split when it was
known that the gap actually existed. That is, when the negation of the
marker expressions did *not* correspond to the empty set. After a lot
of thought, unfortunately, this (I believe) effectively boils down to
3SAT, which is NP-complete.

Instead, what we do here is *always* create an extra split unless we
can definitively tell that it is empty. We look for a few cases, but
otherwise throw our hands up and potentially do wasted work.

This also updates the lock scenario tests to reflect the actual bug fix
here.

Ref #4732, Fixes #4687

---

_Comment by @BurntSushi on 2024-07-15 14:51_

One interesting consequence of this PR is that, even in very basic forks, this will create an additional fork covering the rest of the universe. For example:

```
requires = [
  "a>=2 ; sys_platform == 'linux'",
  "a<2 ; sys_platform == 'darwin'",
]
```

While those marker expressions are disjoint, they do not cover everything. So this PR will result in an additional fork with `sys_platform != "linux" and sys_platform != "darwin"`.

We do try to avoid creating forks if we know the marker expressions can never be true, but after some thought, I believe that's equivalent to solving 3SAT.

---

_Review requested from @konstin by @BurntSushi on 2024-07-15 14:51_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-07-15 14:51_

---

_@ibraheemdev approved on 2024-07-15 17:01_

This makes sense to me.

---

_Merged by @BurntSushi on 2024-07-15 17:09_

---

_Closed by @BurntSushi on 2024-07-15 17:09_

---

_Branch deleted on 2024-07-15 17:09_

---

_@konstin reviewed on 2024-07-16 09:00_

nice work!

---
