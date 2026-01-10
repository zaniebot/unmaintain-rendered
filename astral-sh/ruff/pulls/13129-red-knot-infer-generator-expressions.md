```yaml
number: 13129
title: "[red-knot] Infer generator expressions"
type: pull_request
state: closed
author: dylwil3
labels:
  - ty
assignees: []
base: main
head: infer-generator
created_at: 2024-08-27T22:27:21Z
updated_at: 2024-08-28T00:58:48Z
url: https://github.com/astral-sh/ruff/pull/13129
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Infer generator expressions

---

_Pull request opened by @dylwil3 on 2024-08-27 22:27_

Adds a `Generator` type and infers the type of a generator expression as `Generator[T, None, None]` where `T` is the inferred type of the first comprehension.

Related to astral-sh/ty#244.


---

_Review requested from @carljm by @dylwil3 on 2024-08-27 22:27_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-08-27 22:27_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-27 22:27_

---

_Comment by @dylwil3 on 2024-08-27 22:27_

This is my first one of these, so apologies if I'm way off!

---

_Comment by @github-actions[bot] on 2024-08-27 22:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-08-27 23:50_

---

_Comment by @carljm on 2024-08-27 23:59_

Hey, thanks so much for the PR! So I should have added more color to astral-sh/ty#244 to clarify some of these better; this is an example of one where we're kinda blocked on adding support for generics in order to do this correctly. The generator type should be derived from its definition in typeshed rather than added as a special case in the `Type` enum. And understanding its definition in typeshed requires that we understand type variables and generic classes, which we don't have. Sorry this wasn't clear in the issue, I've updated the text of the issue now to mark the ones blocked on this!

Going to close this since I think pretty much everything here would look different with the typeshed/generics approach, sorry about that! If you're interested in tackling an open expression kind, there are gaps in our unary/binary expression support, or for something a bit more complex, lambda expressions might be interesting. @chriskrycho is currently working on call expression inference, which will unlock a bunch more expression kinds where we should be resolving them as dunder method calls. Feel free to ping us on Discord or in the issue if you want to get a bit of direction before starting work on an issue!

---

_Closed by @carljm on 2024-08-27 23:59_

---

_Branch deleted on 2024-08-28 00:57_

---

_Comment by @dylwil3 on 2024-08-28 00:58_

Not a problem! Sorry for spamming the PRs 😅 I'll coordinate on the issue/Discord before taking another stab. Thanks!

---
