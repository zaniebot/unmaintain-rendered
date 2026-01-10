```yaml
number: 13462
title: "Reinstall local packages in `uv add`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: reinstall-packages-uv-add
created_at: 2025-05-15T10:03:15Z
updated_at: 2025-05-15T14:08:54Z
url: https://github.com/astral-sh/uv/pull/13462
synced_at: 2026-01-10T11:10:41Z
```

# Reinstall local packages in `uv add`

---

_Pull request opened by @blueraft on 2025-05-15 10:03_

## Summary

Closes https://github.com/astral-sh/uv/issues/13388

## Test Plan

`cargo test`


---

_Renamed from "lqReinstall local packages in `uv add`" to "Reinstall local packages in `uv add`" by @blueraft on 2025-05-15 10:03_

---

_Review requested from @charliermarsh by @konstin on 2025-05-15 11:46_

---

_Label `enhancement` added by @konstin on 2025-05-15 11:46_

---

_@zanieb reviewed on 2025-05-15 12:55_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1868 on 2025-05-15 12:55_

I think I'd prefer if we didn't make `args` mutable for this, does it seem reasonable to extract the `reinstall` option and just mutate / extend that?

---

_@zanieb reviewed on 2025-05-15 12:56_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1849 on 2025-05-15 12:56_

Should this happen for remote source trees too? Like a Git repository?

---

_@zanieb reviewed on 2025-05-15 13:00_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1868 on 2025-05-15 13:00_

@konstin kindly pointed out to me this is an existing pattern (https://github.com/astral-sh/uv/blame/26e37f3a1ed2701c76a5e366639724779928b37c/crates/uv/src/lib.rs#L678-L695) — feel free to leave as-is.

---

_@blueraft reviewed on 2025-05-15 13:29_

---

_Review comment by @blueraft on `crates/uv/src/lib.rs`:1849 on 2025-05-15 13:29_

We have the following line in https://docs.astral.sh/uv/concepts/cache/#dependency-caching

```
As a special case, uv will always rebuild and reinstall any local directory dependencies passed explicitly on the command-line (e.g., uv pip install .).
```

So I guess this shouldn't happen for remote source trees? 

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1849 on 2025-05-15 13:40_

That seems okay to me, at least as a starting point — let's say "local source trees" here then

---

_@zanieb reviewed on 2025-05-15 13:40_

---

_@zanieb approved on 2025-05-15 13:40_

---

_@charliermarsh reviewed on 2025-05-15 13:57_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:1849 on 2025-05-15 13:57_

👍 Let's mirror what we do elsewhere for now.

---

_Comment by @zanieb on 2025-05-15 14:01_

Thank you!

---

_Merged by @zanieb on 2025-05-15 14:08_

---

_Closed by @zanieb on 2025-05-15 14:08_

---
