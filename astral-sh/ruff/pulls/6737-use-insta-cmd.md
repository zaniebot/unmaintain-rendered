```yaml
number: 6737
title: Use insta_cmd
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: insta-cmd
created_at: 2023-08-21T19:00:30Z
updated_at: 2023-09-05T12:28:51Z
url: https://github.com/astral-sh/ruff/pull/6737
synced_at: 2026-01-12T02:45:38Z
```

# Use insta_cmd

---

_Pull request opened by @konstin on 2023-08-21 19:00_

I've experimented with using [insta_cmd](https://github.com/mitsuhiko/insta-cmd) as a replacement for [assert_cmd](https://github.com/assert-rs/assert_cmd). I like it a lot, if we have consensus we want to do the switch i'll finish this PR. merging depends on the https://github.com/mitsuhiko/insta-cmd/issues/2 fix being shipped in a release.

---

_Label `internal` added by @konstin on 2023-08-21 19:00_

---

_Label `type-inference` added by @konstin on 2023-08-21 19:00_

---

_Label `type-inference` removed by @konstin on 2023-08-21 19:00_

---

_Comment by @github-actions[bot] on 2023-08-21 19:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-08-22 01:23_

I support this change.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/snapshots/ruff_cli__commands__run__test__E902_E902.py.snap`:4 on 2023-08-22 06:02_

Can we remove the ASCII codes before printing to text? 

---

_@MichaReiser approved on 2023-08-22 06:03_

---

_@konstin reviewed on 2023-08-22 08:13_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/snapshots/ruff_cli__commands__run__test__E902_E902.py.snap`:4 on 2023-08-22 08:13_

yes this needs to be fixed

---

_@konstin reviewed on 2023-08-22 09:41_

---

_Review comment by @konstin on `crates/ruff_cli/tests/black_compatibility_test.rs`:180 on 2023-08-22 09:41_



i don't think this worked before


---

_@MichaReiser reviewed on 2023-08-22 09:52_

---

_Review comment by @MichaReiser on `crates/ruff_cli/Cargo.toml`:74 on 2023-08-22 09:52_

Can you test that the colors still show when running `cargo run --bin ruff_cli` locally?

---

_@konstin reviewed on 2023-08-22 10:16_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:74 on 2023-08-22 10:16_

yes, `cargo run -p ruff_cli -- --no-cache .` shows colors

---

_Comment by @konstin on 2023-08-22 10:16_

This will remain a draft until we have a new insta_cmd release


---

_Closed by @konstin on 2023-08-22 10:16_

---

_Reopened by @konstin on 2023-08-22 10:16_

---

_Marked ready for review by @konstin on 2023-09-04 07:38_

---

_Comment by @konstin on 2023-09-04 07:54_

Style question: Do we want snapshot files (`stdin_success`) or inline snapshots (`stdin_error`) for `ruff_cli`? I find inline snapshots easier to review, but we don't have any other inline insta tests yet.

---

_Comment by @MichaReiser on 2023-09-04 07:57_

> Style question: Do we want snapshot files (`stdin_success`) or inline snapshots (`stdin_error`) for `ruff_cli`? I find inline snapshots easier to review, but we don't have any other inline insta tests yet.

I don't follow the question because I'm not familiar with the subject. Do you have an example?

---

_@MichaReiser reviewed on 2023-09-04 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_cli/tests/integration_test.rs`:50 on 2023-09-04 07:58_

Is this an inline snapshot? I'm a huge fan of inline snapshots if the content isn't massive because I then don't need to switch between test and expected output. 

---

_Comment by @konstin on 2023-09-04 08:56_

> I don't follow the question because I'm not familiar with the subject. Do you have an example?

There are two possible styles we could use in the ruff_cli tests:

a) Test with snapshot file

https://github.com/astral-sh/ruff/blob/095e94008bad50f6693ef82d5f7945a565a70436/crates/ruff_cli/tests/integration_test.rs#L33-L38

https://github.com/astral-sh/ruff/blob/095e94008bad50f6693ef82d5f7945a565a70436/crates/ruff_cli/tests/snapshots/integration_test__stdin_success.snap#L1-L16

b) Test with inline snapshot

https://github.com/astral-sh/ruff/blob/095e94008bad50f6693ef82d5f7945a565a70436/crates/ruff_cli/tests/integration_test.rs#L40-L54



---

_Comment by @MichaReiser on 2023-09-04 09:07_

Thanks! For me it depends on the size of the snapshot. I very much like inline snapshots for short assertions if they are easy to update (although I would have loved to have a more explicit API, like `assert_inline_cmd_snapshot` similar to Jest). But I would prefer external snapshots for long assertions (I don't know, like ~20 lines plus) because the file becomes massive. 

What's your preference and why?

---

_Comment by @konstin on 2023-09-04 09:44_

I prefer the inline snapshots because the external files make it hard to review what output belongs to what test and require a lot of context switches, plus the inline tests are more concise. I agree with the length issue; our CLI test should in most cases be short, with the exception of the json output and the rule explanation maybe which can remain snapshot file test.

---

_Comment by @MichaReiser on 2023-09-04 10:47_

> I prefer the inline snapshots because the external files make it hard to review what output belongs to what test and require a lot of context switches, plus the inline tests are more concise. I agree with the length issue; our CLI test should in most cases be short, with the exception of the json output and the rule explanation maybe which can remain snapshot file test.

I like that!

---

_Comment by @konstin on 2023-09-05 12:13_

merging with the current format, i can change the style in a follow up PR if required

---

_Merged by @konstin on 2023-09-05 12:21_

---

_Closed by @konstin on 2023-09-05 12:21_

---

_Branch deleted on 2023-09-05 12:21_

---
