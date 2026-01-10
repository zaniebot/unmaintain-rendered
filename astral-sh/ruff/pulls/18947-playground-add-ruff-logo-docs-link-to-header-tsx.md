```yaml
number: 18947
title: "[`playground`] Add ruff logo docs link to Header.tsx"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - playground
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-25T21:27:02Z
updated_at: 2025-06-26T06:54:59Z
url: https://github.com/astral-sh/ruff/pull/18947
synced_at: 2026-01-10T18:39:09Z
```

# [`playground`] Add ruff logo docs link to Header.tsx

---

_Pull request opened by @MeGaGiGaGon on 2025-06-25 21:27_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #18825

This should make it so that clicking on the top left `RUFF` in the playground will take you to the docs. Untested locally since I could not figure out how to get `npm` to install correctly, so hopefully it just works/someone else can test it for me.

I did not add a link to the `ty` playground since a dedicated docs pages does not exist yet/it still says "ASTRAL" in the corner instead of having a `ty` logo.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected.

---

_Renamed from "[`playground`] Add ruff logo link to Header.tsx" to "[`playground`] Add ruff logo docs link to Header.tsx" by @MeGaGiGaGon on 2025-06-25 21:27_

---

_Label `playground` added by @AlexWaygood on 2025-06-25 21:27_

---

_Comment by @ntBre on 2025-06-25 22:23_

Thanks, it seems to work for me! I guess you can't really see my cursor, but I'm clicking on the logo in the header.

![recording](https://github.com/user-attachments/assets/89cc352c-ceaa-4794-95c5-0954a9777300)

I'll let Micha take a look just in case there's more subtlety here, but it looks good to me.


---

_Review requested from @MichaReiser by @ntBre on 2025-06-25 22:23_

---

_Comment by @MichaReiser on 2025-06-26 06:54_

Thank you

---

_Merged by @MichaReiser on 2025-06-26 06:54_

---

_Closed by @MichaReiser on 2025-06-26 06:54_

---

_Branch deleted on 2025-06-26 06:54_

---
