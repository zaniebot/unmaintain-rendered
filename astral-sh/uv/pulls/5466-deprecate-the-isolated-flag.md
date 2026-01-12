```yaml
number: 5466
title: "Deprecate the `--isolated` flag"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/deprecate-isolated
created_at: 2024-07-26T00:15:26Z
updated_at: 2024-07-30T22:40:39Z
url: https://github.com/astral-sh/uv/pull/5466
synced_at: 2026-01-12T16:06:50Z
```

# Deprecate the `--isolated` flag

---

_@charliermarsh_

## Summary

This PR deprecates the `--isolated` flag. The treatment varies across the APIs:

- For non-preview APIs, we warn but treat it as equivalent to `--no-config`.
- For preview APIs, we warn and ignore it, with two exceptions...
- For `tool run` and `run` specifically, we don't even warn, because we can't differentiate the command-specific `--isolated` from the global `--isolated`.

---

_Label `cli` added by @charliermarsh on 2024-07-26 00:15_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 00:15_

---

_Review requested from @konstin by @charliermarsh on 2024-07-26 00:15_

---

_Marked ready for review by @charliermarsh on 2024-07-26 00:15_

---

_Comment by @j178 on 2024-07-26 00:33_

Could you also update the `sync-python-releases.yml` workflow and the module commets in the `uv-python/*.py` scripts? Or do you think it would be better to handle this in a separate PR?

---

_Comment by @charliermarsh on 2024-07-26 00:36_

Yes, thanks for the reminder.

---

_@charliermarsh reviewed on 2024-07-26 00:56_

---

_Review comment by @charliermarsh on `scripts/release.sh`:21 on 2024-07-26 00:56_

This is probably not necessary?

---

_@charliermarsh reviewed on 2024-07-26 00:56_

---

_Review comment by @charliermarsh on `.github/workflows/sync-python-releases.yml`:24 on 2024-07-26 00:56_

A little tedious...

---

_@konstin reviewed on 2024-07-26 09:24_

---

_Review comment by @konstin on `.github/workflows/sync-python-releases.yml`:24 on 2024-07-26 09:24_

Doesn't inline metadata imply isolation? `uv run fetch-download-metadata.py` works for me

---

_@konstin reviewed on 2024-07-26 09:25_

---

_Review comment by @konstin on `crates/uv-python/fetch-download-metadata.py`:14 on 2024-07-26 09:25_

Same here, i think the isolation is implied

---

_@konstin approved on 2024-07-26 09:26_

---

_@charliermarsh reviewed on 2024-07-26 18:31_

---

_Review comment by @charliermarsh on `crates/uv-python/fetch-download-metadata.py`:14 on 2024-07-26 18:31_

Oh yeah, I forgot that we marked uv itself as "unmanaged".

---

_Comment by @zanieb on 2024-07-30 19:54_

Will review this next.

---

_Comment by @charliermarsh on 2024-07-30 19:54_

Oh I was just gonna merge -- go for it, np.

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:84 on 2024-07-30 19:58_

Don't you need "files, or **use** `--no-workspace`" for the comma to be correct?

---

_@zanieb reviewed on 2024-07-30 19:58_

---

_@zanieb reviewed on 2024-07-30 20:00_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:90 on 2024-07-30 20:00_

"will be ignored" is a little ambiguous as to if it applies to this invocation or in the future. "is deprecated and has no effect"?

---

_@zanieb approved on 2024-07-30 20:03_

Thought the review request was new for some reason :)

---

_Merged by @charliermarsh on 2024-07-30 22:40_

---

_Closed by @charliermarsh on 2024-07-30 22:40_

---

_Branch deleted on 2024-07-30 22:40_

---
