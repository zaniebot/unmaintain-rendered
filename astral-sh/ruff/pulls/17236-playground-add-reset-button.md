```yaml
number: 17236
title: "[playground] Add Reset button"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/playground-reset
created_at: 2025-04-06T16:54:23Z
updated_at: 2025-04-06T17:13:22Z
url: https://github.com/astral-sh/ruff/pull/17236
synced_at: 2026-01-10T19:40:37Z
```

# [playground] Add Reset button

---

_Pull request opened by @MichaReiser on 2025-04-06 16:54_

## Summary

Add a *Reset* button to both Ruff's and Red Knot's playground that resets the playground to its initial state.

Closes https://github.com/astral-sh/ruff/issues/17195

## Test Plan


https://github.com/user-attachments/assets/753cca19-155a-44b1-89ba-76744487a55d

https://github.com/user-attachments/assets/7d19f04c-70f4-4d9e-b745-0486cb1d4993



---

_Label `playground` added by @MichaReiser on 2025-04-06 16:54_

---

_Review requested from @Copilot by @MichaReiser on 2025-04-06 16:56_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-06 16:56_



Copilot reviewed 6 out of 6 changed files in this pull request and generated no comments.


<details>
<summary>Comments suppressed due to low confidence (2)</summary>

**playground/shared/src/Header.tsx:135**
* [nitpick] Consider renaming the 'onClicked' prop to 'onClick' in ResetButton for better alignment with standard React naming conventions.
```
function ResetButton({ onClicked }: { onClicked?: () => void }) {
```
**playground/shared/src/ThemeButton.tsx:17**
* [nitpick] Review the removal of 'ml-4' from the className to ensure that the layout spacing remains as intended across all screen sizes.
```
className="sm:ml-0 dark:shadow-copied"
```
</details>



---

_Review requested from @Copilot by @MichaReiser on 2025-04-06 17:05_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-06 17:05_



Copilot reviewed 8 out of 8 changed files in this pull request and generated no comments.


<details>
<summary>Comments suppressed due to low confidence (4)</summary>

**playground/shared/src/ShareButton.tsx:31**
* Since the onShare prop is now required, the previous null check in the disabled prop has been removed. Ensure that every usage of ShareButton supplies a valid onShare function to avoid runtime errors.
```
disabled={copied}
```
**playground/shared/src/Header.tsx:52**
* [nitpick] Consider renaming the ResetButton prop 'onClicked' to 'onReset' for improved clarity and to align with the naming conventions used elsewhere in the code.
```
          <ResetButton onClicked={onReset} />
```
**playground/knot/src/Playground.tsx:131**
* When closing open files in the reset handler, consider adding error logging or additional handling to ensure that failures in workspace.closeFile do not silently affect the reset process.
```
  const handleReset = useCallback(() => {
```
**playground/knot/src/Editor/Editor.tsx:113**
* [nitpick] Adding a comment explaining the purpose of the key prop (forcing a remount of the editor on reset) would help future maintainers understand why this approach is necessary.
```
      key={files.playgroundRevision}
```
</details>



---

_Marked ready for review by @MichaReiser on 2025-04-06 17:08_

---

_Merged by @MichaReiser on 2025-04-06 17:09_

---

_Closed by @MichaReiser on 2025-04-06 17:09_

---

_Branch deleted on 2025-04-06 17:09_

---

_Comment by @sharkdp on 2025-04-06 17:10_

Thank you Micha!

---

_Comment by @MichaReiser on 2025-04-06 17:13_

Thanks for the idea. I hope I didn't break too much :)

---
