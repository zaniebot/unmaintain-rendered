```yaml
number: 1416
title: Release 0.0.1-alpha.24
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: alpha-24
created_at: 2025-10-23T12:26:21Z
updated_at: 2025-10-23T13:16:45Z
url: https://github.com/astral-sh/ty/pull/1416
synced_at: 2026-01-12T15:54:27Z
```

# Release 0.0.1-alpha.24

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2025-10-23 12:26_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-10-23 12:34_

```suggestion
- Prefer the declared type over the inferred type for invariant collection literals ([#20927](https://github.com/astral-sh/ruff/pull/20927))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2025-10-23 12:35_

```suggestion
- Use declared variable types as bidirectional type context for solving type variables ([#20796](https://github.com/astral-sh/ruff/pull/20796))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-10-23 12:36_

```suggestion
- Add support for legacy namespace packages ([#20897](https://github.com/astral-sh/ruff/pull/20897))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2025-10-23 12:36_

```suggestion
- Add suggestion to "unknown rule" diagnostics ([#20948](https://github.com/astral-sh/ruff/pull/20948))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2025-10-23 12:36_

```suggestion
- Improve error messages for "unresolved attribute" diagnostics ([#20963](https://github.com/astral-sh/ruff/pull/20963))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:29 on 2025-10-23 12:37_

```suggestion
- Fix autocomplete suggestions when the cursor is at the end of a file ([#20993](https://github.com/astral-sh/ruff/pull/20993))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-10-23 12:37_

```suggestion
- Fix rare hang relating to multithreading ([#21038](https://github.com/astral-sh/ruff/pull/21038))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-10-23 12:38_

```suggestion
- Fix auto-import edits made by autocompletions for files with an existing `from __future__` import ([#20987](https://github.com/astral-sh/ruff/pull/20987))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2025-10-23 12:38_

```suggestion
- Display variance when hovering over type variables ([#20900](https://github.com/astral-sh/ruff/pull/20900))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:44 on 2025-10-23 12:38_

what does this mean?

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:48 on 2025-10-23 12:39_

This could go in "Type inference and diagnostics", since it's an improvement to our diagnostics

also:

```suggestion
- Truncate `Literal` type display in some situations ([#20928](https://github.com/astral-sh/ruff/pull/20928))
```

---

_@AlexWaygood approved on 2025-10-23 12:39_

thank you!

---

_@MichaReiser reviewed on 2025-10-23 12:48_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:44 on 2025-10-23 12:48_

I'll update the text in a minute but I'm inclined to remove it entirely as it's a bit cryptic and not really observable by users

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:45 on 2025-10-23 12:59_

```suggestion
- Avoid sending an unnecessary "clear diagnostics" message for clients supporting [pull diagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_pullDiagnostics). ([#20989](https://github.com/astral-sh/ruff/pull/20989))
```

---

_@AlexWaygood approved on 2025-10-23 13:00_

---

_Merged by @MichaReiser on 2025-10-23 13:16_

---

_Closed by @MichaReiser on 2025-10-23 13:16_

---

_Branch deleted on 2025-10-23 13:16_

---
