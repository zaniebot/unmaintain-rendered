```yaml
number: 910
title: Make hashes optional
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/optional-hashes
created_at: 2024-01-14T12:34:18Z
updated_at: 2024-01-14T21:32:57Z
url: https://github.com/astral-sh/uv/pull/910
synced_at: 2026-01-12T16:04:17Z
```

# Make hashes optional

---

_@konstin_

There is no guarantee that indexes provide hashes at all or the sha256 we support specifically. [PEP 503](https://peps.python.org/pep-0503/#specification):

> The URL SHOULD include a hash in the form of a URL fragment with the following syntax: #<hashname>=<hashvalue>, where <hashname> is the lowercase name of the hash function (such as sha256) and <hashvalue> is the hex encoded digest.

We instead use the url as input to generate a hash when caching.

---

_@konstin reviewed on 2024-01-14 12:36_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:47 on 2024-01-14 12:36_

This may be the relative url, should we change the dists to carry the absolute url instead? In my mental model it makes more sense than passing the `BaseUrl` around. Alternatively we could pass the base url into `DistributionDatabase`. As it currently is we run the risk of collisions.

---

_@charliermarsh reviewed on 2024-01-14 15:22_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:669 on 2024-01-14 15:22_

Shouldn't this be `self.url.distribution_id()`?

---

_@charliermarsh reviewed on 2024-01-14 15:25_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:97 on 2024-01-14 15:25_

This isn't a `str`, it allocates.

---

_@charliermarsh reviewed on 2024-01-14 15:25_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:674 on 2024-01-14 15:25_

This strings me as an unnecessary allocation given that we call this a lot.

---

_@charliermarsh reviewed on 2024-01-14 15:27_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:47 on 2024-01-14 15:27_

Shouldn't this use `cache_key::digest(&cache_key::RepositoryUrl::new(self.url))`? Why an entirely new hash construction vs. what we use elsewhere?

---

_@charliermarsh reviewed on 2024-01-14 15:27_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:43 on 2024-01-14 15:27_

I kinda think this method shouldn't exist. In the URL case, it's kind of misleading since it doesn't return a hash of the file.

---

_@charliermarsh reviewed on 2024-01-14 15:28_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:43 on 2024-01-14 15:28_

Can we instead just call `.distribution_id` on the file wherever we're using this, to make it clearer?

---

_Comment by @konstin on 2024-01-14 15:40_

Current dependencies on/for this PR:
* `main`
  * **PR #910** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/910?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #912** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #913** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #914** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/910?utm_source=stack-comment).

---

_@konstin reviewed on 2024-01-14 15:45_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:47 on 2024-01-14 15:45_

Yeah that's better than going with sha256 consistency; `cache_key::digest(&self.url)` works, self.url is a string.

---

_@konstin reviewed on 2024-01-14 15:45_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:669 on 2024-01-14 15:45_

It's a string not a url and doesn't have that method

---

_@charliermarsh approved on 2024-01-14 21:32_

---

_Merged by @charliermarsh on 2024-01-14 21:32_

---

_Closed by @charliermarsh on 2024-01-14 21:32_

---

_Branch deleted on 2024-01-14 21:32_

---
