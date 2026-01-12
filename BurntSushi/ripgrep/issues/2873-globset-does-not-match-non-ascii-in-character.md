```yaml
number: 2873
title: "`globset` does not match non-ascii in character class"
type: issue
state: open
author: Dav1dde
labels: []
assignees: []
created_at: 2024-08-14T14:30:26Z
updated_at: 2024-08-14T14:48:20Z
url: https://github.com/BurntSushi/ripgrep/issues/2873
synced_at: 2026-01-12T16:13:25Z
```

# `globset` does not match non-ascii in character class

---

_@Dav1dde_

The following code fails the assertion, where I would expect it to succeed.

```rs
assert!(globset::Glob::new("[ඞ]").unwrap().compile_matcher().is_match("ඞ"));
```

The problem appears to be the escaping here: https://github.com/BurntSushi/ripgrep/blob/af8c386d5e10eaa4b8cb687c4f5b05893a7ae0ce/crates/globset/src/glob.rs#L701

I can PR this and remove the escaping if you think this is the correct fix. Although some escaping would still be necessary, for example for the pattern `[\\]`, the `\` still needs to be escaped.

---
