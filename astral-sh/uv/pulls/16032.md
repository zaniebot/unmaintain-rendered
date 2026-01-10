```yaml
number: 16032
title: "Add a `uv cache size` command"
type: pull_request
state: merged
author: yumeminami
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: feat/uv-cache-size-command
created_at: 2025-09-26T06:24:05Z
updated_at: 2025-11-02T20:44:28Z
url: https://github.com/astral-sh/uv/pull/16032
synced_at: 2026-01-10T06:28:12Z
```

# Add a `uv cache size` command

---

_Pull request opened by @yumeminami on 2025-09-26 06:24_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implement `uv cache size` to output the cache directory size in raw bytes by default, with a `--human` option for human-readable output.

close #15821

<!-- How was it tested? -->


---

_Renamed from "Feat/uv cache size command" to "Add a `uv cache size` command" by @konstin on 2025-09-26 07:24_

---

_Label `enhancement` added by @konstin on 2025-09-26 07:24_

---

_@charliermarsh reviewed on 2025-10-27 02:09_

I'm not strongly opposed but it does seem like `du -sh $(uv cache dir)` might be equally sufficient here? In the linked issue, the idea is raised that we might track the cache size within the cache (to make reporting faster). That seems quite difficult, but if we _aren't_ doing that, is there a good reason to support this over `du -sh`?

---

_Comment by @yumeminami on 2025-10-27 03:34_

@charliermarsh 

I think the main reasons is:

Cross-platform consistency: eliminating the need to call different command-line tools depending on the platform, `du` in Linux/macOS,  but Windows doesn't have `du`(use `Get-ChildItem` in Windows). I think use the same code to handle is more elegant and maintanble than call different command-line tools


---

_@charliermarsh approved on 2025-11-02 20:10_

---

_Label `preview` added by @charliermarsh on 2025-11-02 20:20_

---

_Merged by @charliermarsh on 2025-11-02 20:44_

---

_Closed by @charliermarsh on 2025-11-02 20:44_

---
