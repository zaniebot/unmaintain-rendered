```yaml
number: 2983
title: "Add `add_gitignore` mthod to `WalkBuilder`"
type: issue
state: closed
author: shulaoda
labels: []
assignees: []
created_at: 2025-02-03T07:51:37Z
updated_at: 2025-09-23T01:29:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2983
synced_at: 2026-01-12T16:13:25Z
```

# Add `add_gitignore` mthod to `WalkBuilder`

---

_@shulaoda_

#### Describe your feature request

Related to #2964 

Currently, `OverrideBuilder` does not behave as expected with respect to the reverse functionality of `GitignoreBuilder`. Therefore, I propose adding an `add_gitignore` method to `WalkBuilder` that would allow users to directly pass a `Gitignore` object instead of needing to provide a path that converts a `.gitignore` file into a `Gitignore` object.

This method could function similarly to `add_ignore`, but instead of reading the `.gitignore` file from a `path`, it would accept a `Gitignore` object directly.

```rust
pub fn add_gitignore(&mut self, gi: Gitignore)  {
    self.ig_builder.add_ignore(gi);
}
```

---

_Comment by @BurntSushi on 2025-09-23 01:29_

I'm worried about adding this because it will likely have subtle semantics. You mention `OverrideBuilder`, but an analog to `WalkBuilder::add_ignore` would have difference precedence than `WalkBuilder::overrides`. Plus, this new method would need to be mutually exclusive with `WalkBuilder::add_ignore`. Finally, it's unclear to me why `OverrideBuilder` isn't working for your use case.

---

_Closed by @BurntSushi on 2025-09-23 01:29_

---
