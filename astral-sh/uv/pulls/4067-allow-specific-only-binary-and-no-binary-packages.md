```yaml
number: 4067
title: "Allow specific `--only-binary` and `--no-binary` packages to override `:all:`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/only-no-binary
created_at: 2024-06-05T19:55:28Z
updated_at: 2024-06-12T20:48:00Z
url: https://github.com/astral-sh/uv/pull/4067
synced_at: 2026-01-12T16:06:01Z
```

# Allow specific `--only-binary` and `--no-binary` packages to override `:all:`

---

_@zanieb_

Updates `--no-binary <package>` to take precedence over `--only-binary :all:` and `--only-binary <package>` to take precedence over `--no-binary :all:`.

~I'm not entirely sure about this behavior, e.g. maybe I provided `--only-binary :all:` later on the command line and really want it to override those earlier arguments of `--no-binary <package>` for safety. Right now we just fail to solve though since we can't satisfy the overlapping requests.~

Closes https://github.com/astral-sh/uv/issues/4063

---

_Label `enhancement` added by @zanieb on 2024-06-05 19:55_

---

_Comment by @zanieb on 2024-06-05 20:26_

In the future, we may want to combine `NoBuild` and `NoBinary` into a single type to consolidate this logic.

edit: See #4284 

---

_@zanieb reviewed on 2024-06-05 20:27_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1902 on 2024-06-05 20:27_

Going to open an issue for this one

---

_Review requested from @charliermarsh by @zanieb on 2024-06-05 20:31_

---

_Marked ready for review by @zanieb on 2024-06-06 12:42_

---

_Review requested from @konstin by @zanieb on 2024-06-10 14:30_

---

_@charliermarsh reviewed on 2024-06-10 19:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/flat_index.rs`:183 on 2024-06-10 19:41_

Could we instead create a struct that couples `NoBinary` and `NoBuild`, and lets us query for the `no_binary` or `no_build` status, abstracting away this repeated logic? It seems really easy to miss a site as-is.


---

_@zanieb reviewed on 2024-06-10 20:05_

---

_Review comment by @zanieb on `crates/uv-resolver/src/flat_index.rs`:183 on 2024-06-10 20:05_

Yeah we should totally do that as noted at https://github.com/astral-sh/uv/pull/4067#issuecomment-2150906635 but I'd rather do it in a follow-up.

I didn't want to invest in a refactor until we were on the same page about the behavior. I could do it here too if you think this makes sense.

---

_@charliermarsh reviewed on 2024-06-10 20:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/flat_index.rs`:183 on 2024-06-10 20:10_

I think the behavior seems ok. Do you want to do it in this PR or na?

---

_@zanieb reviewed on 2024-06-10 20:14_

---

_Review comment by @zanieb on `crates/uv-resolver/src/flat_index.rs`:183 on 2024-06-10 20:14_

I don't really mind doing it here or at least writing the follow-up before merge.

---

_Review comment by @konstin on `crates/uv-dispatch/src/lib.rs`:330 on 2024-06-11 09:22_

nit: Can move the part after the or to the front? I got confused since when match has closures

---

_@konstin approved on 2024-06-11 09:37_

I agree that merging the no binary and no build types would be a good idea

---

_@zanieb reviewed on 2024-06-12 18:29_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:330 on 2024-06-12 18:29_

Should be resolved in #4284 

---

_@zanieb reviewed on 2024-06-12 18:30_

---

_Review comment by @zanieb on `crates/uv-resolver/src/flat_index.rs`:183 on 2024-06-12 18:30_

See #4284 — opted for a separate pull request for review purposes.

---

_Merged by @zanieb on 2024-06-12 20:47_

---

_Closed by @zanieb on 2024-06-12 20:47_

---

_Branch deleted on 2024-06-12 20:47_

---
