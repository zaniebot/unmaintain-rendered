```yaml
number: 14536
title: Conditionalize version_extras test on the pypi feature
type: pull_request
state: merged
author: musicinmybrain
labels:
  - testing
assignees: []
merged: true
base: main
head: version_extras-pypi
created_at: 2025-07-10T11:10:37Z
updated_at: 2025-07-10T18:19:38Z
url: https://github.com/astral-sh/uv/pull/14536
synced_at: 2026-01-10T06:53:02Z
```

# Conditionalize version_extras test on the pypi feature

---

_Pull request opened by @musicinmybrain on 2025-07-10 11:10_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `version_extras` test added in 85c0fc963b1d4f6c8130c4b4446d15b8f6ac8ac4 needs to connect to PyPI. This PR conditionalizes it on the `pypi` extra so that people running the tests offline don’t have to skip that test explicitly.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
I already ran `cargo test` in the git checkout to confirm I didn’t somehow introduce a syntax error. I am also applying this PR as a patch to [the `uv` package in Fedora](https://src.fedoraproject.org/rpms/uv), which runs tests offline with the `pypi` feature disabled.


---

_@zanieb approved on 2025-07-10 18:19_

---

_Merged by @zanieb on 2025-07-10 18:19_

---

_Closed by @zanieb on 2025-07-10 18:19_

---

_Label `testing` added by @zanieb on 2025-07-10 18:19_

---
