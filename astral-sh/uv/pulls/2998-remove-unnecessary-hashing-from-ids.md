```yaml
number: 2998
title: Remove unnecessary hashing from IDs
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/canon
created_at: 2024-04-11T18:48:19Z
updated_at: 2024-04-11T21:23:38Z
url: https://github.com/astral-sh/uv/pull/2998
synced_at: 2026-01-10T14:43:31Z
```

# Remove unnecessary hashing from IDs

---

_Pull request opened by @charliermarsh on 2024-04-11 18:48_

## Summary

In all of these ID types, we pass values to `cache_key::digest` prior to passing to `DistributionId` or `ResourceId`. The `DistributionId` and `ResourceId` are then hashed later, since they're used as keys in hash maps.

It seems wasteful to hash the value, then hash the hashed value? So this PR modifies those structs to be enums that can take one of a fixed set of types.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-11 18:48_

---

_Label `internal` added by @charliermarsh on 2024-04-11 18:48_

---

_Marked ready for review by @charliermarsh on 2024-04-11 18:48_

---

_@BurntSushi approved on 2024-04-11 19:00_

I think this is an improvement, but I think you still get the "hashing a hashed value" behavior. Namely, the `Hash` impl of `DistributionId` is still going to re-hash the contents of the `HashDigest`. Maybe the way to look at this is that it overall avoids the creation of a new `String` in cases where it isn't needed.

---

_Comment by @charliermarsh on 2024-04-11 19:17_

Yeah, that specific case is _okay_ because that's a digest we receive from the server, not something we computed, but I agree it's still a bit strange.

---

_Merged by @charliermarsh on 2024-04-11 21:23_

---

_Closed by @charliermarsh on 2024-04-11 21:23_

---

_Branch deleted on 2024-04-11 21:23_

---
