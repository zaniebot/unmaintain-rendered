```yaml
number: 14388
title: "[red-knot] Do not pull types for subexpressions of annotations"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
base: main
head: david/annotation-subexpressions
created_at: 2024-11-16T20:48:57Z
updated_at: 2024-11-18T08:46:22Z
url: https://github.com/astral-sh/ruff/pull/14388
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Do not pull types for subexpressions of annotations

---

_Pull request opened by @sharkdp on 2024-11-16 20:48_

## Summary

As you can see from the drastically reduced `KNOWN_FAILURES` list, a lot of the remaining panics in the corpus test are caused by pulling types for every subexpression of a type annotation. Consider for example the type annotation `tuple[int, ...]`. If we insist on being able to infer types for each and every subexpression of annotations, then we also need to infer a type for the ellipsis.

In the current changeset, I simply stop the `PullTypesVisitor` from descending further into annotation expressions. This disables the check even for annotations like `int | str` where we can — and do — assign correct types to subexpressions.

I am aware that this might not be the right fix. I'm opening the PR to discuss how to proceed here. Two alternative approaches that I can think of:

1. Adapt type inference to infer types for each and every subexpressions of annotations. For the ellipsis in the example above, we would perhaps infer `Type::Unknown` (or maybe `tuple[int, …]`, somewhat recursively?).
1. Adapt type inference to infer types for *most* subexpressions of annotations, with certain exceptions. We adapt the `PullTypes` visitor to ignore those exceptional expressions.

## Test Plan

Ran corpus test with reduced known-failures list.

---

_Label `red-knot` added by @sharkdp on 2024-11-16 20:48_

---

_Review requested from @carljm by @sharkdp on 2024-11-16 20:48_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-16 20:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-16 20:48_

---

_Comment by @codspeed-hq[bot] on 2024-11-16 20:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david/annotation-subexpressions)

### Merging #14388 will **improve performances by 5.31%**

<sub>Comparing <code>david/annotation-subexpressions</code> (211ac75) with <code>main</code> (4dcb7dd)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `david/annotation-subexpressions` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 936.3 µs | 889.1 µs | +5.31% |


---

_Comment by @github-actions[bot] on 2024-11-16 21:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-11-16 21:13_

What's the reason that we don't infer types for annotations? Is it just because we don't do it yet or is it because it's unclear what type to infer for expressions in annotations? 

If it's technically possible and inferring types for annotations is just something we aren't doing yet, then I'd rather acknowledge the shortcoming by using `KNOWN_FAILURES`. Mainly because the long-term goal is that the linter uses the same API as the test, and it should be possible to query the type of every expression.

---

_Comment by @sharkdp on 2024-11-16 21:27_

> What's the reason that we don't infer types for annotations? Is it just because we don't do it yet or is it because it's unclear what type to infer for expressions in annotations?

I don't know. This is what I tried to discuss in the PR description.

> If it's technically possible and inferring types for annotations is just something we aren't doing yet, then I'd rather acknowledge the shortcoming by using `KNOWN_FAILURES`.

I would assume it's technically possible. It's just not clear to me what something like the ellipsis in a `tuple[int, ...]` annotation would have a as a type?

I'm definitely not trying to get rid of entries in `KNOWN_FAILURES` if there is an actual known limitation. I am trying to solve it.

It looks to me like pyright/pylance go with alternative approach no. 2, as you get "mouse over" types for the `tuple` and `int` sub-expressions, but nothing for the ellipsis.

---

_Comment by @MichaReiser on 2024-11-16 21:32_

For the case 2. In that case it would probably be best to change the `HasTy` interface to return `Option<Type>`

---

_Comment by @InSyncWithFoo on 2024-11-17 08:16_

Theoretically, I would say the first approach is the correct one (though the type should be `*tuple[int, ...]`, to be exact). However, the average user is likely to be confused by it, so here's another approach to consider: Infer the type internally but don't show it to the user, or only show conditionally.

---

_Comment by @sharkdp on 2024-11-17 18:35_

I think (1) is probably indeed the best approach. Closing this for now.

---

_Closed by @sharkdp on 2024-11-17 18:35_

---

_Comment by @sharkdp on 2024-11-18 08:46_

See follow up: https://github.com/astral-sh/ruff/pull/14426 

---
