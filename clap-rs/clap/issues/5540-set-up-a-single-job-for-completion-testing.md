```yaml
number: 5540
title: Set up a single job for completion testing
type: issue
state: closed
author: shannmu
labels: []
assignees: []
created_at: 2024-06-19T17:14:32Z
updated_at: 2024-07-05T20:44:49Z
url: https://github.com/clap-rs/clap/issues/5540
synced_at: 2026-01-12T16:14:17Z
```

# Set up a single job for completion testing

---

_@shannmu_

I found a issue when running the clap_complete tests locally (on the clap master branch) using the command:

```shell=
cargo test --features="unstable-dynamic"
```
The result was:

```shell=
---- expected: tests/testsuite/fish.rs:186:20
++++ actual:   In-memory
   1      - % exhaustive
        1 + % exhaustive
   2    2 | action                                                             last              -V         (Print version)
   3    3 | alias                                                              pacman            --generate      (generate)
   4    4 | complete            (Register shell completions for this program)  quote             --global      (everywhere)
   5    5 | help  (Print this message or the help of the given subcommand(s))  value             --help        (Print help)
   6    6 | hint                                                               -h  (Print help)  --version  (Print version)∅

Update with SNAPSHOTS=overwrite

note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    fish::complete_dynamic

test result: FAILED. 72 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 2.87s

```
There is a difference of a space between them. However, this test case error was not reported in the CI. The issue is may due to this line https://github.com/clap-rs/clap/blob/70e84174a72f766020f5f65f354e16f1e1a49bed/clap_complete/tests/testsuite/fish.rs#L146. So I think that we should make sure the job's environment includes bash, fish, elvish, and zsh shells to cover all test cases in CI.

---

_Referenced in [clap-rs/clap#5551](../../clap-rs/clap/pulls/5551.md) on 2024-06-29 10:45_

---

_Referenced in [clap-rs/clap#5567](../../clap-rs/clap/pulls/5567.md) on 2024-07-05 20:07_

---

_Closed by @epage on 2024-07-05 20:44_

---
