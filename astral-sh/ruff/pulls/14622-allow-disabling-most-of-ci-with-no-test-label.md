```yaml
number: 14622
title: "Allow disabling (most of) CI with `no-test` label"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
  - no-test
assignees: []
merged: true
base: main
head: dhruv/ci-no-tests
created_at: 2024-11-27T04:23:01Z
updated_at: 2025-01-22T08:29:16Z
url: https://github.com/astral-sh/ruff/pull/14622
synced_at: 2026-01-10T20:05:43Z
```

# Allow disabling (most of) CI with `no-test` label

---

_Pull request opened by @dhruvmanila on 2024-11-27 04:23_

Ref: https://github.com/astral-sh/uv/pull/9456

---

_Label `ci` added by @dhruvmanila on 2024-11-27 04:23_

---

_Label `no-test` added by @dhruvmanila on 2024-11-27 04:23_

---

_Comment by @MichaReiser on 2024-11-27 07:46_

I don't mind the change if other people find it useful, but I worry that it adds complexity to our CI jobs without being used much because it requires an extra label. I'm probably too lazy to add it, and not adding it has no actual cost to me. Would it make more sense to automatically apply the new behavior to draft PRs unless they have a specific label? Draft PRs already express the *I'm not done yet* without requiring an extra step from my side (and the `gh` CLI makes it very easy to publish in draft, publishing with a label is a bit more involved)

---

_Comment by @dhruvmanila on 2024-11-27 07:48_

> Would it make more sense to automatically apply the new behavior to draft PRs unless they have a specific label because draft PRs already express the: I'm not done here.

Yeah, I think I'm more in favor of checking the draft status of the PR instead of label. I've asked @zanieb for their opinion on this, will wait until then. I'm happy to hear others opinion as well.

---

_Comment by @AlexWaygood on 2024-11-27 11:36_

One of the reasons I sometimes open draft PRs is to e.g. check it passes all CI before requesting the review of codeowners. Most CI can be run locally, it's true, but for some CI jobs it's much easier to just see how it does on GitHub (e.g. the ecosystem checks). So I'm not sure I'd be a huge fan of auto-disabling all CI for draft PRs.

---

_Comment by @zanieb on 2025-01-21 21:41_

Just noticed the cross-link here. I feel pretty strongly that it doesn't make sense to toggle based on "draft" status — for the same reasons Alex said. This has required no maintenance since we added it in uv. I don't know if that changes anything here. We have more expensive CI over there.

---

_@MichaReiser approved on 2025-01-22 07:04_

I don't have a use case for this myself but if anyone else finds it useful, then I don't see why we shouldn't do it.

---

_Comment by @dhruvmanila on 2025-01-22 08:29_

I'll go ahead and merge this but happy to re-iterate or revert if this turns out to be not so useful.

---

_Merged by @dhruvmanila on 2025-01-22 08:29_

---

_Closed by @dhruvmanila on 2025-01-22 08:29_

---

_Branch deleted on 2025-01-22 08:29_

---
