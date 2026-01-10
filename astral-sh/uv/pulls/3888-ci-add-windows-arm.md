```yaml
number: 3888
title: "ci: add Windows ARM"
type: pull_request
state: closed
author: henryiii
labels: []
assignees: []
draft: true
base: main
head: henryiii/ci/winarm
created_at: 2024-05-28T19:46:09Z
updated_at: 2024-05-28T20:58:16Z
url: https://github.com/astral-sh/uv/pull/3888
synced_at: 2026-01-10T14:32:20Z
```

# ci: add Windows ARM

---

_Pull request opened by @henryiii on 2024-05-28 19:46_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Trying out Windows ARM.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Going to download locally if it works.

<!-- How was it tested? -->


---

_Comment by @codspeed-hq[bot] on 2024-05-28 19:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/henryiii:henryiii/ci/winarm)

### Merging #3888 will **not alter performance**

<sub>Comparing <code>henryiii:henryiii/ci/winarm</code> (1cd4adf) with <code>main</code> (47db418)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @henryiii on 2024-05-28 20:06_

It looks like several dependencies don't support cross compiling.

---

_Closed by @henryiii on 2024-05-28 20:06_

---

_Comment by @charliermarsh on 2024-05-28 20:57_

`libz-ng-sys` we can remove. We disable that on some other platforms.

Ring we need...

---

_Comment by @charliermarsh on 2024-05-28 20:58_

It's supposedly supported: https://github.com/briansmith/ring/blob/main/BUILDING.md#windows-arm64

---
