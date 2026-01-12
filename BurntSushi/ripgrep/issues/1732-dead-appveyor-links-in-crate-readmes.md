```yaml
number: 1732
title: Dead AppVeyor links in crate Readmes
type: issue
state: closed
author: atouchet
labels: []
assignees: []
created_at: 2020-11-16T23:22:20Z
updated_at: 2020-11-17T00:10:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1732
synced_at: 2026-01-12T16:13:24Z
```

# Dead AppVeyor links in crate Readmes

---

_@atouchet_

#### Describe your bug.

The Readmes for various crates in this repo contain links to AppVeyor that no longer work.

Ex: https://github.com/BurntSushi/ripgrep/blob/master/crates/cli/README.md

links to https://ci.appveyor.com/project/BurntSushi/ripgrep which says "Project not found or access denied."

Should these be edited or removed?

---

_Closed by @BurntSushi on 2020-11-17 00:09_

---

_Comment by @BurntSushi on 2020-11-17 00:10_

I switched to GitHub Actions a while ago. It replaces both AppVeyor and Travis CI.

It should just link to the same CI pages as the main ripgrep README. I've updated them. Thanks for the report!

---
