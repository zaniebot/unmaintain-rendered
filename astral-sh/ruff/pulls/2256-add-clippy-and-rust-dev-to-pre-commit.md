```yaml
number: 2256
title: add clippy and rust_dev to pre-commit
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: pre-commit-clippy
created_at: 2023-01-27T14:44:51Z
updated_at: 2023-01-27T23:53:45Z
url: https://github.com/astral-sh/ruff/pull/2256
synced_at: 2026-01-12T15:55:07Z
```

# add clippy and rust_dev to pre-commit

---

_@sbrugman_

I presume the reasoning for not including clippy in `pre-commit` was that it passes all files. This can be turned off with `pass_filenames`, in which case it only runs once.

`cargo +nightly dev generate-all` is also added (when excluding `target` is does not give false positives).

(The overhead of these commands is not much when the build is there. People can always choose to run only certain hooks with `pre-commit run [hook] --all-files`)

---

_Renamed from "add clippy to pre-commit" to "add clippy and rust_dev to pre-commit" by @sbrugman on 2023-01-27 14:44_

---

_Comment by @charliermarsh on 2023-01-27 23:51_

Happy to add this if helpful though I admittedly don't use `pre-commit`!

---

_Merged by @charliermarsh on 2023-01-27 23:53_

---

_Closed by @charliermarsh on 2023-01-27 23:53_

---
