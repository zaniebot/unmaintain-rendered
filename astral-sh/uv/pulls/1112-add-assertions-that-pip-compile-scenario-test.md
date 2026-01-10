```yaml
number: 1112
title: "Add assertions that `pip compile` scenario test matches expected outcome"
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
base: main
head: zb/assert-compile
created_at: 2024-01-25T23:56:11Z
updated_at: 2024-02-02T02:41:43Z
url: https://github.com/astral-sh/uv/pull/1112
synced_at: 2026-01-10T15:33:24Z
```

# Add assertions that `pip compile` scenario test matches expected outcome

---

_Pull request opened by @zanieb on 2024-01-25 23:56_

This was kind of a pain, but basically we could not make assertions about the `pip compile` results (beyond the snapshot) since the standard output is hidden inside `assert_cmd_snapshot`. We want to make sure we're actually conforming to the scenario's expectations, so we now have an extra assertion on whether resolution failed or succeeded as well as that it includes the given packages.

Closes https://github.com/astral-sh/puffin/issues/1030

I also tried piping standard output into a file then reading from that, but this ends up being a little simpler.

---

_@zanieb reviewed on 2024-01-25 23:58_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-25 23:58_

But like.. does this spawn the command a second time?

---

_Label `testing` added by @zanieb on 2024-01-26 00:00_

---

_@charliermarsh reviewed on 2024-01-26 01:57_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-26 01:57_

I think yes?

---

_@charliermarsh reviewed on 2024-01-26 01:58_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:251 on 2024-01-26 01:58_

Aren't these covered by the `success:` code in the snapshot though?

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-26 05:04_

Annoying, but I don't really see what else to do. The piped stdout was no good.

---

_@zanieb reviewed on 2024-01-26 05:04_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:251 on 2024-01-26 05:05_

Yes, but 

- Since we update these in bulk it is easy to miss that a snapshot changed to have a different expected outcome
- The expected outcome is encoded in the scenario, but isn't otherwise enforced in the test which means incorrect behavior can slip in
-  The "on success stdout contains expected package versions" assertions further ensure we are resolving as the scenario expects

---

_@zanieb reviewed on 2024-01-26 05:05_

---

_@charliermarsh reviewed on 2024-01-28 14:42_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-28 14:42_

I really think we should find another solution here. It strikes me as a bad tradeoff to run every test twice to assert something that's already in the snapshot. It's _already_ true that the snapshots need to be manually reviewed and aren't auto-generated, right?

Here's an alternative: instead of writing `@r###"<snapshot>"###` as the placeholder in the generation script, can you generate the correct _header_ in `update.py`? That way, it will part of the diff.

E.g., could the initial snapshot be:

`@r###"
success: true
exit_code: 0
---- stdout ----
<snapshot>
"###
`

or something?


---

_@zanieb reviewed on 2024-01-28 15:38_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-28 15:38_

> It strikes me as a bad tradeoff to run every test twice to assert something that's already in the snapshot. 

I agree generally, but:

- This is _only_ in the `pip_compile_scenarios` which only has a few tests
- The scenarios have a limited number of packages and are fast to resolve
- The cache is reused and it's very fast

> Can you generate the correct header in update.py? That way, it will part of the diff.

 Snapshots are currently automatically accepted because we always replace the previous snapshot with the `<snapshot>` placeholder. I'm not sure how we could meaningfully make it a part of the diff?

---

_@zanieb reviewed on 2024-01-28 15:41_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-28 15:41_

I see this as a short-term solution that we can replace with better assertion infrastructure as discussed at https://github.com/astral-sh/puffin/pull/1122#discussion_r1468584952

The goal here is to prevent regressions in the generated tests, which have already made it into `main` several times now. I do not like this approach either, but I cannot justify spending more time on the test infrastructure right now.

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-28 15:59_

> Snapshots are currently automatically accepted because we always replace the previous snapshot with the <snapshot> placeholder. I'm not sure how we could meaningfully make it a part of the diff?

Ah ok, I didn't realize we were running with `--accept`. I assumed the snapshots were manually accepted. If they were manually accepted, then what I'm suggesting would attempt to highlight any mismatches in the status vs. expectation for the operator.

> The cache is reused and it's very fast

I really don't mean to be annoying but for me this is actually another good argument for _not_ doing this. The two commands aren't actually doing the same thing, since the second is operating on cached state. If we introduced bugs around caching, these tests could erroneously pass!

Anyway, I'm not in favor of the change but I feel heard and I defer to you as the owner of the code. (To be clear: you should merge it if you aren't convinced by my concerns and feel it's the right tradeoff.)


---

_@charliermarsh reviewed on 2024-01-28 15:59_

---

_@zanieb reviewed on 2024-01-28 16:20_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:110 on 2024-01-28 16:20_

I'm mostly willing to take a subpar approach because I've already merged incorrect snapshots, but let's wait and see if Konsti takes on implementing a better approach for Windows snapshotting.

---

_Closed by @zanieb on 2024-02-02 02:41_

---
