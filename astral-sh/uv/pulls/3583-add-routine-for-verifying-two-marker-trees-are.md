```yaml
number: 3583
title: Add routine for verifying two marker trees are disjoint
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: marker-tree-intersection
created_at: 2024-05-14T17:21:22Z
updated_at: 2024-05-15T17:01:06Z
url: https://github.com/astral-sh/uv/pull/3583
synced_at: 2026-01-10T14:37:54Z
```

# Add routine for verifying two marker trees are disjoint

---

_Pull request opened by @ibraheemdev on 2024-05-14 17:21_

## Summary

Implements https://github.com/astral-sh/uv/issues/3355.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/marker.rs`:13 on 2024-05-14 17:26_

Perfect.

---

_@BurntSushi reviewed on 2024-05-14 17:37_

---

_Marked ready for review by @ibraheemdev on 2024-05-14 18:53_

---

_Comment by @ibraheemdev on 2024-05-14 19:01_

This should be ready, but I have a few concerns.

According to PEP508, `python_version` is defined to be to `'.'.join(platform.python_version_tuple()[:2])` (i.e. no patch version, as opposed to `python_full_version`). However, we don't handle this in the existing marker code, so `python_version == '3.7'` will fail if the environment contains version `3.7.1`, which I believe is incorrect? To make this work with pubgrub we would have to normalize all marker python versions to `x.y.*`.

The check currently doesn't consider the special case of `python_version` and `python_full_version`, which are related and can be disjoint.

However, I think this can be merged as is and `python_version` markers be fixed separately.

---

_@BurntSushi approved on 2024-05-15 12:27_

Sweet. The tests look great. This is awesome.

One thing I noticed (but didn't realize yesterday) is that this is in `uv-resolver`. I think that's fine because it should be the only place we need it. But it does seem like it would be nice for this to live in the `pep508_rs` crate. We'd have to change the implementation to not use `PubGrubSpecifier` though. But, I think it's fine for it to live here until there's a use case to move it. :-)

---

_Comment by @BurntSushi on 2024-05-15 12:28_

> However, I think this can be merged as is and `python_version` markers be fixed separately.

Yeah I think this is fine.

---

_Comment by @ibraheemdev on 2024-05-15 17:01_

I originally did have this in `pep508_rs`, but moved it because of how much simpler it would be to use `pubgrub`.

---

_Merged by @ibraheemdev on 2024-05-15 17:01_

---

_Closed by @ibraheemdev on 2024-05-15 17:01_

---
