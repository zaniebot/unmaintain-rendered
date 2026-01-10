```yaml
number: 1371
title: Bump version to 0.0.1-alpha.23
type: pull_request
state: merged
author: sharkdp
labels: []
assignees: []
merged: true
base: main
head: david/bump-0-0-1a23
created_at: 2025-10-16T17:13:33Z
updated_at: 2025-10-16T18:02:42Z
url: https://github.com/astral-sh/ty/pull/1371
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.23

---

_Pull request opened by @sharkdp on 2025-10-16 17:13_

I excluded the following items from the CHANGELOG. Let me know if you want one of these to be included:

- Rooster config: ignore PRs labelled `documentation` or `playground` when generating the changelog ([#1360](https://github.com/astral-sh/ty/pull/1360))
- refactor `Place` ([#20871](https://github.com/astral-sh/ruff/pull/20871))
- Standardize syntax error construction ([#20903](https://github.com/astral-sh/ruff/pull/20903))
- Use field-specifier return type as the default type for the field ([#20915](https://github.com/astral-sh/ruff/pull/20915))
- Do not bind `self` to non-positional parameters ([#20850](https://github.com/astral-sh/ruff/pull/20850))

The last one is *in principle* observable, but sounds much more scary than it is, as it only affected `CallableTypeOf[…]`, I believe.

---

_Comment by @AlexWaygood on 2025-10-16 17:19_

> Remove "pre-release software" warning ([#20817](https://github.com/astral-sh/ruff/pull/20817))

That one has actually been asked for by users, I think (https://github.com/astral-sh/ty/issues/1253) so it might be worth including!

> Rooster config: ignore PRs labelled `documentation` or `playground` when generating the changelog ([#1360](https://github.com/astral-sh/ty/pull/1360))

Hah. We should probably also ignore PRs labelled `release` so this kind of thing is auto-excluded 😆

> cache Type::is_redundant_with ([#20477](https://github.com/astral-sh/ruff/pull/20477))

This was a big performance improvement and fixed hangs on some projects! Definitely worth including, I think

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-10-16 17:20_

```suggestion
- Fix handling of dataclass `field()`s without default values ([#20914](https://github.com/astral-sh/ruff/pull/20914))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-10-16 17:20_

these can be combined into one bullet IMO

```suggestion
- Fix false-positive diagnostics on `super()` calls ([#20814](https://github.com/astral-sh/ruff/pull/20814), [#20843](https://github.com/astral-sh/ruff/pull/20843))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2025-10-16 17:21_

```suggestion
- Fix runaway execution time for mutually referential instance attributes ([#20645](https://github.com/astral-sh/ruff/pull/20645))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2025-10-16 17:22_

this feels a bit cryptic but I'm not sure how to improve it. We could possibly just omit it (or maybe move it down to the `Type inference and diagnostics` section?)

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2025-10-16 17:22_

```suggestion
- For `unresolved-import` diagnostics, limit the shown import paths to at most five, unless in verbose mode ([#20912](https://github.com/astral-sh/ruff/pull/20912))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:25 on 2025-10-16 17:22_

```suggestion
- Improve ranking of autocomplete suggestions ([#20807](https://github.com/astral-sh/ruff/pull/20807))
```

---

_@AlexWaygood approved on 2025-10-16 17:23_

Thank you!

---

_Review comment by @dcreager on `CHANGELOG.md`:46 on 2025-10-16 17:49_

Do we typically cite the Actions bot?

---

_@dcreager reviewed on 2025-10-16 17:49_

---

_@sharkdp reviewed on 2025-10-16 17:51_

---

_Review comment by @sharkdp on `CHANGELOG.md`:46 on 2025-10-16 17:51_

We did, a few times, but happy to remove.

---

_@sharkdp reviewed on 2025-10-16 17:51_

---

_Review comment by @sharkdp on `CHANGELOG.md`:46 on 2025-10-16 17:51_

```suggestion
```

---

_@AlexWaygood reviewed on 2025-10-16 17:51_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:46 on 2025-10-16 17:51_

you would deny credit to one of our most prolific contributors??

---

_Merged by @sharkdp on 2025-10-16 18:02_

---

_Closed by @sharkdp on 2025-10-16 18:02_

---

_Branch deleted on 2025-10-16 18:02_

---
