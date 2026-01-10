```yaml
number: 11593
title: "Display the built file name instead of the canonicalized name in `uv build`"
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/display-real-filename
created_at: 2025-02-18T11:04:55Z
updated_at: 2025-02-20T08:11:18Z
url: https://github.com/astral-sh/uv/pull/11593
synced_at: 2026-01-10T11:10:38Z
```

# Display the built file name instead of the canonicalized name in `uv build`

---

_Pull request opened by @jtfmumm on 2025-02-18 11:04_

We keep the raw filename (prior to parsing/normalization) on the `BuildMessage` so we can display it.

Closes #11103.

Closes #11635.


---

_Review comment by @konstin on `crates/uv/src/commands/build_frontend.rs`:1150 on 2025-02-18 11:45_

Do we still need the `filename` field now that we have the raw name?

---

_@konstin reviewed on 2025-02-18 11:45_

---

_@jtfmumm reviewed on 2025-02-18 12:56_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/build_frontend.rs`:1150 on 2025-02-18 12:56_

There are a couple places that grab the `.version()` and there's this line:
```
            let path = output_dir.join(sdist_build.filename().to_string());
```
I wasn't sure if it would be safe to replace it for that path.

---

_@charliermarsh reviewed on 2025-02-18 14:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:1150 on 2025-02-18 14:24_

I would expect that `let path = output_dir.join(sdist_build.filename().to_string` needs to use the raw filename since we're trying to read from that path. Can we check why that _isn't_ failing today?

---

_@jtfmumm reviewed on 2025-02-19 19:52_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/build_frontend.rs`:1150 on 2025-02-19 19:52_

I added a test to `build.rs` that fails because of this line. The test passes after updating to use the raw filename.

---

_@charliermarsh reviewed on 2025-02-19 22:34_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:1185 on 2025-02-19 22:34_

I'd suggest returning `&str` here? I think it's somewhat uncommon to return `&String` since it's strictly less flexible (you can always get an `&str` from an `&String`, but you can't go the other way around).

---

_@charliermarsh reviewed on 2025-02-19 22:35_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:1199 on 2025-02-19 22:35_

Nit: this seems like a potentially unnecessary assignment since they're so similar (but not blocking obviously, up to you).

---

_@charliermarsh approved on 2025-02-19 22:35_

---

_Comment by @charliermarsh on 2025-02-19 22:36_

I linked the other issue here too, to close it out on merge.

---

_@jtfmumm reviewed on 2025-02-20 07:54_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/build_frontend.rs`:1199 on 2025-02-20 07:54_

Yeah that was just an oversight

---

_Merged by @jtfmumm on 2025-02-20 08:11_

---

_Closed by @jtfmumm on 2025-02-20 08:11_

---

_Branch deleted on 2025-02-20 08:11_

---
