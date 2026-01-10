```yaml
number: 7495
title: "Revert \"Prune unzipped source distributions in `uv cache prune --ci` (#7446)\""
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
base: main
head: ag/revert-pr7446
created_at: 2024-09-18T13:04:28Z
updated_at: 2024-09-18T13:47:32Z
url: https://github.com/astral-sh/uv/pull/7495
synced_at: 2026-01-10T12:53:48Z
```

# Revert "Prune unzipped source distributions in `uv cache prune --ci` (#7446)"

---

_Pull request opened by @BurntSushi on 2024-09-18 13:04_

This reverts commit 9f7d9da449cab4119d07da3ecd2e3b868e092afd from PR #7446.

This is meant to be available as a quick fix if we can't otherwise find
a quick fix to the problem reported in #7485.

Fixes #7485


---

_Converted to draft by @BurntSushi on 2024-09-18 13:08_

---

_Marked ready for review by @BurntSushi on 2024-09-18 13:16_

---

_@zanieb reviewed on 2024-09-18 13:22_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:743 on 2024-09-18 13:22_

Woo haha

---

_Comment by @BurntSushi on 2024-09-18 13:34_

I reproduced the problem by following https://github.com/astral-sh/uv/issues/7485#issuecomment-2357719016. And I can confirm that "borks" the cache. I then tried `uv` built from this PR, and the version bump works as expected. So if we need it, this PR should be good to go (once I get CI passing).

---

_Comment by @zanieb on 2024-09-18 13:37_

@BurntSushi can you test with #7498 too?

---

_Comment by @BurntSushi on 2024-09-18 13:46_

@zanieb Yup, #7498 resolves it too.

---

_Comment by @BurntSushi on 2024-09-18 13:47_

Closing in favor of #7498.

---

_Closed by @BurntSushi on 2024-09-18 13:47_

---
