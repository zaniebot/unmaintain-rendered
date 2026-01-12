```yaml
number: 16115
title: mdtests are no longer coloured when run locally
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2025-02-12T11:36:16Z
updated_at: 2025-02-12T14:38:07Z
url: https://github.com/astral-sh/ruff/issues/16115
synced_at: 2026-01-12T15:54:55Z
```

# mdtests are no longer coloured when run locally

---

_@AlexWaygood_

### Description

There's a known issue that the output from mdtest is not coloured if you simply run `cargo test` (https://github.com/astral-sh/ty/issues/235). Until recently, however, you _would_ get pretty colours from the mdtest framework if you simply ran `cargo test -p red_knot_python_semantic` (which is what I usually do locally). I added the feature in https://github.com/astral-sh/ruff/pull/13725. It appears to have broken recently: whereas you used to get something like this when the tests failed:

<details>
<summary>Screenshot</summary>

![Image](https://github.com/user-attachments/assets/7751a060-c0d3-471b-b23f-2ee6cc5c7d8b)

</details>

You now get something like this, which is much harder to read:

<details>
<summary>Screenshot</summary>

![Image](https://github.com/user-attachments/assets/66dd1303-3adb-4b94-a498-c2b220398547)

</details>

I tried bisecting, and `git bisect` points to d47088c8f83f966f734c254a23073eeee123c347 as the first bad commit (from https://github.com/astral-sh/ruff/pull/15976). I find this surprising -- I can't see any obvious changes in that PR that would have had any impact on this? But indeed, if I checkout that commit, no test failures are colourised when I run `cargo test -p red_knot_python_semantic`; but if I checkout the previous commit, test failures are colourised as expected.

Cc. @BurntSushi 

---

_Label `bug` added by @AlexWaygood on 2025-02-12 11:36_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-12 11:36_

---

_Label `testing` added by @AlexWaygood on 2025-02-12 11:36_

---

_Comment by @MichaReiser on 2025-02-12 11:44_

I think the problem is that we globally disable `colored` when rendering diagnostics in the mdtest frameworks which is global 🤷 

---

_Comment by @AlexWaygood on 2025-02-12 11:47_

sure... but why did that change in d47088c8f83f966f734c254a23073eeee123c347? I don't see any changes to the `red_knot_test` crate or any `Cargo.toml` files in that commit

---

_Comment by @MichaReiser on 2025-02-12 11:49_

It introduced a `snapshot-diagnostics` test, which disables coloring for these tests, and all tests run after it. 

---

_Assigned to @BurntSushi by @BurntSushi on 2025-02-12 14:01_

---

_Comment by @BurntSushi on 2025-02-12 14:01_

I can take a look at this by making it so diagnostic display doesn't reach out to global mutable state.

---

_Closed by @BurntSushi on 2025-02-12 14:38_

---

_Closed by @BurntSushi on 2025-02-12 14:38_

---
