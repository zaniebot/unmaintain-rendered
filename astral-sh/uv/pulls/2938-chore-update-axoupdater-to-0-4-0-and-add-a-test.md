```yaml
number: 2938
title: "chore: update axoupdater to 0.4.0 and add a test"
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: updateit
created_at: 2024-04-09T17:08:32Z
updated_at: 2024-04-10T03:41:17Z
url: https://github.com/astral-sh/uv/pull/2938
synced_at: 2026-01-10T14:43:31Z
```

# chore: update axoupdater to 0.4.0 and add a test

---

_Pull request opened by @Gankra on 2024-04-09 17:08_

## Summary

This updates to the version of axoupdater used in cargo-dist 0.13.0's own selfupdate command, with all relevant fixes for platforms. It also tentatively introduces a mildly dangerous self-runtest that runs `uv self update` and checks that the binary is installed and executable.

I *believe* some adjustments need to be made to your CI to have this new test run, because it requires the `self-update` feature to be enabled, and I didn't want to just start messing with how you do feature coverage in your CI. **As a result I haven't yet had a chance to actually fully run this in CI**, though I've locally tested it on windows (with the guard disabled).


## Test Plan

Most of the machinery here is provided by axoupdater itself (cargo-dist also includes a variant of these tests in its codebase). This initial implementation has a couple major limitations: 

* This is For Reals modifying the system that runs the test (so it's off unless it detects it's running in CI, and if you want variations on this test they'll need to be [run in serial](https://github.com/axodotdev/cargo-dist/blob/5e7826f7b09a64e5ba9edc16dab8cee7b13daa27/cargo-dist/tests/cli-tests.rs#L235)). Since many of the testing issues were surrounding precise details of Actual Deployed Executions, this seemed worth the tradeoff.
* The actual installer *script* it's ultimately invoking is the one you last published, and *not* the one that cargo-dist will make when you next publish.

We're already working on implementing some logic for "get cargo-dist to generate a fresh installer script too", which is in fact the basis of a huge amount of cargo-dist's own testsuite. Now that we're dogfooding this stuff, it should be quite hard for this stuff to break without cargo-dist's own codebase noticing it first.


<!-- How was it tested? -->


---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-10 03:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 03:35_

---

_@charliermarsh approved on 2024-04-10 03:41_

Cool, this looks good to me. I'll merge as-is and see if I can add to CI in a follow-up PR. Thanks!

---

_Merged by @charliermarsh on 2024-04-10 03:41_

---

_Closed by @charliermarsh on 2024-04-10 03:41_

---
