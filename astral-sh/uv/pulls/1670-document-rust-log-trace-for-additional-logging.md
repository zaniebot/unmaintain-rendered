```yaml
number: 1670
title: Document RUST_LOG=trace for additional logging verbosity
type: pull_request
state: merged
author: olivierlefloch
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-02-19T00:17:38Z
updated_at: 2024-02-19T21:43:59Z
url: https://github.com/astral-sh/uv/pull/1670
synced_at: 2026-01-10T15:33:24Z
```

# Document RUST_LOG=trace for additional logging verbosity

---

_Pull request opened by @olivierlefloch on 2024-02-19 00:17_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This improves Contributing documentation to specifically mention `trace` level logging can be obtained via `RUST_LOG=trace uv …` as mentioned here: https://github.com/astral-sh/uv/issues/1569#issuecomment-1951489407

## Test Plan

Compare the output of

```
uv pip install --verbose requests
```

and

```
RUST_LOG=trace uv pip install --verbose requests
```


---

_@zanieb approved on 2024-02-19 00:21_

Makes sense, probably better to have #1569 but until then... :)

Thank you!

---

_Label `documentation` added by @zanieb on 2024-02-19 00:21_

---

_Merged by @zanieb on 2024-02-19 00:21_

---

_Closed by @zanieb on 2024-02-19 00:21_

---

_Branch deleted on 2024-02-19 21:43_

---
