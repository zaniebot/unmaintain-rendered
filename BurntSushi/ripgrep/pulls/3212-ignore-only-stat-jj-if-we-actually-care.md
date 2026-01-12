```yaml
number: 3212
title: "ignore: only stat .jj if we actually care"
type: pull_request
state: merged
author: ianloic
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2025-10-30T17:09:33Z
updated_at: 2025-10-30T17:30:26Z
url: https://github.com/BurntSushi/ripgrep/pull/3212
synced_at: 2026-01-12T18:23:15Z
```

# ignore: only stat .jj if we actually care

---

_@ianloic_

I was comparing the work being done by fd and find and noticed (with strace -f -c -S calls) that fd was doing a ton of failed statx calls. Upon closer inspection it was stating .jj even though I was passing --no-ignore. Eventually I turned up this check in Ignore::add_child_path that was doing stat on .jj regardless of whether the options request it.

With this patch it'll only stat .jj if that's relevant to the query.

---

_@BurntSushi approved on 2025-10-30 17:15_

Oh nice find!

---

_Merged by @BurntSushi on 2025-10-30 17:29_

---

_Closed by @BurntSushi on 2025-10-30 17:29_

---

_Comment by @BurntSushi on 2025-10-30 17:30_

This PR is on crates.io in `ignore 0.4.25`.

---
