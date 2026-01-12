```yaml
number: 566
title: Implement editable installs in dev command
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/implement-editable-installs
created_at: 2023-12-05T12:58:04Z
updated_at: 2023-12-12T14:45:58Z
url: https://github.com/astral-sh/uv/pull/566
synced_at: 2026-01-12T16:04:02Z
```

# Implement editable installs in dev command

---

_@konstin_

First step, sufficient to run
```shell
cargo run --bin puffin-dev -- build --editable -w target/editables/ scripts/editable-installs/poetry_editable/
```
and check the wheel to confirm its working. Tests will be added with the pip-sync integration.


---

_Comment by @konstin on 2023-12-05 12:58_

Current dependencies on/for this PR:
* main
  * **PR #564** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/564?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #565** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #566** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
        * **PR #567** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #587** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2023-12-12 14:33_

---

_Review requested from @charliermarsh by @konstin on 2023-12-12 14:33_

---

_Comment by @konstin on 2023-12-12 14:34_

Let's merge this before we get merge conflicts

---

_@charliermarsh approved on 2023-12-12 14:39_

---

_Merged by @konstin on 2023-12-12 14:45_

---

_Closed by @konstin on 2023-12-12 14:45_

---

_Branch deleted on 2023-12-12 14:45_

---
