```yaml
number: 10609
title: Update Monaco
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/monaco
created_at: 2024-03-26T10:59:32Z
updated_at: 2024-03-26T11:41:22Z
url: https://github.com/astral-sh/ruff/pull/10609
synced_at: 2026-01-12T15:55:32Z
```

# Update Monaco

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [@monaco-editor/react](https://togithub.com/suren-atoyan/monaco-react) | [`4.5.1` -> `4.6.0`](https://renovatebot.com/diffs/npm/@monaco-editor%2freact/4.5.1/4.6.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@monaco-editor%2freact/4.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@monaco-editor%2freact/4.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@monaco-editor%2freact/4.5.1/4.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@monaco-editor%2freact/4.5.1/4.6.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [monaco-editor](https://togithub.com/microsoft/monaco-editor) | [`^0.40.0` -> `^0.47.0`](https://renovatebot.com/diffs/npm/monaco-editor/0.40.0/0.47.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/monaco-editor/0.47.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/monaco-editor/0.47.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/monaco-editor/0.40.0/0.47.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/monaco-editor/0.40.0/0.47.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>suren-atoyan/monaco-react (@&#8203;monaco-editor/react)</summary>

### [`v4.6.0`](https://togithub.com/suren-atoyan/monaco-react/blob/HEAD/CHANGELOG.md#460)

[Compare Source](https://togithub.com/suren-atoyan/monaco-react/compare/v4.5.2...v4.6.0)

###### *Oct 6, 2023*

-   Editor/DiffEditor: use `'use client'` on top of `Editor.tsx` and `DiffEditor.tsx`
-   loader: update `@monaco-editor/loader` version (1.4.0)
-   playground: use createRoot for bootstrapping

### [`v4.5.2`](https://togithub.com/suren-atoyan/monaco-react/blob/HEAD/CHANGELOG.md#452)

[Compare Source](https://togithub.com/suren-atoyan/monaco-react/compare/v4.5.1...v4.5.2)

###### *Aug 23, 2023*

-   DiffEditor: apply updated on `originalModelPath` and `modifiedModelPath` before `original` and `modified` props

</details>

<details>
<summary>microsoft/monaco-editor (monaco-editor)</summary>

### [`v0.47.0`](https://togithub.com/microsoft/monaco-editor/blob/HEAD/CHANGELOG.md#0470)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.46.0...v0.47.0)

##### Additions

-   Bug fixes
-   `registerNewSymbolNameProvider`
-   Experimental `registerInlineEditProvider`

### [`v0.46.0`](https://togithub.com/microsoft/monaco-editor/blob/HEAD/CHANGELOG.md#0460)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.45.0...v0.46.0)

-   Bug fixes

### [`v0.45.0`](https://togithub.com/microsoft/monaco-editor/blob/HEAD/CHANGELOG.md#0450)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.44.0...v0.45.0)

##### Breaking Changes

-   `wordBasedSuggestions: boolean` -> `'off' | 'currentDocument' | 'matchingDocuments' | 'allDocuments'`
-   `occurrencesHighlight: boolean` -> `'off' | 'singleFile' | 'multiFile'`

##### Additions

-   Many bug fixes
-   `IEditorScrollbarOptions.ignoreHorizontalScrollbarInContentHeight`
-   `IDiffEditor.goToDiff`
-   `IDiffEditor.revealFirstDiff`

### [`v0.44.0`](https://togithub.com/microsoft/monaco-editor/blob/HEAD/CHANGELOG.md#0440)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.43.0...v0.44.0)

-   Removes old diff editor implementation.
-   Custom diff algorithms no longer can be passed via diff editor options, instead a service should be used ([see #&#8203;3558 for more details](https://togithub.com/microsoft/monaco-editor/issues/3558)).

### [`v0.43.0`](https://togithub.com/microsoft/monaco-editor/compare/v0.41.0...v0.43.0)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.41.0...v0.43.0)

### [`v0.41.0`](https://togithub.com/microsoft/monaco-editor/blob/HEAD/CHANGELOG.md#0410)

[Compare Source](https://togithub.com/microsoft/monaco-editor/compare/v0.40.0...v0.41.0)

-   `IDiffEditor.diffReviewNext` was renamed to `IDiffEditor.accessibleDiffViewerNext`.
-   `IDiffEditor.diffReviewPrev` was renamed to `IDiffEditor.accessibleDiffViewerPrev`.
-   Introduces `InlineCompletionsProvider.yieldsToGroupIds` to allows inline completion providers to yield to other providers.
-   Bugfixes

Contributions to `monaco-editor`:

-   [@&#8203;claylibrarymarket](https://togithub.com/claylibrarymarket): Fix Twig's plain text class expression [PR #&#8203;4063](https://togithub.com/microsoft/monaco-editor/pull/4063)
-   [@&#8203;FossPrime (Ray Foss)](https://togithub.com/FossPrime): Use new GitHub pages workflow [PR #&#8203;4000](https://togithub.com/microsoft/monaco-editor/pull/4000)
-   [@&#8203;leandrocp (Leandro Pereira)](https://togithub.com/leandrocp): Elixir - Add support for multi-letter uppercase sigils [PR #&#8203;4041](https://togithub.com/microsoft/monaco-editor/pull/4041)
-   [@&#8203;philippleidig (PhilippLe)](https://togithub.com/philippleidig): Add TwinCAT file support for structured text (st) language [PR #&#8203;3315](https://togithub.com/microsoft/monaco-editor/pull/3315)
-   [@&#8203;remcohaszing (Remco Haszing)](https://togithub.com/remcohaszing)
    -   Add mdx language [PR #&#8203;3096](https://togithub.com/microsoft/monaco-editor/pull/3096)
    -   Export custom TypeScript worker variables [PR #&#8203;3488](https://togithub.com/microsoft/monaco-editor/pull/3488)
    -   Document some basic concepts [PR #&#8203;4087](https://togithub.com/microsoft/monaco-editor/pull/4087)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://togithub.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-03-26 10:59_

---

_Comment by @MichaReiser on 2024-03-26 11:35_

I tested locally that:
* Autofixes are still working
* Clicked through all the panels and verified that syntax highlighting continues to work

---

_Merged by @MichaReiser on 2024-03-26 11:41_

---

_Closed by @MichaReiser on 2024-03-26 11:41_

---

_Branch deleted on 2024-03-26 11:41_

---
