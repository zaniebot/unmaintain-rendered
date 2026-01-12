```yaml
number: 21942
title: "Fix playground Share button showing \"Copied!\" before clipboard copy completes"
type: pull_request
state: merged
author: mahiro72
labels:
  - playground
assignees: []
merged: true
base: main
head: feature/fix-playground-sharebutton-copied
created_at: 2025-12-12T11:07:02Z
updated_at: 2025-12-17T11:16:02Z
url: https://github.com/astral-sh/ruff/pull/21942
synced_at: 2026-01-12T15:57:37Z
```

# Fix playground Share button showing "Copied!" before clipboard copy completes

---

_@mahiro72_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix the playground Share button to only show "Copied!" after the clipboard copy operation actually completes.

Previously, the Share button would immediately show "Copied!" when clicked, before the async `persist` and `clipboard.writeText` operations finished. This caused a confusing UX where users would see "Copied!" but if they tabbed away quickly, the copy would fail with a `NotAllowedError: Document is not focused` error.

Now the button waits for `onShare()` to complete before updating the UI state.

Fixes #21691

## Test Plan

<!-- How was it tested? -->

Tested locally with `npm start --workspace ruff-playground`

## Demo
![Kapture 2025-12-12 at 20 47 51](https://github.com/user-attachments/assets/dcbb505c-724f-4538-af8a-1d96d6f17d32)


---

_Comment by @dhruvmanila on 2025-12-12 11:30_

Ohh, this is what was happening! I noticed this multiple times but didn't dig into it. Thanks for opening a fix for this issue, it would be useful to add a short video to demo this in the PR description when you make it ready for review.

---

_Marked ready for review by @mahiro72 on 2025-12-12 11:53_

---

_Label `playground` added by @AlexWaygood on 2025-12-12 13:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-13 18:16_

---

_Review comment by @MichaReiser on `playground/ruff/src/Editor/Chrome.tsx`:27 on 2025-12-15 07:13_

```suggestion
			try {
        await persist(serialized);
      } catch (error) {
        // eslint-disable-next-line no-console
        console.error("Failed to share playground", error);
      }
```

---

_Review comment by @MichaReiser on `playground/shared/src/ShareButton.tsx`:37 on 2025-12-15 07:17_

I think we want to distinguish between three states

* `initial`: Button is clickable, label is `Share`
* `copying`: Button is disabled, label is `Share`
* `copied`: Button is disabled, label is `Copied`

We should ensure that we reset the state to `Share` if `onShare` throws (maybe by moving the try catch handling from `onShare` to here, we could even consider having a 4th state `failed`)

---

_@MichaReiser reviewed on 2025-12-15 07:18_

Thank you.

I think we should move the error handling into `ShareButton` to avoid async race conditions or stale states if sharing failed.

---

_Comment by @mahiro72 on 2025-12-16 18:25_

Thank you for the review @MichaReiser ! 

I’ve addressed your feedback:
- Moved error handling from `handleShare` to `ShareButton`
- Added three states: `initial`, `copying`, and `copied`

Please take another look when you have a chance.

---

_@MichaReiser approved on 2025-12-17 11:15_

This is great. Thank you

---

_Merged by @MichaReiser on 2025-12-17 11:16_

---

_Closed by @MichaReiser on 2025-12-17 11:16_

---
