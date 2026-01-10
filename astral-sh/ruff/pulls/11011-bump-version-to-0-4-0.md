```yaml
number: 11011
title: Bump version to 0.4.0
type: pull_request
state: merged
author: zanieb
labels:
  - release
assignees: []
merged: true
base: main
head: release/040
created_at: 2024-04-18T14:47:42Z
updated_at: 2024-04-18T19:21:22Z
url: https://github.com/astral-sh/ruff/pull/11011
synced_at: 2026-01-10T22:37:01Z
```

# Bump version to 0.4.0

---

_Pull request opened by @zanieb on 2024-04-18 14:47_

_No description provided._

---

_Label `release` added by @zanieb on 2024-04-18 14:47_

---

_Comment by @github-actions[bot] on 2024-04-18 15:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb reviewed on 2024-04-18 15:13_

---

_Review comment by @zanieb on `CHANGELOG.md`:7 on 2024-04-18 15:13_

We should say something like: "Ruff just got 2x faster" :)

---

_Review comment by @snowsignal on `CHANGELOG.md`:28 on 2024-04-18 15:13_

I think the link needs to be fixed here - it should be https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/README.md.

---

_@snowsignal reviewed on 2024-04-18 15:13_

---

_@AlexWaygood reviewed on 2024-04-18 15:22_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2024-04-18 15:22_

found another `with` bug

```suggestion
server that comes built-in with Ruff. It can be used with any editor that supports the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) (LSP).
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2024-04-18 15:22_

```suggestion
of exciting features. It’s also faster than our previous [Python-based language server](https://github.com/astral-sh/ruff-lsp)
-- but you probably guessed that already.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:24 on 2024-04-18 15:23_

```suggestion
- Comes with commands for quickly performing actions: `ruff.applyAutofix`, `ruff.applyFormat`, and `ruff.applyOrganizeImports`
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2024-04-18 15:23_

```suggestion
- Automatically reloads your project configuration when you change it
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2024-04-18 15:24_

```suggestion
- \[`pycodestyle`\] Do not trigger `E3` rules on `def`s following a function/method with a dummy body ([#10704](https://github.com/astral-sh/ruff/pull/10704))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2024-04-18 15:28_

```suggestion
- \[`flake8-bandit`\] Allow `urllib.request.urlopen` calls with static `Request` argument (`S310`) ([#10964](https://github.com/astral-sh/ruff/pull/10964))
- \[`flake8-bugbear`\] Treat `raise NotImplemented`-only bodies as stub functions (`B006`) ([#10990](https://github.com/astral-sh/ruff/pull/10990))
- \[`flake8-slots`\] Respect same-file `Enum` subclasses (`SLOT000`) ([#11006](https://github.com/astral-sh/ruff/pull/11006))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:72 on 2024-04-18 15:30_

```suggestion
- Avoid `non-augmented-assignment` for reversed, non-commutative operators (`PLR6104`) ([#10909](https://github.com/astral-sh/ruff/pull/10909))
- Limit commutative non-augmented-assignments to primitive data types (`PLR6104`) ([#10912](https://github.com/astral-sh/ruff/pull/10912))
```

---

_@AlexWaygood reviewed on 2024-04-18 15:31_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:56 on 2024-04-18 16:24_

```suggestion
*This section is devoted to updates for our new language server, written in Rust.*
```

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:63 on 2024-04-18 16:25_

I think we should emit the common prefix "ruff server"

```suggestion
- Refreshes diagnostics for open files when file configuration is changed ([#10988](https://github.com/astral-sh/ruff/pull/10988))
- Important errors are now shown as popups ([#10951](https://github.com/astral-sh/ruff/pull/10951))
- Introduce settings for directly configuring the linter and formatter ([#10984](https://github.com/astral-sh/ruff/pull/10984))
- Resolve configuration for each document individually ([#10950](https://github.com/astral-sh/ruff/pull/10950))
- Write a setup guide for Neovim ([#10987](https://github.com/astral-sh/ruff/pull/10987))
```

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:7 on 2024-04-18 16:32_

Well, it's the _parser_ which got 2x faster, Ruff only about 30-50% ;)

---

_@dhruvmanila reviewed on 2024-04-18 16:32_

---

_@AlexWaygood reviewed on 2024-04-18 16:33_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2024-04-18 16:33_

"only" 😆

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:74 on 2024-04-18 18:26_

New commits
```suggestion
- Consider `if` expression for parenthesized with items parsing ([#11010](https://github.com/astral-sh/ruff/pull/11010))
- Consider binary expr for parenthesized with items parsing ([#11012](https://github.com/astral-sh/ruff/pull/11012))
- Reset `FOR_TARGET` context for all kinds of parentheses ([#11009](https://github.com/astral-sh/ruff/pull/11009))

```

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:8 on 2024-04-18 18:26_

```suggestion
There's a lot to say about this exciting change, so check out the [blog post](https://astral.sh/blog/ruff-v0.4.0) for more details!
```

---

_@dhruvmanila reviewed on 2024-04-18 18:54_

---

_@AlexWaygood reviewed on 2024-04-18 18:55_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:84 on 2024-04-18 18:55_

Could consider omitting this section; I don't think most users will care much

```suggestion
```

---

_Merged by @dhruvmanila on 2024-04-18 19:10_

---

_Closed by @dhruvmanila on 2024-04-18 19:10_

---

_Branch deleted on 2024-04-18 19:10_

---
