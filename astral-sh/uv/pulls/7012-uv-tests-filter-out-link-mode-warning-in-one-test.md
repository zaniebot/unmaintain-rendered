```yaml
number: 7012
title: "uv/tests: filter out link mode warning in one test"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/squash-hardlink-warning
created_at: 2024-09-04T12:44:48Z
updated_at: 2024-09-04T14:45:41Z
url: https://github.com/astral-sh/uv/pull/7012
synced_at: 2026-01-10T12:53:38Z
```

# uv/tests: filter out link mode warning in one test

---

_Pull request opened by @BurntSushi on 2024-09-04 12:44_

In the `lock_redact_https` test specifically, it prompts a link mode
warning from `uv` on my system. Debugging seems to suggest it is
provoked by attempting to hardlink between `/tmp` and `~/.local`. Since
these are on different file systems for me (with `/tmp` being a
ramdisk), it provokes the warning, and this turn spoils the snapshot
when running tests locally.

This PR adds a test specific filter rule to fix this.


---

_@zanieb reviewed on 2024-09-04 13:36_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:5840 on 2024-09-04 13:36_

That's weird. What are we placing in `/tmp`? We should be avoiding most use of `/tmp` in tests now. I don't see anything particular about this test.

---

_Comment by @BurntSushi on 2024-09-04 14:07_

If I set `RUST_LOG=debug` for the offending command and disable all filters, then these are the relevant lines I think:

```
        258 │+DEBUG Failed to hardlink `/home/andrew/.local/share/uv/tests/.tmpYOH182/temp/.venv/lib/python3.12/site-packages/iniconfig/__init__.py` to `/tmp/.tmp0UsfkF/archive-v0/A3TQjfa4sxc0QPCeXeRsz/iniconfig/__init__.py`, attempting to copy files as a fallback
        259 │+warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
        260 │+         If the cache and target directories are on different filesystems, hardlinking may not be supported.
        261 │+         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
        262 │+DEBUG Extracting file name=PackageName("foo")
        263 │+DEBUG Extracted 9 files name=PackageName("iniconfig")
        264 │+DEBUG Failed to hardlink `/home/andrew/.local/share/uv/tests/.tmpYOH182/temp/.venv/lib/python3.12/site-packages/__editable___foo_0_1_0_finder.py` to `/tmp/.tmp0UsfkF/archive-v0/HsLf3qhPZUp3lFYZabmDn/__editable___foo_0_1_0_finder.py`, attempting to copy files as a fallback
```

---

_Comment by @zanieb on 2024-09-04 14:18_

Ah, it's because we set `--no-cache` so we don't use the test context cache location. Awkward. Hm. cc @charliermarsh / @fasterthanlime — I guess we'll need to add a way to configure the `--no-cache` temporary directory location during testing?

---

_@zanieb approved on 2024-09-04 14:20_

Happy to unblock you here — but we should fix the root cause. Can you update the comment to reflect my findings?

---

_Merged by @BurntSushi on 2024-09-04 14:45_

---

_Closed by @BurntSushi on 2024-09-04 14:45_

---

_Branch deleted on 2024-09-04 14:45_

---
