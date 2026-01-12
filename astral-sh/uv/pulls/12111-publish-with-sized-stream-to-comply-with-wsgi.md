```yaml
number: 12111
title: publish with sized stream to comply with WSGI pypi server constraints.
type: pull_request
state: merged
author: jmrouet
labels: []
assignees: []
merged: true
base: main
head: publish-with-content-length-issue-11862
created_at: 2025-03-11T10:45:43Z
updated_at: 2025-03-11T14:54:31Z
url: https://github.com/astral-sh/uv/pull/12111
synced_at: 2026-01-12T16:10:08Z
```

# publish with sized stream to comply with WSGI pypi server constraints.

---

_@jmrouet_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is meant to fix issue #11862 

It allows to send sized bodies during `publish`

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

the PR was tested on the MRE from #11862 

<!-- How was it tested? -->


---

_@konstin approved on 2025-03-11 14:54_

Thanks!

---

_Merged by @konstin on 2025-03-11 14:54_

---

_Closed by @konstin on 2025-03-11 14:54_

---
