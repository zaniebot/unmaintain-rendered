```yaml
number: 16635
title: "Implement `pip install --report <file>`"
type: pull_request
state: open
author: andrewjcg
labels: []
assignees: []
base: main
head: report
created_at: 2025-11-07T16:01:59Z
updated_at: 2025-11-07T21:27:48Z
url: https://github.com/astral-sh/uv/pull/16635
synced_at: 2026-01-10T06:28:12Z
```

# Implement `pip install --report <file>`

---

_Pull request opened by @andrewjcg on 2025-11-07 16:01_

Support `pip`s `--report` flag, producing a JSON encoded file with metadata
on the packages to be installed.


---

_Comment by @zanieb on 2025-11-07 21:27_

Thanks for looking into this.

Can you provide some more context on the choices you've made here? What changes were made to uv internals to facilitate the feature and why? What differences are there from pip's output and interface, if any? Are there parts of pip's output format that are controversial or problematic? Are you planning to add tests?

---
