```yaml
number: 13207
title: "Implement `AstNode` for `Identifier`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/identifier-ast-node
created_at: 2024-09-02T09:12:22Z
updated_at: 2024-09-17T10:12:14Z
url: https://github.com/astral-sh/ruff/pull/13207
synced_at: 2026-01-12T15:55:43Z
```

# Implement `AstNode` for `Identifier`

---

_@dhruvmanila_

## Summary

Follow-up to #13147, this PR implements the `AstNode` for `Identifier`. This makes it easier to create the `NodeKey` in red knot because it uses a generic method to construct the key from `AnyNodeRef` and is important for definitions that are created only on identifiers instead of `ExprName`.

## Test Plan

`cargo test` and `cargo clippy`


---

_Label `internal` added by @dhruvmanila on 2024-09-02 09:12_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-02 09:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-02 09:12_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-09-02 09:12_

---

_@AlexWaygood reviewed on 2024-09-02 09:31_

Looks reasonable to me! @MichaReiser is probably the better reviewer on this, though

---

_Comment by @github-actions[bot] on 2024-09-02 09:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>




---

_@MichaReiser approved on 2024-09-02 10:20_

---

_Merged by @dhruvmanila on 2024-09-02 10:57_

---

_Closed by @dhruvmanila on 2024-09-02 10:57_

---

_Branch deleted on 2024-09-02 10:57_

---

_Comment by @carljm on 2024-09-03 17:46_

Nice! Does this also mean that we'd now be able to just store the identifier directly as an `AstNodeRef` in the `MatchPatternDefinitionKind`, instead of storing an index? If so, that would make it more similar to assignment statements, which might be nice when we implement unpacking.

Or it may turn out that indices are nicer to work with than the node references, in which case we'd rather change assignments to use indices instead :) So maybe this is just something to consider when working on actually implementing unpacking assignment.

---

_Comment by @dhruvmanila on 2024-09-03 18:03_

> Nice! Does this also mean that we'd now be able to just store the identifier directly as an `AstNodeRef` in the `MatchPatternDefinitionKind`, instead of storing an index? If so, that would make it more similar to assignment statements, which might be nice when we implement unpacking.

Yeah, although I think index might still be better to identify the type. This provides us the position of the identifier in the pattern, then we'll kinda zip through the subject / pattern to perform structural matching until we reach the index position. Without the index, we will have to compare each identifier in the pattern to determine the one that we're looking to determine the type for. Does this make sense? I can provide an example as well.

---

_Comment by @carljm on 2024-09-03 18:17_

> Without the index, we will have to compare each identifier in the pattern to determine the one that we're looking to determine the type for

It does make sense; that's what I was thinking of when I said "it may turn out indices are nicer to work with." My main thought is that when we do unpacking, we should probably settle on a preference for either indices or node refs for these unpacking targets, and make them all work as similarly as possible (whichever way we decide is best), because I don't see a strong rationale for them to be different.

---

_Comment by @MichaReiser on 2024-09-17 10:12_

The part that we forgot to do here is to start visiting `Identifier` in nodes that store an identifier, for example `Parameter`. Although I think it's good that we forgot to do so because that would be a breaking change for the formatter where we suddenly would start associating comments with identifiers.

---
