```yaml
number: 254
title: Add error when user requests extras that do not exist
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/extra-error
created_at: 2023-10-31T15:47:49Z
updated_at: 2023-10-31T19:17:38Z
url: https://github.com/astral-sh/uv/pull/254
synced_at: 2026-01-12T16:03:49Z
```

# Add error when user requests extras that do not exist

---

_@zanieb_

Extends #253 
Closes #241 

Adds `extras` to `RequirementsSpecification` to track extras used to construct the requirements so we can throw an error when not all of the requested extras are used.

---

_Converted to draft by @zanieb on 2023-10-31 15:48_

---

_@charliermarsh reviewed on 2023-10-31 15:53_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 15:53_

Can we just have this return an error rather than returning the used extras?

---

_@zanieb reviewed on 2023-10-31 15:54_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 15:54_

No, if you provide multiple pyproject files the extra may be present in one but not the other and that is not an error.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 15:56_

Why can that not be handled within `RequirementsSpecification::try_from_sources`? That method already takes all the sources and all the extras.

---

_@charliermarsh reviewed on 2023-10-31 15:56_

---

_@zanieb reviewed on 2023-10-31 16:01_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 16:01_

Then we'd need to read and parse all of the sources twice? I'm confused.

---

_@zanieb reviewed on 2023-10-31 16:02_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 16:02_

To be clear: I'd prefer not to add `extras` to `RequirementsSpecification` but you'll need to be clearer about the proposed alternative because I considered a few other options and I don't see them working efficiently.

---

_@charliermarsh reviewed on 2023-10-31 16:26_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 16:26_

To rephrase: why can't the code you have below just go at the end of `try_from_sources`? That method has access to all the same information. (It's fine if `extras` goes on `RequirementsSpecification`, but why does this validation need to live here?)

---

_@zanieb reviewed on 2023-10-31 16:34_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 16:34_

Oh.. that could move there. Although I'd find it weird that `try_from_sources` errors on missing extras but `try_from_source` does not. I think that's why I pushed validation out of `RequirementsSpecification` itself.

---

_Marked ready for review by @zanieb on 2023-10-31 17:01_

---

_@charliermarsh reviewed on 2023-10-31 17:18_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:66 on 2023-10-31 17:18_

I think I'd suggest:

```rust
let mut unused_extras = extras
    .iter()
    .filter(|extra| !used.contains(extra))
    .collect::<Vec<_>>();
if !unused_extras.is_empty() {
    unused_extras.sort_unstable();
    unused_extras.dedup();
    let s = if unused_extras.len() == 1 { "" } else { "s" };
    return Err(anyhow!(
        "Requested extra{s} not found: {}",
        unused_extras.join(", ")
    ));
}
```

I feel like cloning and hashing all the extras is more work, since we have to iterate over and hash all the extras once, then do another pass to compute the difference.


---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:60 on 2023-10-31 17:18_

I think we're tending towards capitalizing these in the repo.

---

_@charliermarsh reviewed on 2023-10-31 17:18_

---

_@charliermarsh reviewed on 2023-10-31 17:18_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:56 on 2023-10-31 17:18_

I tend to use `.collect::<Vec<_>>` to reduce the uses of `itertools` which we may want to remove at some point.

---

_@charliermarsh reviewed on 2023-10-31 17:19_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:51 on 2023-10-31 17:19_

Hmm, ok, fair enough. I could see either being correct but agree with continuing with what you have here.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:61 on 2023-10-31 17:20_

Should we wrap each extra in backticks? (I'm not sure.)

---

_@charliermarsh reviewed on 2023-10-31 17:20_

---

_@charliermarsh approved on 2023-10-31 17:20_

Some feedback on the difference check, but otherwise LGTM.

---

_@zanieb reviewed on 2023-10-31 17:39_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:61 on 2023-10-31 17:39_

Only if the names can contain spaces and commas, to prevent ambiguity. 

---

_@zanieb reviewed on 2023-10-31 17:43_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:61 on 2023-10-31 17:43_

which they should not, but can now.. I'll handle that separately.

---

_@zanieb reviewed on 2023-10-31 19:14_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:61 on 2023-10-31 19:14_

#257 

---

_Merged by @zanieb on 2023-10-31 19:17_

---

_Closed by @zanieb on 2023-10-31 19:17_

---

_Branch deleted on 2023-10-31 19:17_

---
