```yaml
number: 21410
title: Fix panic when formatting comments in unary expressions
type: pull_request
state: closed
author: ntBre
labels:
  - bug
  - formatter
assignees: []
draft: true
base: main
head: brent/format-unary-comment
created_at: 2025-11-12T20:37:01Z
updated_at: 2025-11-18T15:48:38Z
url: https://github.com/astral-sh/ruff/pull/21410
synced_at: 2026-01-12T15:57:23Z
```

# Fix panic when formatting comments in unary expressions

---

_@ntBre_

Summary
--

This is a second attempt that fixes #19226 based on the feedback in https://github.com/astral-sh/ruff/pull/20494#issuecomment-3467920065.

We currently mark the comment in an expression like this:

```py
if '' and (not #
0):
    pass
```

as a leading comment on `not`, eventually causing a panic in
`Operand::has_unparenthesized_leading_comments` because the end of the comment
is greater than the start of the expression.

https://github.com/astral-sh/ruff/blob/a1d9cb5830eca9b63e7fb529504fc536e99bca23/crates/ruff_python_formatter/src/expression/binary_like.rs#L843

This PR fixes the issue by instead making such a comment a dangling comment on
the unary expression.

In the third commit, I instead tried making the comment a leading comment on the
operand, which also looks pretty reasonable to me. Making it a dangling comment
seems more in line with the docs on `handle_unary_op_comments`, though.

I also tried deleting the leading comment logic in favor of the new dangling
logic in the fifth commit before reverting in the sixth. This looks okay to me
too, but the current state of the PR seems like the least invasive fix.

Test Plan
--

A new, minimized test case based on the issue. I also checked that the original
snippet from the report works now.


---

_Label `bug` added by @ntBre on 2025-11-12 20:37_

---

_Label `formatter` added by @ntBre on 2025-11-12 20:37_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 20:47_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @ntBre on 2025-11-12 20:51_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-12 20:51_

---

_@amyreese approved on 2025-11-12 22:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1868 on 2025-11-13 07:53_

Can we add a test case for

```py
if '' and (not #
(0)
):
```

and


```py
if '' and (not 
	(  #
	0
)):
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1872 on 2025-11-13 07:55_

I think we now need to update the documentation of this method and explain in which situation it makes comments as dangling

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__unary.py.snap`:326 on 2025-11-13 08:01_

Can you double check that 

```py
if (  # comment
    not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Comment
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass
```

still formats the same before and after your PR, just to make sure we don't accidentially change the stable style

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__unary.py.snap`:268 on 2025-11-13 08:03_

I don't like this :) A comment between operator and the operand is confusing

---

_@MichaReiser approved on 2025-11-13 08:04_

---

_@ntBre reviewed on 2025-11-13 13:47_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1868 on 2025-11-13 13:47_

Well that immediately panicked again, thanks!

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1868 on 2025-11-13 14:54_

The second case is okay, it formats like this:

```py
if "" and (
    not (  #
        0
    )
):
    pass
```

but the first case is related to an existing test case:

```py
x = (
    # a
    not # b
    # c
    ( # d
        # e
        True
    )
)
```

which formats like this:

```py
x = (
    # a
    # b
    # c
    not (  # d
        # e
        True
    )
)
```

but this causes a panic on main:

```py
x = '' and (
    # a
    not # b
    # c
    ( # d
        # e
        True
    )
)
```

So I'm kind of wondering if adjusting the placement is really the right fix. It seems like we often want this kind of comment to become a leading comment to preserve the relative order with other comments and a change to the parent expression is what introduces the problem.

---

_@ntBre reviewed on 2025-11-13 14:54_

---

_@ntBre reviewed on 2025-11-13 15:15_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1868 on 2025-11-13 15:15_

I tried a different fix in `binary_like` in a separate branch: https://github.com/astral-sh/ruff/compare/main...brent/unary-comment2

I kind of like this because it feels more targeted and doesn't affect any existing snapshots, but it also feels a bit like just treating the symptom. However, it seems hard to differentiate the cases in my comment above otherwise.

---

_@ntBre reviewed on 2025-11-13 15:19_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1868 on 2025-11-13 15:19_

Hmm, I guess that branch is quite similar to #20494 though :sweat_smile: 

---

_Comment by @ntBre on 2025-11-14 19:47_

I played a bit more with this and chatted with Micha, and I think we should just patch this in the binary expression formatting. I think the closest I got to working something out was making the comment placement handling in `handle_unary_op_comment`:

https://github.com/astral-sh/ruff/blob/3d4b0559f1e7c2bb52276178f28e9e8df7f9d663/crates/ruff_python_formatter/src/comments/placement.rs#L1867-L1870

`CommentPlacement::dangling` and then partitioning these dangling comments into leading and trailing comments in the unary expression formatting proper:

https://github.com/astral-sh/ruff/blob/ddcff387084570ca2c3367eeb1d945448b3705a8/crates/ruff_python_formatter/src/expression/expr_unary_op.rs#L35-L40

But this causes issues with other parent expression formatting. For example, the `if` statement formatting needs to know if the condition expression has any leading (or trailing) comments so that it can parenthesize it, but if we make the comments dangling, it won't, and I was causing a syntax error in a case like this:

```py
if (
  not
  # comment
  a):
    pass
```

which was formatted to

```py
if 
# comment
not a:
    pass
