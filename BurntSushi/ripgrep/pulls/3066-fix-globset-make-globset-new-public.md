```yaml
number: 3066
title: "fix(globset): make GlobSet::new public"
type: pull_request
state: closed
author: bcheidemann
labels:
  - rollup
assignees: []
base: master
head: globset/make-constructor-public
created_at: 2025-06-12T15:11:32Z
updated_at: 2025-09-20T01:08:45Z
url: https://github.com/BurntSushi/ripgrep/pull/3066
synced_at: 2026-01-12T18:23:15Z
```

# fix(globset): make GlobSet::new public

---

_@bcheidemann_

Currently, `GlobSet::new` is private and can only be accessed via the `GlobSetBuilder`.

However, `GlobSetBuilder` simply creates a `Vec<Glob>` and then invokes `GlobSet::new`. For users of `globset` who already have a `Vec<Glob>` (or similar), this design requires them to iterate over their `Vec<Glob>`, calling `GlobSetBuilder::add` for each `Glob`, thus constructing a new `Vec<Glob>` internal to the `GlobSetBuilder`.

This makes the consuming code unnecessarily verbose to and may result in some performance impact (not tested).

---

_@bcheidemann reviewed on 2025-06-12 15:12_

---

_Review comment by @bcheidemann on `crates/globset/src/lib.rs`:415 on 2025-06-12 15:12_

Note: I've changed the pats type here to make it more flexible for consumers.

---

_Comment by @reneleonhardt on 2025-06-21 09:33_

Good idea!

Would also be nice if GlobSetBuilder would also support `extend(other: &GlobSet)` to chain multiple globsets together 😄
A combined GlobSet could match quickly, then the "inner" GlobSets could be matched sequentially to determine which logical set actually matched.

---

_@BurntSushi approved on 2025-08-20 00:13_

Thanks! I've incorporated this into my rollup branch.

I've also made the API a bit more robust. In particular, to mimic the [`regex::RegexSet::new`](https://docs.rs/regex/latest/regex/struct.RegexSet.html#method.new) constructor, it takes an iterator of `AsRef<Glob>`. This in turn requires an `impl AsRef<Glob> for Glob` to work ergonomically.

---

_Label `rollup` added by @BurntSushi on 2025-08-20 00:13_

---

_Comment by @BurntSushi on 2025-08-20 00:14_

> Good idea!
> 
> Would also be nice if GlobSetBuilder would also support `extend(other: &GlobSet)` to chain multiple globsets together 😄 A combined GlobSet could match quickly, then the "inner" GlobSets could be matched sequentially to determine which logical set actually matched.

This is an API footgun and changes the worst case time complexity of the matching routine. So... not going to happen.

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
