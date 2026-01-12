```yaml
number: 789
title: "version string is malformed when using `cargo install`"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2018-02-12T12:18:55Z
updated_at: 2018-02-21T01:05:58Z
url: https://github.com/BurntSushi/ripgrep/issues/789
synced_at: 2026-01-12T16:13:22Z
```

# version string is malformed when using `cargo install`

---

_@BurntSushi_

When running `cargo install ripgrep`, the git hash in the version string is improperly formatted. In this case, we should just omit the revision identifier altogether.

Example:

```
ripgrep 0.8.0 (rev )
```

We should probably improve how we use `git` to get the hash. e.g., What happens if you compile ripgrep from its git source, but outside the git repo itself?

---

_Label `bug` added by @BurntSushi on 2018-02-12 12:18_

---

_Comment by @okdana on 2018-02-12 17:25_

FYI, the official Homebrew version of `rg` has this problem too, presumably because they build from the source tarball. I agree that just omitting the revision when it's not available would be nicer / less confusing.

---

_Closed by @BurntSushi on 2018-02-21 01:05_

---
