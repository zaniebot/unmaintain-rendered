```yaml
number: 2137
title: Treat empty index URL strings as null instead of erroring
type: pull_request
state: merged
author: yasufumy
labels:
  - compatibility
assignees: []
merged: true
base: main
head: fix/index-url-parser
created_at: 2024-03-03T09:21:50Z
updated_at: 2024-03-04T12:09:20Z
url: https://github.com/astral-sh/uv/pull/2137
synced_at: 2026-01-12T16:04:53Z
```

# Treat empty index URL strings as null instead of erroring

---

_@yasufumy_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolve  #2129

I changed the behavior to parse an empty string for `--index-url` to be the same as the default.


---

_Marked ready for review by @yasufumy on 2024-03-03 09:32_

---

_@charliermarsh reviewed on 2024-03-03 15:00_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:231 on 2024-03-03 15:00_

I think empty should probably map to `None` rather than PyPI. The difference is subtle but if a user has an `--index-url` in a `requirements.txt`, and they specify `UV_INDEX_URL=`, we should probably respect the index URL in the `requirements.txt` rather than overriding to PyPI (which will be accomplished by mapping this to `None`).

---

_@charliermarsh reviewed on 2024-03-03 18:49_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:231 on 2024-03-03 18:49_

Playing around with this myself and Clap doesn't make it easy.

---

_Label `compatibility` added by @charliermarsh on 2024-03-03 19:50_

---

_Merged by @charliermarsh on 2024-03-03 19:57_

---

_Closed by @charliermarsh on 2024-03-03 19:57_

---

_Branch deleted on 2024-03-04 00:53_

---

_Renamed from "Make an empty string behave like the default" to "Treat empty index URL strings as null instead of erroring" by @zanieb on 2024-03-04 12:09_

---
