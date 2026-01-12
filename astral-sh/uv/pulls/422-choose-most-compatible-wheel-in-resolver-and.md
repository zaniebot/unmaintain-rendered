```yaml
number: 422
title: Choose most-compatible wheel in resolver and installer
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/any-wheel
created_at: 2023-11-14T04:12:39Z
updated_at: 2023-11-15T18:22:12Z
url: https://github.com/astral-sh/uv/pull/422
synced_at: 2026-01-12T16:03:56Z
```

# Choose most-compatible wheel in resolver and installer

---

_@charliermarsh_

## Summary

This PR implements logic to sort wheels by priority, where priority is defined as preferring more "specific" wheels over less "specific" wheels. For example, in the case of Black, my machine now selects `black-23.11.0-cp311-cp311-macosx_11_0_arm64.whl`, whereas sorting by lowest priority instead gives me `black-23.11.0-py3-none-any.whl`.

As part of this change, I've also modified the resolver to fallback to using incompatible wheels when determining package metadata, if no compatible wheels are available.

The `VersionMap` was also moved out of `resolver.rs` and into its own file with a wrapper type, for clarity.

Closes https://github.com/astral-sh/puffin/issues/380.
Closes https://github.com/astral-sh/puffin/issues/421.

---

_Marked ready for review by @charliermarsh on 2023-11-14 04:12_

---

_Review requested from @BurntSushi by @charliermarsh on 2023-11-14 04:12_

---

_Review requested from @konstin by @charliermarsh on 2023-11-14 04:12_

---

_@charliermarsh reviewed on 2023-11-14 04:13_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:141 on 2023-11-14 04:13_

This method could just as easily be `compatibility(...).is_some()`, except that it short-circuits when it finds the first compatible match. I figured it was fine to keep as-is.

---

_@charliermarsh reviewed on 2023-11-14 04:14_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:52 on 2023-11-14 04:14_

In this method, everything here and up is a straight move from `resolver.rs`.

---

_Review comment by @konstin on `crates/puffin-resolver/src/finder.rs`:160 on 2023-11-14 11:27_

I'm note sure if this assumption (files are in version order) holds up outside pypi, or even inside pypi, PEP 503 doesn't require any order. This isn't specific to this PR and doesn't need to be fixed now, but at some point or people may get the feeling that puffin is an unreliable resolver on their private registry because it seems to ignore versions at random

---

_Comment by @konstin on 2023-11-14 11:46_

Just so we're on the same page: This encodes the assumption that the wheels of a version, e.g. `black-23.11.0-cp311-cp311-macosx_11_0_arm64.whl` and `black-23.11.0-py3-none-any.whl` have the same metadata. But we also wouldn't look into an incompatible over building the sdist when resolving, right?

---

_@konstin approved on 2023-11-14 11:47_

---

_Review comment by @BurntSushi on `crates/platform-tags/src/lib.rs`:141 on 2023-11-14 13:14_

Yeah the short-circuiting is useful. And small/simple enough that trying to abstract it probably isn't worth it. I agree.

---

_Review comment by @BurntSushi on `crates/platform-tags/src/lib.rs`:19 on 2023-11-14 13:19_

I might add to the docs here that the priority given to each tag is based on its position in the `Vec` given. It looks like tags appearing earlier get higher priority?

---

_Review comment by @BurntSushi on `crates/platform-tags/src/lib.rs`:174 on 2023-11-14 13:20_

It might just be my brain, but it wasn't clear to me whether a lower or higher value was higher priority. I might add a quick note mentioning which it is. (From looking at the code elsewhere, it looks like a higher value implies a higher priority.)

---

_@BurntSushi approved on 2023-11-14 13:24_

Nice! I like it.

One thing that wasn't clear from me in this PR is how tag priority is actually determined. I see that it's assigned in `Tags::new` based on the order of the input, but how is that order determined? It might be worth calling that out somewhere in the comments.

---

_Comment by @charliermarsh on 2023-11-14 14:08_

> But we also wouldn't look into an incompatible over building the sdist when resolving, right?

@konstin -- We _will_ prefer an incompatible wheel over building a compatible sdist when resolving. Do you think that's incorrect?

(We will prefer the compatible sdist when _installing_. We will never install an incompatible wheel.)


---

_Comment by @konstin on 2023-11-14 14:11_

> (We will prefer the compatible sdist when installing. We will never install an incompatible wheel.)

ah thanks, that's the installer part of the resolver, makes sense!

---

_Comment by @charliermarsh on 2023-11-14 14:14_

@konstin - As an edge case... if there's _no_ source distribution, should we avoid falling back to an incompatible wheel in the resolver? Since we knowingly won't be able to install that package later.

---

_Comment by @konstin on 2023-11-14 14:26_

I tend towards yes, i think a missing version is less confusing than the installation later

---

_Comment by @charliermarsh on 2023-11-15 18:18_

@konstin - Will follow-up on that in a separate PR.

---

_Merged by @charliermarsh on 2023-11-15 18:22_

---

_Closed by @charliermarsh on 2023-11-15 18:22_

---

_Branch deleted on 2023-11-15 18:22_

---
