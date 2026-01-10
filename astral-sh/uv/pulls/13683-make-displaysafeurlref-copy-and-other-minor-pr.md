```yaml
number: 13683
title: "Make `DisplaySafeUrlRef` Copy and other minor PR follow-ups"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: main
head: jtfm/display-safe-url-followup
created_at: 2025-05-27T14:54:32Z
updated_at: 2025-05-28T10:36:20Z
url: https://github.com/astral-sh/uv/pull/13683
synced_at: 2026-01-10T11:10:42Z
```

# Make `DisplaySafeUrlRef` Copy and other minor PR follow-ups

---

_Pull request opened by @jtfmumm on 2025-05-27 14:54_

This PR implements a few review follow-ups from #13560. In particular, it
* Makes `DisplaySafeUrlRef` implement `Copy` so that it can be passed by value.
* Updates `to_string_with_credentials` methods with `displayable_with_credentials`, returning an `impl Display` instead of `String` for greater flexibility.
* Updates the `DisplaySafeUrl` and `DisplaySafeUrlRef` `Debug` implementations to match the underlying `Url` `Debug` implementation (with the exception that credentials are masked).
* Replaces an unnecessary `DisplaySafeUrl::from(Url::from_file_path` with `DisplaySafeUrl::from_file_path`


---

_Review requested from @charliermarsh by @konstin on 2025-05-27 15:02_

---

_Label `enhancement` added by @jtfmumm on 2025-05-27 15:15_

---

_@charliermarsh approved on 2025-05-28 00:39_

Great, thank you!

---

_Merged by @jtfmumm on 2025-05-28 10:36_

---

_Closed by @jtfmumm on 2025-05-28 10:36_

---

_Branch deleted on 2025-05-28 10:36_

---
