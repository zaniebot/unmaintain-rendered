```yaml
number: 10607
title: Group some NPM dependency updates
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: group-some-npm-dependencies
created_at: 2024-03-26T10:06:52Z
updated_at: 2024-03-26T10:40:57Z
url: https://github.com/astral-sh/ruff/pull/10607
synced_at: 2026-01-10T22:47:02Z
```

# Group some NPM dependency updates

---

_Pull request opened by @MichaReiser on 2024-03-26 10:06_

## Summary

Group some npm dependencies that should be upgraded together (e.g. all vite or monaco dependencies)

## Test Plan

I validate the configuration with `renovate-config-validator`


---

_Label `ci` added by @MichaReiser on 2024-03-26 10:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-26 10:12_

---

_@AlexWaygood reviewed on 2024-03-26 10:13_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:14 on 2024-03-26 10:13_

nit: this is a `.json5` file, not `.json`, so we can use trailing commas to reduce diffs from future changes to this file:

```suggestion
  "suppressNotifications": [
    "prEditedNotification",
  ],
  "extends": [
    "config:recommended",
  ],
  "labels": [
    "internal",
  ],
  "schedule": [
    "before 4am on Monday",
```

Did prettier or another autoformatter make the changes to these lines?

---

_Review comment by @MichaReiser on `.github/renovate.json5`:2 on 2024-03-26 10:14_

I Formatted the file with Prettier, it removes the unnecessary quotes.

---

_@MichaReiser reviewed on 2024-03-26 10:14_

---

_@MichaReiser reviewed on 2024-03-26 10:14_

---

_Review comment by @MichaReiser on `.github/renovate.json5`:14 on 2024-03-26 10:14_

It was jetbrains. i disabled the autoformatter for JSON and instead ran prettier (which is what we use for other files)

---

_@AlexWaygood reviewed on 2024-03-26 10:17_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:14 on 2024-03-26 10:17_

I like the prettier changes more than the JetBrains changes :) though it's a _little_ hard to see here which changes have an impact on the config and which are just cosmetic

---

_@AlexWaygood reviewed on 2024-03-26 10:20_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:46 on 2024-03-26 10:20_

This group currently just includes `@monaco-editor/react` and `monaco-editor`, right? (Just checking I understand correctly)

---

_@MichaReiser reviewed on 2024-03-26 10:35_

---

_Review comment by @MichaReiser on `.github/renovate.json5`:46 on 2024-03-26 10:35_

Yes, the main reason is that it doesn't make sense to update them separately.

---

_Renamed from "Group NPM dependency upgrades" to "Group some NPM dependency updates" by @MichaReiser on 2024-03-26 10:39_

---

_Merged by @MichaReiser on 2024-03-26 10:39_

---

_Closed by @MichaReiser on 2024-03-26 10:39_

---

_Branch deleted on 2024-03-26 10:39_

---

_Comment by @AlexWaygood on 2024-03-26 10:40_

Thanks!

---
