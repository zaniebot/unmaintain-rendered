```yaml
number: 1334
title: Bump version to 0.0.1-alpha.22
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: alex/release
created_at: 2025-10-10T12:17:33Z
updated_at: 2025-10-10T12:52:18Z
url: https://github.com/astral-sh/ty/pull/1334
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.22

---

_Pull request opened by @AlexWaygood on 2025-10-10 12:17_

_No description provided._

---

_Label `release` added by @AlexWaygood on 2025-10-10 12:18_

---

_Marked ready for review by @AlexWaygood on 2025-10-10 12:24_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:5 on 2025-10-10 12:37_

I don't think we have this for any of the other releases. What's the motivation for adding it? Isn't it redundant with the information in the releases page

---

_Review comment by @MichaReiser on `CHANGELOG.md`:15 on 2025-10-10 12:38_

The playground is released independently

```suggestion
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:29 on 2025-10-10 12:39_

The `etc` part is unclear to me. What does it refer to, other types than `None` or other types than `Union`?

```suggestion
- Infer better specializations of unions with `None` ([#20749](https://github.com/astral-sh/ruff/pull/20749))
```

---

_@sharkdp approved on 2025-10-10 12:39_

Thank you!

---

_Review comment by @sharkdp on `CHANGELOG.md`:15 on 2025-10-10 12:40_

We omitted playground changes, so far. But I'm also fine with keeping them
```suggestion
```

---

_Review comment by @sharkdp on `CHANGELOG.md`:52 on 2025-10-10 12:40_

```suggestion
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:26 on 2025-10-10 12:40_

I do like the header but I wonder if we we group this under `Typing semantics and features`

---

_@sharkdp reviewed on 2025-10-10 12:40_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:44 on 2025-10-10 12:41_

```suggestion
- Optimise and generalise union/intersection simplification ([#20602](https://github.com/astral-sh/ruff/pull/20602))
```

The how seems irrelevant to users

---

_Review comment by @MichaReiser on `CHANGELOG.md`:52 on 2025-10-10 12:41_

```suggestion
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:53 on 2025-10-10 12:41_

```suggestion
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:73 on 2025-10-10 12:42_

```suggestion
### Other typing semantics and features
```

---

_@MichaReiser approved on 2025-10-10 12:42_

---

_@AlexWaygood reviewed on 2025-10-10 12:44_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:5 on 2025-10-10 12:44_

Rooster added it automatically 🤷

---

_@AlexWaygood reviewed on 2025-10-10 12:45_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2025-10-10 12:45_

These features are all related and the number of new features this release is huge because it's been such a while since the last release. I think it's much easier to read this changelog if we have these in a separate section.

---

_@AlexWaygood reviewed on 2025-10-10 12:46_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:53 on 2025-10-10 12:46_

this could theoretically have a user-facing impact for anybody else who wants to create a WASM build of Ruff, though, right?

---

_@MichaReiser reviewed on 2025-10-10 12:47_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:53 on 2025-10-10 12:47_

That's true. But we don't release or version the WASM build. That's why I'd remove it

---

_@AlexWaygood reviewed on 2025-10-10 12:49_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:29 on 2025-10-10 12:49_

```suggestion
- Improve solving of a type variable `T` if it appears in a union with non-`TypeVar`s (`T | None`, `T | str | None`, etc.) ([#20749](https://github.com/astral-sh/ruff/pull/20749))
```

---

_Merged by @AlexWaygood on 2025-10-10 12:52_

---

_Closed by @AlexWaygood on 2025-10-10 12:52_

---

_Branch deleted on 2025-10-10 12:52_

---
