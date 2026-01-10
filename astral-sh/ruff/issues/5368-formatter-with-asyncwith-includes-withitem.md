```yaml
number: 5368
title: "Formatter: `With` / `AsyncWith` (includes `WithItem`)"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-06-26T14:10:17Z
updated_at: 2023-07-15T15:03:10Z
url: https://github.com/astral-sh/ruff/issues/5368
synced_at: 2026-01-10T11:09:47Z
```

# Formatter: `With` / `AsyncWith` (includes `WithItem`)

---

_Issue opened by @MichaReiser on 2023-06-26 14:10_

* [x] `With` https://github.com/astral-sh/ruff/pull/5350
* [x] `AsyncWith`
* [ ] Parentheses in `WithItem`

---

_Comment by @MichaReiser on 2023-06-26 14:11_

`With` has been implemented by https://github.com/astral-sh/ruff/pull/5350

@davidszotten do you want to look into/plan to work on generalizing the logic by either defining an union enum or a trait for `AsyncWith`?

---

_Label `formatter` added by @MichaReiser on 2023-06-26 14:11_

---

_Comment by @davidszotten on 2023-06-26 14:58_

sure, will take a look

---

_Assigned to @davidszotten by @MichaReiser on 2023-06-26 15:01_

---

_Comment by @MichaReiser on 2023-06-26 15:10_

I'm not sure how to handle this inside of the formatter but we're currently adding to many parentheses for 

```python
# Input
with (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb) as c:
    ...

# Ruff Output
with (
	(
		aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
	) as c,
):
	...

# Black
with (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
) as c:
    ...

```

Because both the binary expression and the `with` statement are adding parentheses. It may be necessary to explicitly enumerate all the expressions that support breaking the left side, similar to `can_break_expr` OR rethink how we group elements, potentially use `best_fitting`.

Other interesting patterns

```python
with (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb) as c, (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb) as c:
    ...

with (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb), (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb):
    ...

with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as c, aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as c:
    ...
```

We don't need to match black precisely but I do prefer black's formatting overall.

We can tackle this in a separate PR and I can also look into it

---

_Comment by @davidszotten on 2023-06-26 18:39_

i was trying to follow the code to understand how `if_group_breaks` works (but failed). does the outer group also consider itself broken if the inner one does? or what is causing the outer group to expand?

---

_Comment by @MichaReiser on 2023-06-26 19:57_

> i was trying to follow the code to understand how `if_group_breaks` works (but failed). does the outer group also consider itself broken if the inner one does? or what is causing the outer group to expand?

That's correct. An outer group always expands if an inner group expands. I'm on my phone but there's a propagate_expands that implements this behavior. 

A sequence of groups expands the groups from right to left

Best fitting allows you to provide multiple versions of the same content and the printer tries to pick the first one that fits (or falls back to an expanded version)

If group breaks accepts an optional with_group. It makes the content dependent on whatever the group with that given id expands rather than the enclosing group. However, the referred group must either be a parent or precede the content. 

I think that summarizes most grouping IR elements.

In this case, it may be necessary that we enumerate all expressions for which we don't want parantheses. 

---

_Comment by @davidszotten on 2023-06-26 20:13_

ok. might leave this for you (separate pr seems ok)

---

_Closed by @MichaReiser on 2023-07-15 15:03_

---
