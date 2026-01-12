```yaml
number: 355
title: Not able to build ripgrep on Ubuntu 16.04.2
type: issue
state: closed
author: ronakg
labels: []
assignees: []
created_at: 2017-02-11T05:16:38Z
updated_at: 2017-02-11T05:22:03Z
url: https://github.com/BurntSushi/ripgrep/issues/355
synced_at: 2026-01-12T16:13:21Z
```

# Not able to build ripgrep on Ubuntu 16.04.2

---

_@ronakg_

Getting following error when I try to build ripgrep on Ubuntu 16.04.2. I've cloned latest master.

```
+ (master) $ cargo build --release
failed to parse manifest at `/home/rogandhi/tools/ripgrep/Cargo.toml`

Caused by:
  could not parse input as TOML
Cargo.toml:45:9 expected a key but found an empty string
Cargo.toml:45:9-45:10 expected `.`, but found `'`
```

---

_Comment by @BurntSushi on 2017-02-11 05:22_

As the README says, you need Rust 1.12 or newer. I'd recommend using rustup.

---

_Closed by @BurntSushi on 2017-02-11 05:22_

---
