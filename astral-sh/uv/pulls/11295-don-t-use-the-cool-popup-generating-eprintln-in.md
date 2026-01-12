```yaml
number: 11295
title: "don't use the Cool popup-generating eprintln in trampoline for warnings"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/popup
created_at: 2025-02-06T20:11:31Z
updated_at: 2025-02-07T18:33:14Z
url: https://github.com/astral-sh/uv/pull/11295
synced_at: 2026-01-12T16:09:46Z
```

# don't use the Cool popup-generating eprintln in trampoline for warnings

---

_@Gankra_

Also I refactored the code a bit to centralize all the calls of eprintln.

Fixes #10706 

---

_Label `bug` added by @Gankra on 2025-02-06 20:11_

---

_Comment by @Gankra on 2025-02-06 20:17_

Oh right I need to like... regenerate these binaries...

---

_Converted to draft by @Gankra on 2025-02-06 20:54_

---

_Comment by @Gankra on 2025-02-06 20:54_

Drafting until binaries uploaded

---

_Marked ready for review by @Gankra on 2025-02-07 16:59_

---

_Comment by @Gankra on 2025-02-07 17:00_

@zanieb slated to upload binaries (not building on my mac right now)

---

_Comment by @Gankra on 2025-02-07 18:12_

Confirmed popups no more

---

_@zanieb reviewed on 2025-02-07 18:18_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:503 on 2025-02-07 18:18_

nit: would prefer like `exit_with_error`?

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:19_

Does this pop-up persist beyond the program exiting? Or block exit until it's ack'd?

---

_@zanieb reviewed on 2025-02-07 18:19_

---

_@zanieb approved on 2025-02-07 18:19_

---

_@zanieb reviewed on 2025-02-07 18:19_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:503 on 2025-02-07 18:19_

I guess probably not worth the regen

---

_@zanieb reviewed on 2025-02-07 18:19_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:503 on 2025-02-07 18:19_

(unless needed to test https://github.com/astral-sh/uv/pull/11295#discussion_r1946976564)

---

_@Gankra reviewed on 2025-02-07 18:20_

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:20_

popup blocks exit (or maybe startup? my example was too trivial to distinguish that) yeah.

---

_@Gankra reviewed on 2025-02-07 18:22_

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:22_

I guess in the error cases that's not even a real distinction (it's a meaningful distinction with warnings, but none of those popup anymore so not a distinction anymore :)

---

_@Gankra reviewed on 2025-02-07 18:23_

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:23_

genuinely unsure how to trigger an error popup here. as you said before most of them are "uv is broken, wtf" errors

---

_@zanieb reviewed on 2025-02-07 18:26_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:26_

I think you'd have to just like `if true` then throw the pop-up to see if it works as intended. Not a big deal, was just wondering if these would actually be visible as intended.

---

_@Gankra reviewed on 2025-02-07 18:32_

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:505 on 2025-02-07 18:32_

good point. confirmed it blocks the process from exiting until the popup is dismissed, so you see it.

---

_Merged by @Gankra on 2025-02-07 18:33_

---

_Closed by @Gankra on 2025-02-07 18:33_

---

_Branch deleted on 2025-02-07 18:33_

---
