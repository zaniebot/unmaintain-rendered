```yaml
number: 17982
title: "[ty] Display \"All checks passed!\" message in green"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/chirpy-all-good
created_at: 2025-05-09T13:24:53Z
updated_at: 2025-05-10T22:09:10Z
url: https://github.com/astral-sh/ruff/pull/17982
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Display "All checks passed!" message in green

---

_@AlexWaygood_

## Summary

Currently if there are errors in your code, you'll see lots of red on the terminal, but if everything's good then you only get black and white feedback. It's kind of a downer!

This makes ty more "enthusiastic" about things if there were no errors in your code. Previously:

![image](https://github.com/user-attachments/assets/3441d014-af05-4b0a-b701-1a01e0695ac5)

With this PR:

![image](https://github.com/user-attachments/assets/28fc37c1-688f-45b0-8f30-34720ed110a2)

## Test Plan

See screenshots above


---

_Review requested from @carljm by @AlexWaygood on 2025-05-09 13:24_

---

_Label `ty` added by @AlexWaygood on 2025-05-09 13:24_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-09 13:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-09 13:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-09 13:24_

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-09 13:27_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-09 13:27_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-09 13:27_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-05-09 13:27_

---

_Comment by @github-actions[bot] on 2025-05-09 13:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
janus (https://github.com/aio-libs/janus)
- All checks passed!
+ All checks passed!

xarray-dataclasses (https://github.com/astropenguin/xarray-dataclasses)
- All checks passed!
+ All checks passed!

alectryon (https://github.com/cpitclaudel/alectryon)
- All checks passed!
+ All checks passed!

```
</details>


---

_@BurntSushi approved on 2025-05-09 13:28_

Love it!

---

_Merged by @AlexWaygood on 2025-05-09 13:29_

---

_Closed by @AlexWaygood on 2025-05-09 13:29_

---

_Branch deleted on 2025-05-09 13:29_

---

_@MichaReiser reviewed on 2025-05-09 13:30_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:264 on 2025-05-09 13:30_

We should not use colors if `settings.terminal.color` is no

---

_@AlexWaygood reviewed on 2025-05-10 17:54_

---

_Review comment by @AlexWaygood on `crates/ty/src/lib.rs`:264 on 2025-05-10 17:54_

I don't think we have a `settings.terminal.color` configuration field. Is that yet to be added?

The message is still correctly printed in black and white if I set the `NO_COLOR` environment variable

---

_@MichaReiser reviewed on 2025-05-10 22:09_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:264 on 2025-05-10 22:09_

Hmm, I thought there's one but it hasn't been added yet

---
