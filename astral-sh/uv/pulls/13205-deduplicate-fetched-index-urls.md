```yaml
number: 13205
title: Deduplicate fetched index URLs
type: pull_request
state: merged
author: christeefy
labels:
  - enhancement
assignees: []
merged: true
base: main
head: perf/dedupe-index-urls
created_at: 2025-04-29T22:07:41Z
updated_at: 2025-05-02T12:03:36Z
url: https://github.com/astral-sh/uv/pull/13205
synced_at: 2026-01-10T11:10:41Z
```

# Deduplicate fetched index URLs

---

_Pull request opened by @christeefy on 2025-04-29 22:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #11970.

## Test Plan

<!-- How was it tested? -->
Ran `cargo nextest` 


---

_Renamed from "Deduplicate index URLs" to "Deduplicate fetched index URLs" by @christeefy on 2025-04-29 22:08_

---

_Comment by @konstin on 2025-04-30 07:01_

The fix you added looks good. Could you add a test? E.g. using mockserver with a custom redirect that checks that we're only querying the index page once.

---

_Comment by @christeefy on 2025-04-30 15:08_

I've added a single test that redirects to `https://pypi.org/simple` in `pip_install.rs`. Technically tests can also be added for cases like, `uv add`, `uv sync` etc., but I'm not doing that to avoid bloating up code & test times. 

Let me know if this should be written as a single unit test instead.

---

_@konstin approved on 2025-05-02 08:28_

nice work on the test, thanks!

---

_Label `enhancement` added by @konstin on 2025-05-02 08:29_

---

_Merged by @konstin on 2025-05-02 08:29_

---

_Closed by @konstin on 2025-05-02 08:29_

---

_Branch deleted on 2025-05-02 12:03_

---
