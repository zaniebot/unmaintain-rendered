```yaml
number: 22177
title: "[ty] Fix completion in decorators with missing declaration"
type: pull_request
state: merged
author: MichaReiser
labels:
  - parser
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-decorator-completion
created_at: 2025-12-24T16:15:36Z
updated_at: 2025-12-25T15:05:48Z
url: https://github.com/astral-sh/ruff/pull/22177
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Fix completion in decorators with missing declaration

---

_Pull request opened by @MichaReiser on 2025-12-24 16:15_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2203

The underlying issue was that the parser discarded any parsed decorators if they weren't followed by a function or class declaration because it had no place to attach them. 

This PR synthesizes a function definition for decorators that aren't followed by a declaration. It feels a bit weird but I think it's fine. It isn't really much different from what we do for:

```py
@decorator
def
```

where the function statement only consists of the `def` keyword. The only real difference is that what follows is completely made up. We have zero information that it's going to be a function definition. However, it shouldn't matter. 

If it turns out that it does matter, we could then do what TypeScript does and introduce a `MissingDeclaration` node, which signifies that we expect the next thing to be a declaration but don't know what kind. I prefer to wait to introduce this node until we have a use case where making up a function is problematic. 


Another alternative would be to convert the decorators into a sequence of `ExprStmt`. However, this complicates parsing a lot because we can only return a single statement where `parse_decorators` is called. 

There's an existing comment suggesting synthesizing a binary expression using the `@` operator (which lacks the left-hand side). While this has the advantage that the synthesized node doesn't have an empty range, it doesn't feel much different from synthesizing a function. We make up a node, but it's very unlikely the user intended to write a binary expression, which can confuse downstream tools. 

This PR also fixes the completion rendering in the playground to include the `=` for keyword arguments.


---

_Label `parser` added by @MichaReiser on 2025-12-24 16:15_

---

_Label `server` added by @MichaReiser on 2025-12-24 16:15_

---

_Label `ty` added by @MichaReiser on 2025-12-24 16:15_

---

_Marked ready for review by @MichaReiser on 2025-12-24 16:27_

---

_Review requested from @carljm by @MichaReiser on 2025-12-24 16:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-24 16:28_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-24 16:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-24 16:28_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-24 16:28_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:51 on 2025-12-24 16:32_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:524 on 2025-12-24 16:32_

```suggestion
            let target = target_token.ast(&cursor)?;
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:563 on 2025-12-24 16:32_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1466 on 2025-12-24 16:32_

```suggestion
                let node = cursor.covering_node(token.range()).node();
```

---

_@AlexWaygood reviewed on 2025-12-24 16:33_

---

_@AlexWaygood approved on 2025-12-24 16:35_

This is cool! The approach seems very reasonable to me 😃

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 16:39_


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

_Converted to draft by @MichaReiser on 2025-12-24 16:42_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-24 16:56_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-24 16:56_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-24 16:56_

---

_@MichaReiser reviewed on 2025-12-24 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2025-12-24 17:01_

I added this `recovery` context for better recovery when we see `@def` (parse it as a decorator followed by a function definition)

---

_Marked ready for review by @MichaReiser on 2025-12-24 17:24_

---

_@MichaReiser reviewed on 2025-12-24 17:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2794 on 2025-12-24 17:27_

This is a somewhat unrelated improvement but it prevents us from parsing `@def` as a decorator calling the function `@def`. I think it's much more likely in this context that `def` is the start of a function.

---

_Comment by @AlexWaygood on 2025-12-24 18:42_

This seems great, although on this branch ty now won't let me press enter after typing `@dataclass(frozen=True)` unless I want to auto-import `operator.truediv`

<img width="1488" height="722" alt="image" src="https://github.com/user-attachments/assets/b12fe478-5728-4b10-925c-5bd8e6efbca5" />

---

`True` also wasn't offered as a completion suggestion for `@dataclass(frozen=Tr<CURSOR>)`.

Both of these may be different issues, however...

---

_Comment by @MichaReiser on 2025-12-24 18:59_

I reverted the editor changes, which seems to fix it.

---

_Comment by @AlexWaygood on 2025-12-24 19:34_

> I reverted the editor changes, which seems to fix it.

Nice. `True` still isn't suggested as a completion suggestion inside `@dataclass(frozen=Tr<CURSOR>)` (in the playground or VSCode), and the `=` is back to not being rendered on the completion label in the playground, but I can now hit "enter" on the playground after `@dataclass(frozen=True)` without `operator.truediv` being auto-imported 😆 And that would have been _very_ annoying, so that seems like the most important thing

---

_@dhruvmanila reviewed on 2025-12-25 04:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2794 on 2025-12-25 04:15_

Can you add these as test cases here?

```py
@
def foo(): ...

@
class Foo: ...
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2917 on 2025-12-25 04:15_

Nice! This resolves point 14 in https://github.com/astral-sh/ruff/issues/10653

---

_@dhruvmanila approved on 2025-12-25 04:21_

---

_Comment by @RasmusNygren on 2025-12-25 12:17_

> > I reverted the editor changes, which seems to fix it.
> 
> Nice. `True` still isn't suggested as a completion suggestion inside `@dataclass(frozen=Tr<CURSOR>)` (in the playground or VSCode)

I believe this is unrelated and due to a bug in completions that causes suggestions inside decorator statements to be pruned a bit too aggressively. This can be reproduced on https://play.ty.dev/ 



---

_Merged by @MichaReiser on 2025-12-25 15:05_

---

_Closed by @MichaReiser on 2025-12-25 15:05_

---

_Branch deleted on 2025-12-25 15:05_

---
