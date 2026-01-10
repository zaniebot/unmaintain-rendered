```yaml
number: 3589
title: Add hashes and versions to all distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/package-resolve
created_at: 2024-05-14T19:26:38Z
updated_at: 2024-05-14T23:07:26Z
url: https://github.com/astral-sh/uv/pull/3589
synced_at: 2026-01-10T14:37:54Z
```

# Add hashes and versions to all distributions

---

_Pull request opened by @charliermarsh on 2024-05-14 19:26_

## Summary

In `ResolutionGraph::from_state`, we have mechanisms to grab the hashes and metadata for all distributions -- but we then throw that information away. This PR preserves it on a new `AnnotatedDist` (yikes, open to suggestions) that wraps `ResolvedDist` and includes (1) the hashes (computed or from the registry) and (2) the `Metadata23`, which lets us extract the version.

Closes https://github.com/astral-sh/uv/issues/3356.

Closes https://github.com/astral-sh/uv/issues/3357.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-14 19:26_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-14 19:26_

---

_Marked ready for review by @charliermarsh on 2024-05-14 19:26_

---

_Label `preview` added by @charliermarsh on 2024-05-14 19:26_

---

_@charliermarsh reviewed on 2024-05-14 19:27_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:619 on 2024-05-14 19:27_

Don't love making `hashes` an argument everywhere but seemed like the most straightforward option (since it only exists on `AnnotatedDist`).

---

_@ibraheemdev approved on 2024-05-14 20:15_

Looks good, don't have strong opinions about the naming. Does `LockedDist` make sense because it's locked to a specific version..?

---

_Comment by @charliermarsh on 2024-05-14 20:29_

👍 I'm gonna defer to @BurntSushi on the names since he has the bigger picture in his head w/r/t the other structs in `lock.rs` etc.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:619 on 2024-05-14 22:26_

Yeah hmmm, I don't like it either.

I think when I was writing this, I was assuming the hashes would be added to types like `PathSourceDist` directly. Out of curiosity, why didn't you take that approach? AIUI, hashes are on _some_ of the distribution types but not all of them. I was thinking we would make that a bit more consistent.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:647 on 2024-05-14 22:26_

Is there a difference between the hash on the annotated dist versus the hash that's already on the `RegistrySourceDist`?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution.rs`:277 on 2024-05-14 22:29_

I don't love these panics. (I think some like this were already in this code though.) I think this circles back to my idea of putting this data on the distribution types. Maybe that would lead to increased memory usage though? Not sure.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution.rs`:335 on 2024-05-14 22:30_

Not necessarily asking for any changes here, but just a "shout out loud": I feel like I've seen this pattern a lot where the code for processing distributions repeats itself. This looks almost identical to the metadata extraction above for example. (I don't yet have opinions on how to resolve this or even if it's worth resolving.)

---

_@BurntSushi approved on 2024-05-14 22:32_

I'm okay with this coming in as-is. I don't think I see anything wrong, although I don't love the code structure/organization. I elaborated a bit in the comments. It is possible that I might have some gaps in my understanding though, so I could have misguided opinions here.

---

_@charliermarsh reviewed on 2024-05-14 22:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:655 on 2024-05-14 22:51_

So... IIUC, `hashes` can be two different things. If the distribution comes from a registry, it's the list of hashes served by the registry. If the distribution comes from elsewhere, it's the hashes we _computed_.


---

_@charliermarsh reviewed on 2024-05-14 22:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:335 on 2024-05-14 22:54_

Yeah it's nearly identical. I'm gonna look at DRYing it up.

---

_@charliermarsh reviewed on 2024-05-14 22:55_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:619 on 2024-05-14 22:55_

They're not quite the same. The hash on the registry distribution is just the hash reported by the registry, _not_ the hash we compute locally. And the distribution exists well before we compute its hashes.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:647 on 2024-05-14 22:59_

In this case I think we should just ignore the hashes from the annotated dist here actually.

The hash on the `RegistrySourceDist` is the hash reported by the registry for that specific distribution (file). The hashes on the annotated dist will be the collection of all hashes for all distributions in the registry.

---

_@charliermarsh reviewed on 2024-05-14 22:59_

---

_@charliermarsh reviewed on 2024-05-14 23:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:277 on 2024-05-14 23:01_

We can _maybe_ put it on `ResolvedDist`? I'd have to look, but it'd be responsible to do so separately IMO since this pattern already exists here.

---

_@charliermarsh reviewed on 2024-05-14 23:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:335 on 2024-05-14 23:01_

Sorry, to be clear: it's exactly the same. They're different arms in a match.

---

_@charliermarsh reviewed on 2024-05-14 23:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:619 on 2024-05-14 23:03_

We don't compute hashes for registry distributions during resolution. We only _validate_ them during installation.

---

_Merged by @charliermarsh on 2024-05-14 23:07_

---

_Closed by @charliermarsh on 2024-05-14 23:07_

---

_@charliermarsh reviewed on 2024-05-14 23:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:277 on 2024-05-14 23:07_

Hmm, no, because we need to be able to go from compatible dist to resolved dist... At some point we need to merge the metadata with the distribution, and I guess that's here (for now, at least).

---

_Branch deleted on 2024-05-14 23:07_

---
