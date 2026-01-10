---
number: 6131
title: "fix(complete): do not suggest options after escape (`--`)"
type: pull_request
state: merged
author: mernen
labels: []
assignees: []
merged: true
base: master
head: do-not-suggest-opts-after-escape
created_at: 2025-09-13T23:04:47Z
updated_at: 2025-09-16T01:39:25Z
url: https://github.com/clap-rs/clap/pull/6131
synced_at: 2026-01-10T01:28:28Z
---

# fix(complete): do not suggest options after escape (`--`)

---

_Pull request opened by @mernen on 2025-09-13 23:04_

Fixes #6130 for dynamic completions.

---

_@epage reviewed on 2025-09-15 20:38_

---

_Review comment by @epage on `clap_complete/tests/testsuite/bash.rs`:504 on 2025-09-15 20:38_

As there is nothing bash-specific about this issue, could you move this test over to https://github.com/clap-rs/clap/blob/master/clap_complete/tests/testsuite/engine.rs?

That will
- Avoid me asking you to duplicate this test to all other shells :)
- Be faster to test by not needing to spawn `bash` and then wait for it to be done

---

_@mernen reviewed on 2025-09-15 21:30_

---

_Review comment by @mernen on `clap_complete/tests/testsuite/bash.rs`:504 on 2025-09-15 21:30_

…oh. Embarrassingly, I hadn't even realized my change affected an existing test. I've removed the bash-specific test, and replaced it with only a test for subcommands then, since the existing `suggest_delimiter_values` nicely illustrates positional arguments.

---
