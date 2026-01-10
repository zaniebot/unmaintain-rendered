---
number: 5601
title: Support multiple values in native completions
type: pull_request
state: merged
author: shannmu
labels: []
assignees: []
merged: true
base: master
head: multi-values
created_at: 2024-07-26T13:05:47Z
updated_at: 2025-06-05T08:52:28Z
url: https://github.com/clap-rs/clap/pull/5601
synced_at: 2026-01-10T01:28:24Z
---

# Support multiple values in native completions

---

_Pull request opened by @shannmu on 2024-07-26 13:05_

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->
**Related issue** https://github.com/clap-rs/clap/issues/3921
### Work in this PR
- Support multi-values for both short flag and long flag
  What we can complete now?
  ```
  dynamic --flag bar1 [TAB]
  ```
  It will complete`bar1 bar2 bar3`.
  Also,
  ```
  dynamic -F bar1 [TAB]
  ```
  will complete`bar1 bar2 bar3`.
- Support multi-values for positional argument with `num_args(N)` which N is fixed.
  ```
  dynamic pos_a [TAB]
  ```
  will complete `pos_a pos_b pos_c`
  ```
  dynamic -- pos_a [TAB]
  ```
  will complete `pos_a pos_b pos_c`

### What may be is left
- Support multi-values for both short flag and long flag with format as follows
  ```
  dynamic --flag=bar1 [TAB]
  dynamic -F=bar1 [TAB]
  ```
- Support multi-values for positional argument with `last` or `trailing_var_arg`

---

_@epage reviewed on 2024-07-26 15:29_

---

_Review comment by @epage on `clap_complete/tests/testsuite/dynamic.rs`:457 on 2024-07-26 15:29_

Even in tests, lets use standard naming conventions.  CLI flags should be kebab-case rather than snake_case

---

_@epage reviewed on 2024-07-26 15:29_

---

_Review comment by @epage on `clap_complete/tests/testsuite/dynamic.rs`:524 on 2024-07-26 15:29_

Aren't these duplicated?

---

_@epage reviewed on 2024-07-26 15:30_

---

_Review comment by @epage on `clap_complete/tests/testsuite/dynamic.rs`:524 on 2024-07-26 15:30_

Did you mean `uncertain` for the second two?

---

_@epage reviewed on 2024-07-26 15:30_

---

_Review comment by @epage on `clap_complete/tests/testsuite/dynamic.rs`:492 on 2024-07-26 15:30_

Let's also test "one past the end" to make sure our counting is right

---

_@epage reviewed on 2024-07-26 15:33_

---

_Review comment by @epage on `clap_complete/tests/testsuite/dynamic.rs`:515 on 2024-07-26 15:33_

This case should be in the prior commit

---

_@epage reviewed on 2024-07-26 15:34_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:118 on 2024-07-26 15:34_

Unsure if errorring is the right thing to do...

---

_@epage reviewed on 2024-07-26 15:34_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:178 on 2024-07-26 15:34_

This looks familiar.  Is there a way for us to reuse this code?

---

_Review comment by @shannmu on `clap_complete/tests/testsuite/dynamic.rs`:524 on 2024-07-26 15:56_

Sorry for this. I found these duplicated tests and changed in the next feat commit. And I originally wanted to perform a rebase operation but i missed.

---

_@shannmu reviewed on 2024-07-26 15:56_

---

_@shannmu reviewed on 2024-07-26 15:56_

---

_Review comment by @shannmu on `clap_complete/tests/testsuite/dynamic.rs`:492 on 2024-07-26 15:56_

Okay.

---

_@shannmu reviewed on 2024-07-26 15:58_

---

_Review comment by @shannmu on `clap_complete/src/dynamic/completer.rs`:118 on 2024-07-26 15:58_

Logically, this branch will not be entered, but it must be matched. If there is a better way to implement it, please tell me.

---

_@epage reviewed on 2024-07-26 16:18_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:118 on 2024-07-26 16:18_

`unreachable!("reason it won't be hit")`

---

_@epage reviewed on 2024-07-26 18:59_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:103 on 2024-07-26 18:59_

> "reason it won't be hit"

You're supposed to fill in this part :)

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:87 on 2024-07-26 19:00_

Won't `ValueDone` put is back into parsing options and subcommands on the next iteration?

---

_@epage reviewed on 2024-07-26 19:00_

---

_@epage reviewed on 2024-07-26 19:01_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:656 on 2024-07-26 19:01_

Not sure whats "parsing" about this.  This is a `get_positional_max_values`.

---

_@epage reviewed on 2024-07-26 19:02_

---

_Review comment by @epage on `clap_complete/src/dynamic/completer.rs`:178 on 2024-07-26 19:02_

Was hoping we culd share more than just the lookup

---
