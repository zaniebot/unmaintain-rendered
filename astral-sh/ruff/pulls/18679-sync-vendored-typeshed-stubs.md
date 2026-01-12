```yaml
number: 18679
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-06-15T00:35:27Z
updated_at: 2025-06-17T16:06:18Z
url: https://github.com/astral-sh/ruff/pull/18679
synced_at: 2026-01-12T15:56:23Z
```

# Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2025-06-15 00:35_

---

_Review requested from @carljm by @github-actions[bot] on 2025-06-15 00:35_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-06-15 00:35_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-06-15 00:35_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-06-15 00:35_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-06-15 00:35_

---

_Closed by @AlexWaygood on 2025-06-15 08:38_

---

_Reopened by @AlexWaygood on 2025-06-15 08:38_

---

_Label `internal` removed by @AlexWaygood on 2025-06-15 08:40_

---

_Label `ty` added by @AlexWaygood on 2025-06-15 08:40_

---

_Comment by @github-actions[bot] on 2025-06-15 08:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-06-15 08:42_

We should rework the github workflow so it no longer adds the "internal" label to these PRs. They lead to behaviour changes for ty users; we should include mentions of them in ty release notes.

---

_Merged by @AlexWaygood on 2025-06-15 09:20_

---

_Closed by @AlexWaygood on 2025-06-15 09:20_

---

_Branch deleted on 2025-06-15 09:20_

---

_Comment by @dhruvmanila on 2025-06-17 07:37_

> We should rework the github workflow so it no longer adds the "internal" label to these PRs. They lead to behaviour changes for ty users; we should include mentions of them in ty release notes.

Can you say how should we highlight a change like this in the release notes? I'm not sure how useful it is to say that the typeshed version was bumped from commit A to B.

---

_Comment by @AlexWaygood on 2025-06-17 10:18_

> Can you say how should we highlight a change like this in the release notes?

It's a good question! Ideally I guess we'd list all the stdlib functions and classes that were changed, added or removed from the stubs, but I don't think there's an easy way to obtain that information. We could maybe link to a GitHub diff between the typeshed commit featured in our previous release and the typeshed commit in the new release -- that might be possible to automate?

Pyright generally just [says](https://github.com/microsoft/pyright/releases/tag/1.1.402) "Updated vendored typeshed stubs to the latest version" in its release notes. I agree that that isn't terribly informative for users. But they do at least get a link to the commit that shows the stubs being updated, which will allow them to check whether a change to the stdlib stubs that they care about was included in the latest pyright release. There are often users who find themselves unable to upgrade to the latest version of pyright or mypy due to regressions in typeshed, so I do think there are often users who will be grateful to know about typeshed changes!

---

_Comment by @dhruvmanila on 2025-06-17 16:02_

> We could maybe link to a GitHub diff between the typeshed commit featured in our previous release and the typeshed commit in the new release -- that might be possible to automate?

Yeah, I like this! We can start with this. I'm not sure about automation but we could update the release instructions to include this.

---

_Comment by @AlexWaygood on 2025-06-17 16:06_

And we always update [this file](https://github.com/astral-sh/ruff/blob/main/crates/ty_vendored/vendor/typeshed/source_commit.txt) when syncing vendored typeshed stubs, so it shouldn't be too hard to get a GitHub diff between the typeshed commit that was present in the previous release and the typeshed commit in the new release

---
