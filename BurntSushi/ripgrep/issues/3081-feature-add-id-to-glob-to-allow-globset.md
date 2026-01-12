```yaml
number: 3081
title: "Feature: Add `id` to Glob to allow GlobSet identifying/reporting which one matched"
type: issue
state: open
author: reneleonhardt
labels:
  - enhancement
assignees: []
created_at: 2025-07-01T06:53:59Z
updated_at: 2025-10-16T01:40:16Z
url: https://github.com/BurntSushi/ripgrep/issues/3081
synced_at: 2026-01-12T16:13:25Z
```

# Feature: Add `id` to Glob to allow GlobSet identifying/reporting which one matched

---

_@reneleonhardt_

#### Describe your feature request

`GlobSet` is very useful, but at the moment it can't report which specific Glob matched, it only reports true/false after iterating patterns.

Current function:
```rust
pub fn is_match_candidate(&self, path: &Candidate<'_>) -> bool {
```

New function:
```rust
pub fn get_match_id(&self, path: &Candidate<'_>) -> Option<String> {
```

```rust
pub struct Glob {
    id: String, // Identifier
    glob: String,
    re: String,
    opts: GlobOptions,
    tokens: Tokens,
}
```

This would allow to process even hundreds of Globs (maybe even in parallel with futures-concurrency) while still being able to identify which exact pattern matched and process a corresponding mapping accordingly (i.e. ".gitignore discarded Cargo.lock").


---

_Comment by @BurntSushi on 2025-10-16 01:40_

I'm open to this, but as mentioned in #3092, the IDs should be auto-assigned as monotonically increasing integers starting from `0`. Similar to how `regex-automata` and `aho-corasick` work. Attaching `String` values like suggested is bad juju and imposes extra costs that don't need to be paid for all such use cases.

---

_Label `enhancement` added by @BurntSushi on 2025-10-16 01:40_

---
