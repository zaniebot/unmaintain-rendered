```yaml
number: 235
title: "mdtest output in CI isn't colourised"
type: issue
state: open
author: AlexWaygood
labels:
  - testing
  - internal
assignees: []
created_at: 2024-10-26T18:56:12Z
updated_at: 2025-11-18T16:10:24Z
url: https://github.com/astral-sh/ty/issues/235
synced_at: 2026-01-10T01:58:59Z
```

# mdtest output in CI isn't colourised

---

_Issue opened by @AlexWaygood on 2024-10-26 18:56_

If you run `cargo test -p red_knot_python_semantic` locally and an mdtest fails, you get nice pretty colours that make the test output much more readable, eg.:

![image](https://github.com/user-attachments/assets/079398d6-93e9-4957-8b1a-c39b2f1ab7e7)

But if you run `cargo test` (rather than `cargo test -p red_knot_python_semantic`) or `cargo nextest` (which is what we do in CI), the output isn't colourised, which makes the output much harder to read: https://github.com/astral-sh/ruff/actions/runs/11530541010/job/32100248614?pr=13918

This is because some of the other crates enable the `"no_color"` feature of the `colored` dependency, which is how we add colours to the output of mdtest: https://github.com/astral-sh/ruff/blob/b6ffa51c164e3898862c565ec860bbca928ddee9/crates/ruff_linter/Cargo.toml#L77-L78

When you run all the tests together, that feature ends up being enabled, meaning no mdtest colours. It would be great if we could see if there was a way of having coloured output for mdtest in CI without breaking the tests for those other crates.



---

_Label `help wanted` added by @AlexWaygood on 2024-10-26 18:56_

---

_Label `testing` added by @AlexWaygood on 2024-10-26 18:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-29 00:53_

---

_Label `help wanted` removed by @MichaReiser on 2024-11-23 09:17_

---

_Unassigned @charliermarsh by @AlexWaygood on 2025-02-05 11:53_

---

_Label `testing` added by @MichaReiser on 2025-05-07 15:22_

---

_Renamed from "[red-knot] mdtest output in CI isn't colourised" to "mdtest output in CI isn't colourised" by @MichaReiser on 2025-05-07 15:27_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:47_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 15:47_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
