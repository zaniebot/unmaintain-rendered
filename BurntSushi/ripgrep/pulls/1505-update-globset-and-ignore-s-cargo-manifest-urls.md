```yaml
number: 1505
title: "update globset and ignore's cargo manifest urls"
type: pull_request
state: merged
author: chippers
labels: []
assignees: []
merged: true
base: master
head: update_subcrate_urls
created_at: 2020-02-29T00:55:19Z
updated_at: 2020-02-29T01:31:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1505
synced_at: 2026-01-12T18:23:14Z
```

# update globset and ignore's cargo manifest urls

---

_@chippers_

The homepage and repo urls for `ignore` and `globset` manifest keys weren't updated when the project structure changed, leading links from https://docs.rs and https://crates.io to 404s.

Sorry about the PR on the new globset repo, I happened to find it first before this crates directory.

---

_@BurntSushi approved on 2020-02-29 01:07_

Thanks! While you're here, would you might updating the links for the rest of the crates in the `crates` directory? I missed this when I did a re-org.

---

_Comment by @chippers on 2020-02-29 01:13_

These two were the only ones that were not pointing to ripgrep root, I'll update the other ones from the root to their directory

---

_Comment by @BurntSushi on 2020-02-29 01:14_

Ah that would be great, thanks!

---

_@BurntSushi approved on 2020-02-29 01:21_

Lovely, thank you!

---

_Merged by @BurntSushi on 2020-02-29 01:31_

---

_Closed by @BurntSushi on 2020-02-29 01:31_

---
