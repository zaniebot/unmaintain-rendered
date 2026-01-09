---
number: 5981
title: "fix(complete): restore `nospace` in bash when reply ends in `=/:`"
type: pull_request
state: merged
author: mernen
labels: []
assignees: []
merged: true
base: master
head: fix-bash-clap-complete-space
created_at: 2025-04-27T21:40:52Z
updated_at: 2025-05-11T22:10:14Z
url: https://github.com/clap-rs/clap/pull/5981
synced_at: 2026-01-07T13:12:20-06:00
---

# fix(complete): restore `nospace` in bash when reply ends in `=/:`

---

_Pull request opened by @mernen on 2025-04-27 21:40_

This is a fix for #5980.

Tested with the same POC app, `app-name --path sr`&lt;TAB> now completes to `src/` with no trailing space, and &lt;TAB> again completes to `src/main.rs ` (with trailing space), as expected.

Also tested with [jj](https://github.com/jj-vcs/jj).

---

_@epage reviewed on 2025-04-28 16:55_

---

_Review comment by @epage on `clap_complete/tests/testsuite/bash.rs`:399 on 2025-04-28 16:55_

Is this testing the right thing?  The commit with the behavior change didn't change this test?

---

_@mernen reviewed on 2025-04-28 16:57_

---

_Review comment by @mernen on `clap_complete/tests/testsuite/bash.rs`:399 on 2025-04-28 16:57_

I actually included a *failing* test that then turns green. After reading your message more carefully, I was already in the process of replacing it with a passing test that demonstrates current behavior and then needs to be updated; I'll push again in a few minutes.

---
