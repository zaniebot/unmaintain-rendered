```yaml
number: 14797
title: "Add support for `HF_TOKEN`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/hf
created_at: 2025-07-21T19:38:02Z
updated_at: 2025-07-21T20:55:34Z
url: https://github.com/astral-sh/uv/pull/14797
synced_at: 2026-01-10T06:53:02Z
```

# Add support for `HF_TOKEN`

---

_Pull request opened by @charliermarsh on 2025-07-21 19:38_

## Summary

If `HF_TOKEN` is set, we'll automatically wire it up to authenticate requests when hitting private `huggingface.co` URLs in `uv run`.

## Test Plan

An unauthenticated request:

```
> cargo run -- run https://huggingface.co/datasets/cmarsh/test/resolve/main/main.py

  File "/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/mainYadr5M.py", line 1
    Invalid username or password.
            ^^^^^^^^
SyntaxError: invalid syntax
```

An authenticated request:

```
> HF_TOKEN=hf_... cargo run run https://huggingface.co/datasets/cmarsh/test/resolve/main/main.py

Hello from main.py!
```


---

_Marked ready for review by @charliermarsh on 2025-07-21 19:38_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-21 19:38_

---

_Review requested from @jtfmumm by @charliermarsh on 2025-07-21 19:38_

---

_Label `enhancement` added by @charliermarsh on 2025-07-21 19:38_

---

_@zanieb approved on 2025-07-21 19:42_

---

_@zanieb reviewed on 2025-07-21 19:50_

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:23 on 2025-07-21 19:50_

Should this filter empty strings?

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:41 on 2025-07-21 19:50_

Should we have a debug or trace log that we loaded this from `HF_TOKEN`?

---

_@zanieb reviewed on 2025-07-21 19:50_

---

_@zanieb reviewed on 2025-07-21 19:51_

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:27 on 2025-07-21 19:51_

We might want to do this after we've determined `HF_TOKEN` is populated (i.e., via `filter`) so we can include a debug log indicating we ignored `HF_TOKEN` because of `UV_NO_HF_TOKEN`.

---

_Merged by @charliermarsh on 2025-07-21 20:55_

---

_Closed by @charliermarsh on 2025-07-21 20:55_

---

_Branch deleted on 2025-07-21 20:55_

---
