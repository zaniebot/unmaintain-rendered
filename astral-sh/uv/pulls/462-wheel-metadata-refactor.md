```yaml
number: 462
title: Wheel metadata refactor
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: wheel-metadata-refactor
created_at: 2023-11-19T19:53:07Z
updated_at: 2023-11-20T16:26:38Z
url: https://github.com/astral-sh/uv/pull/462
synced_at: 2026-01-10T15:50:29Z
```

# Wheel metadata refactor

---

_Pull request opened by @konstin on 2023-11-19 19:53_

A consistent cache structure for remote wheel metadata:

 * `<wheel metadata cache>/pypi/foo-1.0.0-py3-none-any.json`
 * `<wheel metadata cache>/<digest(index-url)>/foo-1.0.0-py3-none-any.json`
 * `<wheel metadata cache>/url/<digest(url)>/foo-1.0.0-py3-none-any.json`

The source dist caching will use a similar structure (#468).

---

_@charliermarsh reviewed on 2023-11-19 20:11_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:624 on 2023-11-19 20:11_

Does this assume that the arbitrary URL supports range requests?

---

_@charliermarsh reviewed on 2023-11-19 20:11_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:624 on 2023-11-19 20:11_

It might be totally fine, I'm just making sure I understand the change. Previously, we downloaded the wheel and read the metadata from disk.

---

_@konstin reviewed on 2023-11-19 20:40_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:624 on 2023-11-19 20:40_

No there's a fallback where we download the entire wheel (to a cache dir, this is to much of an edge case to put that wheel into the wheel cache) 

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:27 on 2023-11-19 22:53_

One thing to note: using `<sha256(index-url)>` requires that we know the index URLs in advance. I think this is ok. For example, in the installer, we'll iterate over the list of known index URLs, and check each cache for a given wheel. But, to be clear, we won't be able to go from (cache shard) to (index URL).

---

_@charliermarsh reviewed on 2023-11-19 22:53_

---

_@charliermarsh reviewed on 2023-11-19 22:54_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:32 on 2023-11-19 22:54_

I think we should use `digest` from `puffin_cache` for this, and cast the URL to a `CanonicalUrl`.

---

_@charliermarsh reviewed on 2023-11-19 22:54_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:32 on 2023-11-19 22:54_

(We can probably avoid a dependency on Sha256.)

---

_@charliermarsh reviewed on 2023-11-19 22:55_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:209 on 2023-11-19 22:55_

This makes me feel like `IndexUrl` should perhaps be an enum?

---

_@charliermarsh reviewed on 2023-11-19 22:55_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:302 on 2023-11-19 22:55_

I think this should be within the `if`, so we don't allocate if it's already inflight, right?

---

_@konstin reviewed on 2023-11-20 09:14_

---

_Review comment by @konstin on `crates/puffin-cache/src/metadata.rs`:32 on 2023-11-20 09:14_

Do we trust that seahash that it never has hash collision on the url? The docs say:

> It is not secure, nor does it aim to be. It aims to have high quality pseudorandom output and few collisions, as well as being fast.

Since we get the urls from remote i consider them untrusted input.

---

_@charliermarsh reviewed on 2023-11-20 11:12_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:32 on 2023-11-20 11:12_

I personally think this is okay because users are expected to trust their indexes. I believe the security model is roughly: as secure as the least secure index that you rely on, right? Regardless, I think that if we don't trust Seahash for this, we should reconsider it for other use-cases too, like hashing Git repos.

---

_Comment by @konstin on 2023-11-20 14:16_

Current dependencies on/for this PR:
* main
  * **PR #462** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/462?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #468** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/468?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/462?utm_source=stack-comment).

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:133 on 2023-11-20 14:41_

Should we restructure this method such that we avoid this assignment if it's a Git dependency? E.g., use `if` with `return` rather than `if`-`else`?

---

_@charliermarsh approved on 2023-11-20 15:55_

---

_@charliermarsh reviewed on 2023-11-20 15:56_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:133 on 2023-11-20 15:56_

(Sorry, I think this comment was stale and accidentally-included in my approval.)

---

_Merged by @konstin on 2023-11-20 16:26_

---

_Closed by @konstin on 2023-11-20 16:26_

---

_Branch deleted on 2023-11-20 16:26_

---
