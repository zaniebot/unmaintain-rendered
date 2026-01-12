```yaml
number: 2433
title: Refactor tests
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-tests
created_at: 2023-02-01T07:36:01Z
updated_at: 2023-02-01T22:06:44Z
url: https://github.com/astral-sh/ruff/pull/2433
synced_at: 2026-01-12T15:55:08Z
```

# Refactor tests

---

_@not-my-profile_

_No description provided._

---

_Renamed from "refactor: Stop repeating `resources/test/` everywhere" to "Switch to `cargo test` and refactor" by @not-my-profile on 2023-02-01 08:04_

---

_@not-my-profile reviewed on 2023-02-01 08:16_

---

_Review comment by @not-my-profile on `src/test.rs`:23 on 2023-02-01 08:16_

While stumbling into this I added the now 1st commit of this PR since the GitHub CI previously with `cargo insta test` didn't print the diff.

---

_@not-my-profile reviewed on 2023-02-01 08:47_

---

_Review comment by @not-my-profile on `src/test.rs`:23 on 2023-02-01 08:47_

Nevermind, I found a better way, see the last commit.

---

_@charliermarsh reviewed on 2023-02-01 12:16_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:72 on 2023-02-01 12:16_

Is the downside here that we no longer flag on unreferenced snapshots? (This used to fail if you included an unused snapshot.)

---

_@charliermarsh reviewed on 2023-02-01 12:52_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:72 on 2023-02-01 12:52_

(To be clear, I actually agree with changing this since `cargo insta test` has non-ideal behavior. It'd be nice to have some solution for unreferenced snapshots, but worst-case we can just run this manually from time-to-time.)

---

_@not-my-profile reviewed on 2023-02-01 13:19_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:72 on 2023-02-01 13:19_

Yes that's indeed the downside. I think ideally there would be a `cargo insta delete-stray-snapshots` command ... I'll ask Armin about it.

---

_@charliermarsh reviewed on 2023-02-01 13:22_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:72 on 2023-02-01 13:22_

In the meantime, is your preference still to merge as-is?

---

_@not-my-profile reviewed on 2023-02-01 13:32_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:72 on 2023-02-01 13:32_

I have dropped the first commit implementing this switch to `cargo test` from this PR ... so we can come back to it once there is a solution without any drawbacks :)

---

_Renamed from "Switch to `cargo test` and refactor" to "Refactor tests" by @not-my-profile on 2023-02-01 13:32_

---

_Merged by @charliermarsh on 2023-02-01 14:17_

---

_Closed by @charliermarsh on 2023-02-01 14:17_

---

_@not-my-profile reviewed on 2023-02-01 14:57_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:72 on 2023-02-01 14:57_

Just filed https://github.com/mitsuhiko/insta/issues/344 :)

---

_@max-sixty reviewed on 2023-02-01 22:06_

---

_Review comment by @max-sixty on `.github/workflows/ci.yaml`:72 on 2023-02-01 22:06_

This was originally my suggestion so mea culpa. 

But `cargo insta test` definitely used to show diffs, and that is the intended behavior. And looks like it'll be back shortly....

Also FYI there is now `--unreferenced=auto` to avoid the subsequent `git diff`; e.g. https://github.com/PRQL/prql/blob/5de452377e13550d70d7dd5a4d8472a764d85475/.github/workflows/test-rust.yaml#L61

---
