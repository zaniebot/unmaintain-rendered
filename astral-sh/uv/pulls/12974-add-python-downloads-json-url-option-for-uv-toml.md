```yaml
number: 12974
title: "Add `python-downloads-json-url` option for `uv.toml` to configure custom Python installations via JSON URL"
type: pull_request
state: merged
author: MeitarR
labels:
  - configuration
assignees: []
merged: true
base: main
head: config-python_downloads_json_url-from-file
created_at: 2025-04-18T20:57:45Z
updated_at: 2025-04-30T20:09:35Z
url: https://github.com/astral-sh/uv/pull/12974
synced_at: 2026-01-12T16:10:29Z
```

# Add `python-downloads-json-url` option for `uv.toml` to configure custom Python installations via JSON URL

---

_@MeitarR_

## Summary

Part of #12838. Allow users to configure `python-downloads-json-url` in `uv.toml` and not just from env.

I followed similar PR #8695, so same as there it's also available in the CLI (I think maybe it's better not to be configurable from the CLI, but since the mirror parameters are, I think it's better to do the same)


## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @MeitarR on 2025-04-18 21:30_

---

_Review comment by @Gankra on `crates/uv/src/settings.rs`:875 on 2025-04-28 19:08_

Weird formatting.

---

_@Gankra approved on 2025-04-28 19:09_

Geez this is some heroic config wiring!

---

_@Gankra reviewed on 2025-04-28 19:11_

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:528 on 2025-04-28 19:11_

Hmm it's a bit dubious that we globally cache this result, effectively negating the extra parameter every time but the first time. but. Probably fine?

---

_Review comment by @MeitarR on `crates/uv/src/settings.rs`:875 on 2025-04-30 16:49_

fixed

---

_@MeitarR reviewed on 2025-04-30 16:49_

---

_@MeitarR reviewed on 2025-04-30 16:56_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:528 on 2025-04-30 16:56_

I agree, added comment about it 91c736bc9

---

_Review requested from @Gankra by @MeitarR on 2025-04-30 19:12_

---

_@Gankra approved on 2025-04-30 19:51_

---

_Label `configuration` added by @Gankra on 2025-04-30 19:51_

---

_Merged by @Gankra on 2025-04-30 19:52_

---

_Closed by @Gankra on 2025-04-30 19:52_

---

_Branch deleted on 2025-04-30 20:09_

---
