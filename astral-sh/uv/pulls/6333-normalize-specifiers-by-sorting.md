```yaml
number: 6333
title: Normalize specifiers by sorting
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/sort-version-specifiers
created_at: 2024-08-21T14:49:39Z
updated_at: 2024-08-29T21:06:20Z
url: https://github.com/astral-sh/uv/pull/6333
synced_at: 2026-01-10T12:53:33Z
```

# Normalize specifiers by sorting

---

_Pull request opened by @konstin on 2024-08-21 14:49_

We currently normalize package and extra names and drop the whitespace from version specifiers, but we were not normalizing the order of the specifiers. By sorting them we match the behavior of `packaging` and become independent of build backends reordering specifiers (#6332).

Surprisingly, the snapshot diff isn't large - most people were already writing sorted specifiers. Still, this will lead to observable differences in lockfiles between releases in cases where there are entries in `requires-dist` that were not previously sorted (while the total number of `requires-dist` is already small compared to the overall lockfile).


---

_Label `enhancement` added by @konstin on 2024-08-21 14:49_

---

_Review requested from @BurntSushi by @konstin on 2024-08-21 14:49_

---

_Review requested from @charliermarsh by @konstin on 2024-08-21 14:49_

---

_@BurntSushi approved on 2024-08-21 15:05_

This makes sense to me, but as discussed offline, we need to decide how we want to handle changes like this where it could cause a change to the lock file contents. The most annoying manifestation likely occurs when multiple people working on the same project use different versions of `uv` to make a change to the lock file. This could result in ping-ponging. Although it's worth saying that this change doesn't impact how lock files are read, so there is a high degree of compatibility.

I'm inclined to merge this as a bug fix, but I think there is a big known unknown here: we have no systematic way at present of measuring the impact of a change like this. For example, if most specifiers are sorted in lock files already, then this has very limited impact and seems okay to merge. On the other hand, if this changed almost every lock file, then that kind of impact would probably demand us to manage it differently.

My sense though, based on the snapshot updates here, is that the impact is much closer to the limited end of the spectrum.

---

_@charliermarsh approved on 2024-08-21 15:16_

---

_Comment by @konstin on 2024-08-29 21:02_

I've confirmed that `uv lock` doesn't change `requires-dist` when there aren't other changes anyway (the noop behavior remains untouched), i.e. there will only be changes to the lockfile (and could ping-pong between versions) when it gets changed anyway, and even then i expect those cases to be rare, so i'm merging this for the next release. 

---

_Label `enhancement` removed by @konstin on 2024-08-29 21:02_

---

_Label `bug` added by @konstin on 2024-08-29 21:02_

---

_Label `bug` removed by @konstin on 2024-08-29 21:02_

---

_Label `enhancement` added by @konstin on 2024-08-29 21:02_

---

_Merged by @konstin on 2024-08-29 21:06_

---

_Closed by @konstin on 2024-08-29 21:06_

---

_Branch deleted on 2024-08-29 21:06_

---
