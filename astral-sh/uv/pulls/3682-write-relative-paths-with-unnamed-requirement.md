```yaml
number: 3682
title: Write relative paths with unnamed requirement syntax
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2024-05-21T00:09:38Z
updated_at: 2025-04-22T05:33:46Z
url: https://github.com/astral-sh/uv/pull/3682
synced_at: 2026-01-10T11:10:33Z
```

# Write relative paths with unnamed requirement syntax

---

_Pull request opened by @charliermarsh on 2024-05-21 00:09_

## Summary

This PR falls back to writing an unnamed requirement if it appears to be a relative URL. pip is way more flexible when providing an unnamed requirement than when providing a PEP 508 requirement. For example, _only_ this works:

```
black @ file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable
```

Any other form will fail.

Meanwhile, _all_ of these work:

```
file:///Users/crmarsh/workspace/uv/scripts/packages/black_editable
scripts/packages/black_editable
./scripts/packages/black_editable
file:./scripts/packages/black_editable
file:scripts/packages/black_editable
```

Closes https://github.com/astral-sh/uv/issues/3180.


---

_Label `compatibility` added by @charliermarsh on 2024-05-21 00:09_

---

_@zanieb approved on 2024-05-21 00:26_

It makes me a little sad but yeah this is the pragmatic choice

---

_Comment by @charliermarsh on 2024-05-21 00:28_

Me too.

---

_Comment by @charliermarsh on 2024-05-21 00:39_

Windows failures... no...

---

_Merged by @charliermarsh on 2024-05-21 01:22_

---

_Closed by @charliermarsh on 2024-05-21 01:22_

---

_Branch deleted on 2024-05-21 01:22_

---

_Comment by @delfick on 2025-04-22 05:33_

hello, can a flag be introduced so that uv can output with the name?

When using the compiled .txt as a constraints file it can't find the name of the editable package when it has the `#egg=` in it (i.e. `-e file:some-package#egg=some-package` doesn't work with `error: Unnamed requirements are not allowed as constraints`) but if I change it to `some-package @ file:some-package` then it does work.

---
