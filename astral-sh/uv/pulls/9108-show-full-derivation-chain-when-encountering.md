```yaml
number: 9108
title: Show full derivation chain when encountering build failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/chain
created_at: 2024-11-14T00:36:25Z
updated_at: 2024-11-14T20:48:28Z
url: https://github.com/astral-sh/uv/pull/9108
synced_at: 2026-01-12T16:08:39Z
```

# Show full derivation chain when encountering build failures

---

_@charliermarsh_

## Summary

This PR adds context to our error messages to explain _why_ a given package was included, if we fail to download or build it.

It's quite a large change, but it motivated some good refactors and improvements along the way.

Closes https://github.com/astral-sh/uv/issues/8962.


---

_Marked ready for review by @charliermarsh on 2024-11-14 01:08_

---

_Label `error messages` added by @charliermarsh on 2024-11-14 01:08_

---

_@zanieb approved on 2024-11-14 04:58_

This looks great; a review from @BurntSushi seems prudent since I'm not as familiar with the resolver.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-14 05:18_

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/lib.rs`:182 on 2024-11-14 12:56_

Yay ref types.

I don't know if you saw it in my conflicting groups PR, but a useful tip if you ever need to use one of these types for key lookups in hashmaps is to use hashbrown and its [`Equivalent`](https://docs.rs/hashbrown/latest/hashbrown/trait.Equivalent.html) trait.

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/lib.rs`:533 on 2024-11-14 12:57_

Oh interesting, I thought Clippy didn't like omitting lifetimes like this. e.g., `DistRef<'_>`.

---

_Review comment by @BurntSushi on `crates/uv-requirements/src/lib.rs`:66 on 2024-11-14 13:02_

These new helper constructors are a nice refactor!

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/derivation.rs`:45 on 2024-11-14 13:05_

Without markers, I think this means that you might find the shortest path, but that path might not be possible? Or is there another downside I'm not seeing?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/derivation.rs`:122 on 2024-11-14 13:08_

This is neat.

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:20007 on 2024-11-14 13:13_

Maybe I missed it, but do you have a test for the case where the failed dependency is not a direct dependency of the root? I was looking for that to see what the error message looks like.

---

_@BurntSushi approved on 2024-11-14 13:13_

I like the refactor and this seems like an overall great UX improvement too!

---

_@zanieb reviewed on 2024-11-14 15:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:20007 on 2024-11-14 15:16_

I also was hoping for a longer chain :)

---

_@charliermarsh reviewed on 2024-11-14 16:57_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:20007 on 2024-11-14 16:57_

It's so annoying to test, but yeah, I definitely need to do it.

---

_Merged by @charliermarsh on 2024-11-14 20:48_

---

_Closed by @charliermarsh on 2024-11-14 20:48_

---

_Branch deleted on 2024-11-14 20:48_

---
