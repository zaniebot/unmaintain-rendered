```yaml
number: 2946
title: "Make `Glob.literal()` a public function"
type: pull_request
state: closed
author: orf
labels: []
assignees: []
base: master
head: patch-1
created_at: 2024-12-09T16:01:04Z
updated_at: 2025-01-24T18:04:39Z
url: https://github.com/BurntSushi/ripgrep/pull/2946
synced_at: 2026-01-12T18:23:14Z
```

# Make `Glob.literal()` a public function

---

_@orf_

Being able to detect if a given string is a glob expression or a literal string is very handy in some contexts. 

As on example, we have a list of default AWS regions that we allow the user to specify a glob expression to match. However, the user may want to include another custom string as a region.

Detecting if the user has passed a glob expression or a literal string is currently not possible with this crate, which is a real shame. If `literal()` was public then the implementation above would be as simple as:

```rust
let globs: &[Glob] = ...;
let glob_set = GlobSet::builder().add(...);
let literal_regions = globs.iter().filter_map(|g| g.literal());
let matched_regions = DEFAULT_REGIONS.iter().filter(|region| glob_set.is_match(region));

return literal_regions.chain(matched_regions).collect();
```

I think that exposing the full AST or any specific internals is a bad idea, but being able to tell if a glob is a literal string is non-invasive (and simple!) enough to warrant exposing?

---

_Closed by @orf on 2025-01-24 18:04_

---
