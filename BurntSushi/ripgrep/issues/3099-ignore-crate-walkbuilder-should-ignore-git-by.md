```yaml
number: 3099
title: "`ignore` crate: `WalkBuilder` should ignore `.git` by default if `.git_ignore(true)` and `.hidden(false)`"
type: issue
state: closed
author: reneleonhardt
labels:
  - wontfix
assignees: []
created_at: 2025-07-09T11:05:11Z
updated_at: 2025-07-09T12:22:19Z
url: https://github.com/BurntSushi/ripgrep/issues/3099
synced_at: 2026-01-12T16:13:25Z
```

# `ignore` crate: `WalkBuilder` should ignore `.git` by default if `.git_ignore(true)` and `.hidden(false)`

---

_@reneleonhardt_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ignore are you using?

`ignore` 0.4.23

### How did you install ignore?

```shell
cargo add ignore
```

### What operating system are you using ignore on?

macOS 15.5

### Describe your bug.

#### `git` behavior
The `.git` folder is excluded / ignored by default, all operations are no-ops:
* `git add .git/info`
* `git log -- .git`

### What are the steps to reproduce the behavior?

#### Current behavior
```rust
let walk = WalkBuilder::new(".").hidden(false).build();
```

#### Manual workaround
```rust
let overrides = OverrideBuilder::new(".").add("!.git").unwrap().build().unwrap();
let walk = WalkBuilder::new(".").hidden(false).overrides(overrides).build();
```

### What is the actual behavior?

* `.git_ignore(true)` (by default) and `.hidden(false)` (not default) emit everything inside the `.git` folder

### What is the expected behavior?

* `.git_ignore(true)` and `.hidden(false)` do not emit the `.git` folder


---

_Comment by @BurntSushi on 2025-07-09 12:22_

I'm unclear as to why you're filing this as a bug, but it is intended behavior and it isn't going to change. `WalkBuilder::git_ignore` is for `.gitignore`. `WalkBuilder::hidden` is for hidden files and directories, of which `.git` is one. `.git` isn't treated as special and is ignored because it is hidden. If you want to treat it as special, then you're free to do so (as your work-around does, by adding a specific rule for it).

---

_Closed by @BurntSushi on 2025-07-09 12:22_

---

_Label `wontfix` added by @BurntSushi on 2025-07-09 12:22_

---
