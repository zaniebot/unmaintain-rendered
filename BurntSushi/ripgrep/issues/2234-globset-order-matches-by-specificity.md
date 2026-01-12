```yaml
number: 2234
title: "globset: order matches by specificity "
type: issue
state: closed
author: smallstepman
labels:
  - wontfix
assignees: []
created_at: 2022-06-13T09:12:31Z
updated_at: 2022-06-13T12:09:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2234
synced_at: 2026-01-12T16:13:24Z
```

# globset: order matches by specificity 

---

_@smallstepman_

I'm building a `GlobSet` object and using it to match against the file path, however, I'd like to be able to get the results ordered from least to most specific match (aka [worst/best match](https://www.perlmonks.org/?node_id=911956)). 
### Example 
```rust
use globset::{Glob, GlobSetBuilder};

let mut builder = GlobSetBuilder::new();
builder.add(Glob::new("src/**/foo.rs")?);
builder.add(Glob::new("*")?);
builder.add(Glob::new("*.rs")?);
builder.add(Glob::new("src/bar/baz/foo.rs")?);
let set = builder.build()?;

// current behaviour
assert_eq!(set.matches("src/bar/baz/foo.rs"), vec![0, 1, 2, 3]);
// desired behaviour
assert_eq!(set.matches_order_by_specificity("src/bar/baz/foo.rs"), vec![1, 2, 0, 3]);
```

---

_Comment by @BurntSushi on 2022-06-13 12:09_

I don't think I'll accept this feature request. I don't know if any obvious definition for the "specificity" of a pattern. And even if I did, the implementation would just wind up reordering the globs. Which can be confusing if the caller isn't expecting it. So to implement this, I'd suggest coming up with your own definition and sorting the globs based on it before handing them to this crate.

---

_Closed by @BurntSushi on 2022-06-13 12:09_

---

_Label `wontfix` added by @BurntSushi on 2022-06-13 12:09_

---
