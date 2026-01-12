```yaml
number: 628
title: Move check filtering to linter.rs
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/filter-rules
created_at: 2022-11-06T22:55:16Z
updated_at: 2022-12-11T15:23:13Z
url: https://github.com/astral-sh/ruff/pull/628
synced_at: 2026-01-12T05:36:31Z
```

# Move check filtering to linter.rs

---

_Pull request opened by @charliermarsh on 2022-11-06 22:55_

Right now, we rely on the various checkers _not_ adding disabled rules to the list of returned checks. This results in a lot of very rigorous validation against `settings.enabled` throughout the codebase. While this does guarantee that we avoid as much useless work as possible, I think it might be overkill, and makes for some very tedious code.

This PR instead adds a step to the core check path to filter out any disabled rules prior to returning. This is safer, in that we're enforcing rule enablement at one singular location, and it means that we can use enablement and disablement purely as a performance optimization (skip unnecessary work) and not as something that we need to track throughout the codebase.

The downside heres here are (1) it does create a situation in which we're not strictly required to skip unnecessary work (since we'll always filter out unneeded checks at the last minute), and (2) technically we're doing a little bit of extra work now in some minor cases.

Resolves #412 (though the approach isn't exactly as-described in that issue).


---

_Comment by @charliermarsh on 2022-11-07 02:20_

(Need to benchmark this.)

---

_Comment by @andersk on 2022-11-12 01:30_

I’m curious why you decided against filtering in `add_check`? It seems to me that would take the same amount of computation with less memory. Perhaps you’re concerned about increasing the code size of this `#[inline(always)]` function—but if we think its calls are rare enough that it’s okay for them to allocate extra memory, then they should also be rare enough that they aren’t worth inlining.

---

_Comment by @charliermarsh on 2022-11-12 01:41_

Primarily because there are other sources of checks apart from the AST `Checker`, and so we'd still have to guard against adding disabled checks in (e.g.) `check_lines.rs`.

---

_Comment by @andersk on 2022-11-12 01:44_

Somewhat tangentially, I bet replacing `enabled: BTreeSet<CheckCode>` with a flat array `enabled: [bool; 173]` (or some bit-set library) would speed up these checks.

---

_Comment by @charliermarsh on 2022-11-12 01:45_

Yeah I like that idea.

---

_Comment by @charliermarsh on 2022-12-11 15:23_

I'll come back to this at some point but this PR is dated and lingering.

---

_Closed by @charliermarsh on 2022-12-11 15:23_

---
