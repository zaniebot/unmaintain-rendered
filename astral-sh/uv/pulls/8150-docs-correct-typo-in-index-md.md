```yaml
number: 8150
title: "docs: correct typo in `index.md`"
type: pull_request
state: merged
author: pantheraleo-7
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-12T19:34:57Z
updated_at: 2024-10-13T01:13:38Z
url: https://github.com/astral-sh/uv/pull/8150
synced_at: 2026-01-12T16:08:11Z
```

# docs: correct typo in `index.md`

---

_@pantheraleo-7_

This was added in [#5426](https://github.com/astral-sh/uv/pull/5426)

The output is such that a python shell was started, but the command will only print out the version because of the `--version` flag.

PS: I think the section `Download Python versions as needed` should be reverted back to `Download Python versions on demand` or something similar because that's what the command will do. It will download the given version if not already installed.

---

_Label `documentation` added by @charliermarsh on 2024-10-12 21:00_

---

_Merged by @charliermarsh on 2024-10-12 21:00_

---

_Closed by @charliermarsh on 2024-10-12 21:00_

---
