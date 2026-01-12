```yaml
number: 6519
title: "doc: add stdin to guide"
type: pull_request
state: closed
author: billy-doyle
labels: []
assignees: []
base: charlie/stdin
head: billy/additional-stdin-docs
created_at: 2024-08-23T14:12:29Z
updated_at: 2024-08-23T16:04:03Z
url: https://github.com/astral-sh/uv/pull/6519
synced_at: 2026-01-12T16:07:24Z
```

# doc: add stdin to guide

---

_@billy-doyle_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Document in guide stdin usage

alllll the easter eggs can do as well, but declined to keep consistent with the other examples 😆 

Additions to https://github.com/astral-sh/uv/pull/6481

```bash
$ uv run - <<EOF               
import antigravity
EOF
```

## Test Plan

<!-- How was it tested? -->


---

_Closed by @charliermarsh on 2024-08-23 15:49_

---

_Comment by @charliermarsh on 2024-08-23 15:57_

Oh sorry -- this got closed because my branch auto-deleted. Do you mind opening a new PR?

---

_Comment by @billy-doyle on 2024-08-23 16:04_

done

---
