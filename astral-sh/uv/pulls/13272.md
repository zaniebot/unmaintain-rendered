```yaml
number: 13272
title: Skip packages with mismatched names in Simple API
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/skip
created_at: 2025-05-02T18:35:08Z
updated_at: 2025-08-26T12:48:15Z
url: https://github.com/astral-sh/uv/pull/13272
synced_at: 2026-01-10T06:44:32Z
```

# Skip packages with mismatched names in Simple API

---

_Pull request opened by @charliermarsh on 2025-05-02 18:35_

## Summary

Given:

```
> cargo run pip install paddlepaddle-gpu==3.0.0 --index https://www.paddlepaddle.org.cn/packages/stable/cu126/ -v
...
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
WARN Skipping file with mismatched package name: `paddlepaddle` vs. `paddlepaddle-gpu`
...
```

---

_Review requested from @zanieb by @charliermarsh on 2025-05-02 18:35_

---

_Label `bug` added by @charliermarsh on 2025-05-02 18:35_

---

_Marked ready for review by @charliermarsh on 2025-05-02 18:35_

---

_Comment by @konstin on 2025-05-02 20:04_

This "broke" `bogus_redirect`, as this check happen earlier than the catch-all that raises the snapshotted error.

---

_Comment by @charliermarsh on 2025-05-03 20:25_

Yeah, hmm, this kind of regresses the error message.

---

_Comment by @zanieb on 2025-05-28 18:40_

@konstin can you take this over?

---

_Assigned to @konstin by @zanieb on 2025-05-28 18:40_

---

_Comment by @charliermarsh on 2025-05-28 18:46_

I don't really know what to do here. We'd need to create a custom incompatibility somehow.

---

_Comment by @konstin on 2025-06-04 08:27_

The paddlepaddle index fixed its page, I'd probably keep the current behavior of erroring instead of skipping the version to ensure the index misconfiguration is surfaced, unless we get more reports where this happens?

---

_Comment by @konstin on 2025-08-26 10:21_

This has become less pressing with the paddlepaddle index fixed, but I've finished it to save the next user hitting such an index the trouble.

---
