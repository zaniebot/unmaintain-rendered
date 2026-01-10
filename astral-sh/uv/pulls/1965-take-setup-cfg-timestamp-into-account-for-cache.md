```yaml
number: 1965
title: "Take `setup.cfg` timestamp into account for cache invalidation"
type: pull_request
state: closed
author: HorlogeSkynet
labels: []
assignees: []
base: main
head: fix/setup-cfg_timestamp
created_at: 2024-02-25T12:32:40Z
updated_at: 2024-02-25T12:35:02Z
url: https://github.com/astral-sh/uv/pull/1965
synced_at: 2026-01-10T14:54:43Z
```

# Take `setup.cfg` timestamp into account for cache invalidation

---

_Pull request opened by @HorlogeSkynet on 2024-02-25 12:32_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This patch adds `setup.cfg` timestamp metadata to the list of files to process for cache invalidation.

Partially related to #1913.

---

Thanks bye :wave:


---

_Comment by @HorlogeSkynet on 2024-02-25 12:35_

So it appears this has been addressed in the meantime through f449bd41fbe90bf0f120c5129600daea841f7c29.

---

_Closed by @HorlogeSkynet on 2024-02-25 12:35_

---