```

I also temporarily added a check for leading comments that start after their expression in `CommentPlacement::leading`. Seeing that this is not the only case that violates the `TextRange` assumption in the binary expression formatting makes me feel slightly better about just adding a check to prevent the panic.

I still need to verify that the surrounding code makes sense after a change like in https://github.com/astral-sh/ruff/compare/main...brent/unary-comment2, but that's what I'm currently planning to pursue.

---

_Comment by @MichaReiser on 2025-11-14 20:28_

Regarding the `if` formatting, I assume that [`needs_parentheses`](https://github.com/astral-sh/ruff/blob/4f2606b0428c9ce0c2fb62b0958e6985a8323a57/crates/ruff_python_formatter/src/expression/expr_unary_op.rs#L73-L91) returns never. You'd have to change it to return `Multiline` if there's a leading operand comment

---

_Comment by @ntBre on 2025-11-14 21:20_

> Regarding the `if` formatting, I assume that [`needs_parentheses`](https://github.com/astral-sh/ruff/blob/4f2606b0428c9ce0c2fb62b0958e6985a8323a57/crates/ruff_python_formatter/src/expression/expr_unary_op.rs#L73-L91) returns never. You'd have to change it to return `Multiline` if there's a leading operand comment

Ooh, thank you! This is looking quite promising. I'm getting an extra newline now, but if I can fix that, this might work!

```py
      361   378 │     not a
      362   379 │ ):
      363   380 │     pass
      364   381 │ 
      365       │-if (  # comment
            382 │+if (
            383 │+    # comment
      366   384 │     not a
      367   385 │ ):
      368   386 │     pass
      369   387 │ 
```

---

_Comment by @ntBre on 2025-11-14 22:48_

Tracking down that newline turned out to be quite the adventure. I think this is another case where things "just work" if the comments on the unary op are leading but would require additional special-casing as dangling comments. I'm pushing up my changes from the other branch with a bit of refactoring and some documentation, but I'm happy to revisit if I'm still missing something.

---

_Comment by @MichaReiser on 2025-11-14 22:53_

> Tracking down that newline turned out to be quite the adventure. I think this is another case where things "just work" if the comments on the unary op are leading but would require additional special-casing as dangling comments. I'm pushing up my changes from the other branch with a bit of refactoring and some documentation, but I'm happy to revisit if I'm still missing something.

Can you share an example of where it goes wrong?

---

_Comment by @ntBre on 2025-11-14 23:01_

These cases all have their comments moved from end of line to own-line comments, as in the diff above:

```py
if (
    not # comment
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass

if (
  not  # comment
  a):
    pass

if '' and (not #
0):
    pass

if '' and (not #
(0)
):
    pass

if '' and (not
	(  #
	0
)):
    pass
```

I tracked the issue into `soft_block_indent` called from here in the if header formatting:

https://github.com/astral-sh/ruff/blob/d63b4b038364f3befe46f133251fc11973feff21/crates/ruff_python_formatter/src/expression/parentheses.rs#L185

triggered by not having leading comments here:

https://github.com/astral-sh/ruff/blob/d63b4b038364f3befe46f133251fc11973feff21/crates/ruff_python_formatter/src/expression/mod.rs#L127-L130

And I was playing with this approach on yet another branch full of debug prints, in case you want to try it: https://github.com/astral-sh/ruff/compare/main...brent/unary-3

---

_Comment by @MichaReiser on 2025-11-15 17:59_

I'm not sure I follow your latest changes. Pasting your snippet shows that all these comments are places as leading unary expression comments and not dangling comments. Is this intentional? You can see this when opening the comments tab in the playground

---

_Comment by @ntBre on 2025-11-15 18:11_

Yes that's intentional. I reverted my earlier changes where I was trying to make them dangling comments, restored them to leading comments like on `main`, and then worked around the panic in the binary formatting instead.

Basically I gave up on the initial approach, but I'm still happy to keep trying if we come up with something better.

---

_Comment by @MichaReiser on 2025-11-15 18:32_

Oh I see. I don't think I understand the changes you made in that branch. What I'd expected:

* Why are we now formatting the trailing comments *before* the operator?
* We need to insert a hard line break if there are any leading comments (before the operator)
* No changes should be needed in the expression formatting

Happy to jump on a pair programming on this but I think we should maybe start fresh in the unary expression formatting and try to make small incremental changes.

---

_Comment by @ntBre on 2025-11-15 19:01_

> * Why are we now formatting the trailing comments _before_ the operator?

I thought this is what we wanted to avoid splitting the operator and the operand?

> * We need to insert a hard line break if there are any leading comments (before the operator)

If I'm thinking of the same hard line break, this would change our stable formatting. That's what I meant about the newline in my comments above. But maybe you're thinking of a different one.

> * No changes should be needed in the expression formatting
> 
> Happy to jump on a pair programming on this but I think we should maybe start fresh in the unary expression formatting and try to make small incremental changes.

That sounds good! The other branch was my attempt to start fresh, but I'm sure I missed a simpler option along the way. I'll convert this back to a draft for now.


---

_Converted to draft by @ntBre on 2025-11-15 19:01_

---

_Comment by @ntBre on 2025-11-18 15:48_

Closing in favor of #21501.

---

_Closed by @ntBre on 2025-11-18 15:48_

---
