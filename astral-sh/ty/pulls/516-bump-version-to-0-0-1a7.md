```yaml
number: 516
title: Bump version to 0.0.1a7
type: pull_request
state: merged
author: sharkdp
labels:
  - release
assignees: []
merged: true
base: main
head: david/bump-0-0-1a7
created_at: 2025-05-26T18:13:31Z
updated_at: 2025-05-26T18:39:36Z
url: https://github.com/astral-sh/ty/pull/516
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1a7

---

_@sharkdp_

_No description provided._

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2025-05-26 18:18_

```suggestion
- Implement Python's floor-division semantics for `Literal` `int`s ([#18249](https://github.com/astral-sh/ruff/pull/18249))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:8 on 2025-05-26 18:19_

```suggestion
- Don't warn about a `yield` expression not being in a function if the `yield` expression is in a function ([#18008](https://github.com/astral-sh/ruff/pull/18008))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-05-26 18:19_

```suggestion
- Fix inference of attribute writes to unions/intersections that including module-literal types ([#18313](https://github.com/astral-sh/ruff/pull/18313))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2025-05-26 18:20_

```suggestion
- Fix false-positive diagnostics in binary comparison inference logic for intersection types ([#18266](https://github.com/astral-sh/ruff/pull/18266))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-05-26 18:21_

(Rephrase it to make it slightly more tailored to the change a user is likely to be interested in)

```suggestion
- Fix crash when hovering over a `ty_extensions.Intersection[A, B]` expression in an IDE context ([#18321](https://github.com/astral-sh/ruff/pull/18321))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-05-26 18:22_

sort of an internal change?

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2025-05-26 18:22_

```suggestion
- Understand that the presence of a `__getattribute__` method indicates arbitrary members can exist on a type ([#18280](https://github.com/astral-sh/ruff/pull/18280))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:23 on 2025-05-26 18:23_

seems kinda internal

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2025-05-26 18:24_

```suggestion
- Add `tests` to `src.root` by default if a `tests/` directory exists and is not a package ([#18286](https://github.com/astral-sh/ruff/pull/18286))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2025-05-26 18:24_

```suggestion
- Add support for detecting activated Conda and Pixi environments ([#18267](https://github.com/astral-sh/ruff/pull/18267))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2025-05-26 18:24_

```suggestion
- Move `respect-ignore-files` configuration setting under `src` section ([#18322](https://github.com/astral-sh/ruff/pull/18322))
```

---

_Label `release` added by @MichaReiser on 2025-05-26 18:24_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:50 on 2025-05-26 18:25_

```suggestion
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:55 on 2025-05-26 18:25_

Nit: We omit documentation changes most of the time for ruff (they're a bit noisy)

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:53 on 2025-05-26 18:25_

```suggestion
```

---

_@AlexWaygood approved on 2025-05-26 18:25_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:44 on 2025-05-26 18:25_

I would omit the playground as it isn't part of the release

---

_@MichaReiser approved on 2025-05-26 18:26_

ty

---

_@sharkdp reviewed on 2025-05-26 18:29_

---

_Review comment by @sharkdp on `CHANGELOG.md`:14 on 2025-05-26 18:29_

I mean, it's not the most impactful change in the world, but it's not internal. It even has some ecosystem changes.

---

_@sharkdp reviewed on 2025-05-26 18:31_

---

_Review comment by @sharkdp on `CHANGELOG.md`:55 on 2025-05-26 18:31_

Removed.

---

_Review comment by @sharkdp on `CHANGELOG.md`:44 on 2025-05-26 18:31_

Removed.

---

_@sharkdp reviewed on 2025-05-26 18:31_

---

_@AlexWaygood reviewed on 2025-05-26 18:33_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-05-26 18:33_

Yeah. "Is there a better way of explaining what the user-facing impact is in a sentence?", is a better question, I guess 😄

But we shouldn't spend too much time figuring out the best way of describing the change; it's only an alpha release. I just don't want _too_ many baffling bullet points in our changelog entries ;)

---

_Comment by @sharkdp on 2025-05-26 18:35_

Thank you for the reviews, especially for the rewording, @AlexWaygood!

---

_Merged by @sharkdp on 2025-05-26 18:39_

---

_Closed by @sharkdp on 2025-05-26 18:39_

---

_Branch deleted on 2025-05-26 18:39_

---
