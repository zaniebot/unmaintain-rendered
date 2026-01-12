```yaml
number: 5707
title: "Allow downloading wheels for metadata with `--no-binary`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/no-binary-meta
created_at: 2024-08-01T19:28:51Z
updated_at: 2024-08-06T18:14:13Z
url: https://github.com/astral-sh/uv/pull/5707
synced_at: 2026-01-12T16:06:58Z
```

# Allow downloading wheels for metadata with `--no-binary`

---

_@charliermarsh_

## Summary

We allow the use of (e.g.) `.whl.metadata` files when `--no-binary` is enabled, so it makes sense that we'd also also allow wheels to be downloaded for metadata extraction. So now, we validate `--no-binary` at install time, rather than metadata-fetch time.

Closes https://github.com/astral-sh/uv/issues/5699.


---

_Label `bug` added by @charliermarsh on 2024-08-01 19:29_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/error.rs`:18 on 2024-08-01 19:29_

We do, however, need to be aggressive about rejecting _builds_ at metadata-fetch time.

---

_@charliermarsh reviewed on 2024-08-01 19:29_

---

_Marked ready for review by @charliermarsh on 2024-08-01 19:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-01 19:29_

---

_Review requested from @konstin by @charliermarsh on 2024-08-01 19:29_

---

_@charliermarsh reviewed on 2024-08-01 19:32_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/preparer.rs`:33 on 2024-08-01 19:32_

(Clippy yelling at me about variant size.)

---

_Comment by @charliermarsh on 2024-08-01 19:32_

I'll figure out how to test this if we're aligned on the behavior.

---

_Comment by @charliermarsh on 2024-08-01 19:39_

It appears that pip does _not_ use wheel metadata when `--no-binary` is provided, they build everything (even if they need to try multiple versions).

---

_Comment by @charliermarsh on 2024-08-02 01:41_

Maybe the ideal here is: we use wheel metadata if we can fetch the wheel without downloading it; otherwise, we download the source distribution?

---

_Comment by @zanieb on 2024-08-02 02:36_

Seems murky, but I did think that was the existing behavior.

---

_Comment by @charliermarsh on 2024-08-02 02:44_

Which behavior exactly? The behavior in the PR, or "we use wheel metadata if we can fetch the wheel without downloading it; otherwise, we download the source distribution"?

---

_@konstin approved on 2024-08-02 08:14_

No idea what the right behavior(s) for `--no-binary` are, but the code looks code.

---

_Comment by @zanieb on 2024-08-02 12:38_

> Which behavior exactly?

We only use locally available wheel metadata.

---

_Comment by @charliermarsh on 2024-08-02 16:59_

I think it would be fine to merge this, and then revisit whether we should use wheels at all here.

---

_Comment by @charliermarsh on 2024-08-06 14:10_

I'm going to merge and document this. We can consider changing the behavior holistically.

---

_Merged by @charliermarsh on 2024-08-06 18:14_

---

_Closed by @charliermarsh on 2024-08-06 18:14_

---

_Branch deleted on 2024-08-06 18:14_

---
