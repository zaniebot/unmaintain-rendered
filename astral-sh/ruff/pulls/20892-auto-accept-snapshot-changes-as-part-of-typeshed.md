```yaml
number: 20892
title: Auto-accept snapshot changes as part of typeshed-sync PRs
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/sync-typeshed-snapshots
created_at: 2025-10-15T13:51:40Z
updated_at: 2025-11-15T14:26:56Z
url: https://github.com/astral-sh/ruff/pull/20892
synced_at: 2026-01-10T16:53:55Z
```

# Auto-accept snapshot changes as part of typeshed-sync PRs

---

_Pull request opened by @AlexWaygood on 2025-10-15 13:51_

## Summary

Some of our ty snapshots (in particular, several in the `ty_ide` crate that relate to autocompletions) are very sensitive to changes in typeshed. If typeshed changes the line number a class or function is defined on, that often leads to the snapshots in `ty_ide` needing to be updated manually before a typeshed-sync PR can be merged, which is a bit of a pain.

This PR adds a step to the `sync_typeshed.yaml` workflow that checks for changes to snapshots and automatically accepts them all before the PR is made. This should hopefully reduce the number of changes we need to make manually to sync-typeshed PRs. (We'll still need to review these PRs carefully to check that all the changes to the snapshots are _desirable_, but I still think that this will probably save us time overall.) The step is allowed to fail; if checking for changes to snapshots produces a nonzero exit code, the sync-typeshed PR should still be filed.

The `cargo insta` docs are here: https://insta.rs/docs/cli/

## Test Plan

I:
1. Ran `git checkout dc64c086336148c57db14060c86f448351710b2e -- crates/ty_python_semantic/resources/mdtest/snapshots`. This reverted all the snapshot changes from https://github.com/astral-sh/ruff/pull/20820, a PR which resulted in lots of snapshot changes -- multiple snapshot changes per mdtest file, in some cases.
2. Ran `cargo insta test --accept`. I observed that this command reverted all changes from step (1).

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-15 13:51_

---

_Label `ci` added by @AlexWaygood on 2025-10-15 13:51_

---

_Label `ty` added by @AlexWaygood on 2025-10-15 13:51_

---

_Closed by @AlexWaygood on 2025-10-15 14:18_

---

_Reopened by @AlexWaygood on 2025-10-15 14:18_

---

_Comment by @AlexWaygood on 2025-10-15 14:34_

I don't know why the Ruff ecosystem job is repeatedly failing on this PR (I tried rerunning it), but it seems unrelated.

---

_@sharkdp approved on 2025-10-15 14:47_

Awesome idea.

---

_Comment by @AlexWaygood on 2025-10-15 14:57_

I kicked off a test run here using this branch: https://github.com/astral-sh/ruff/actions/runs/18533177734

---

_Comment by @AlexWaygood on 2025-10-15 16:37_

> I kicked off a test run here using this branch: [astral-sh/ruff/actions/runs/18533177734](https://github.com/astral-sh/ruff/actions/runs/18533177734)

It took a few iterations but I think it all looks good now:
- Workflow run: https://github.com/astral-sh/ruff/actions/runs/18535425113
- PR: https://github.com/astral-sh/ruff/pull/20899

---

_Merged by @AlexWaygood on 2025-10-15 16:37_

---

_Closed by @AlexWaygood on 2025-10-15 16:37_

---

_Branch deleted on 2025-10-15 16:37_

---

_Comment by @AlexWaygood on 2025-11-15 14:26_

It doesn't look this worked, but I don't really understand why :/

The typeshed sync workflow last night ran the full test suite, and I can see snapshot tests failing as part of the run. But no snapshot changes were accepted afterwards; it just says

> error: test run failed

https://github.com/astral-sh/ruff/actions/runs/19381406943/job/55460969374

I've tried running the same command locally with the same environment variables set, and it always results in snapshots being updated at the end of the run. Not sure what's up here.

---
