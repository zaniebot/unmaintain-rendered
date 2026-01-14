```yaml
number: 22576
title: "[ty] Fix flaky completions"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-use-callback
created_at: 2026-01-14T17:53:05Z
updated_at: 2026-01-14T18:23:43Z
url: https://github.com/astral-sh/ruff/pull/22576
synced_at: 2026-01-14T18:48:06Z
```

# [ty] Fix flaky completions

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2415

Thanks to @RasmusNygren for [figuring out](https://github.com/astral-sh/ruff/pull/22492) that 
registering to the editor events solves the problem too. This made it much easier to identify the root cause.


The root cause was that we didn't memoize the `onChange` callback. Each render passed a new 
instance. In turn, [react-monaco](https://github.com/react-monaco-editor/react-monaco-editor/blob/3062386185b0fee4fb2aa5002e5ef5fdedc05dbb/src/editor.tsx#L63-L67) registers a new content change subscription on each render. However, it doesn't do so immediately; it only does so in an effect and effects run deferred (after the render). This delayed subscription was the reason that the `onChange` handler was triggered after the editor's completion request.

The solution is relatively easy. Use `useCallback` in more places. This should also help to reduce unnecessary renders.

## Test Plan


https://github.com/user-attachments/assets/84322a09-2875-494f-a9fb-eb8e3928a5ac




---

_Label `playground` added by @MichaReiser on 2026-01-14 17:53_

---

_Label `ty` added by @MichaReiser on 2026-01-14 17:53_

---

_Label `playground` added by @MichaReiser on 2026-01-14 17:53_

---

_Label `ty` added by @MichaReiser on 2026-01-14 17:53_

---

_Review requested from @Copilot by @MichaReiser on 2026-01-14 17:56_

---

_Marked ready for review by @MichaReiser on 2026-01-14 17:56_

---

_@AlexWaygood approved on 2026-01-14 18:02_

Cannot review the typescript, but v happy to confirm that this fixes the flakes for me locally!

---

_@copilot-pull-request-reviewer[bot] reviewed on 2026-01-14 18:22_

## Pull request overview

This PR fixes a flaky completions issue caused by unstable callback references that led to delayed subscription to editor content changes. The root cause was that `onChange` callbacks were recreated on each render, causing react-monaco to register new subscriptions in effects that ran after completion requests.

**Changes:**
- Wrapped event handler callbacks in `useCallback` to maintain stable references across renders
- Reordered operations in `handleFileChanged` to update workspace before state

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated no comments.

| File | Description |
| ---- | ----------- |
| playground/ty/src/Playground.tsx | Wrapped `handleFileAdded`, `handleFileChanged`, `handleFileRenamed`, and `handleFileRemoved` in `useCallback` with appropriate dependencies; reordered workspace update before state update in `handleFileChanged` |
| playground/ty/src/Editor/Chrome.tsx | Added `handleChange` wrapper with `useCallback` to pass stable reference to Editor component |





---

💡 <a href="/astral-sh/ruff/new/main/.github/instructions?filename=*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

_Comment by @MichaReiser on 2026-01-14 18:23_

I very much appreciated it @copilot how you can repeat the same as is written in the summary but make it even longer

---

_Merged by @MichaReiser on 2026-01-14 18:23_

---

_Closed by @MichaReiser on 2026-01-14 18:23_

---

_Branch deleted on 2026-01-14 18:23_

---

_Comment by @Copilot on 2026-01-14 18:23_

@MichaReiser I've opened a new pull request, #22579, to work on those changes. Once the pull request is ready, I'll request review from you.

---
