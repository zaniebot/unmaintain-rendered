```yaml
number: 16446
title: "Deterministically order `--find-links` distributions"
type: pull_request
state: merged
author: terror
labels:
  - bug
assignees: []
merged: true
base: main
head: sort-flat-index-dists
created_at: 2025-10-25T02:41:48Z
updated_at: 2025-10-27T01:53:40Z
url: https://github.com/astral-sh/uv/pull/16446
synced_at: 2026-01-10T06:36:16Z
```

# Deterministically order `--find-links` distributions

---

_Pull request opened by @terror on 2025-10-25 02:41_

Made to address this comment: https://github.com/astral-sh/uv/pull/16103#discussion_r2437498249

This PR sorts the distributions collected by `FlatIndexClient::read_from_directory` (used for `--find-links`) so results are ordered deterministically by filename and index.

---

_@chatgpt-codex-connector[bot] reviewed on 2025-10-25 02:45_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/uv-client/src/flat_index.rs`:328 on 2025-10-25 02:45_

**<sub><sub>![P1 Badge](https://img.shields.io/badge/P1-orange?style=flat)</sub></sub>  Sort uses variant order instead of filename order**

The sort is performed on `DistFilename`, whose derived `Ord` compares the enum variants (`SourceDistFilename` before `WheelFilename`) before looking at the contents. When a `--find-links` directory contains both a wheel and a source distribution for the same release, the sorted output will deterministically place the sdist first (e.g., `alpha-1.0.0.tar.gz` before `alpha-1.0.0-py3-none-any.whl`). The new test expects alphabetical ordering by filename string and will fail. If the goal is to sort by filename, the comparison needs to use the raw filename (`entry.filename.to_string()` or similar) rather than `DistFilename`’s variant order.

Useful? React with 👍 / 👎.

---

_@charliermarsh approved on 2025-10-27 01:53_

---

_Merged by @charliermarsh on 2025-10-27 01:53_

---

_Closed by @charliermarsh on 2025-10-27 01:53_

---

_Label `bug` added by @charliermarsh on 2025-10-27 01:53_

---

_Comment by @charliermarsh on 2025-10-27 01:53_

Nice, thank you!

---
