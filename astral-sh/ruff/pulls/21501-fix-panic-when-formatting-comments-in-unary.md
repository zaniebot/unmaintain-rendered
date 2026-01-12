```yaml
number: 21501
title: Fix panic when formatting comments in unary expressions
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: brent/unary-3
created_at: 2025-11-17T16:15:19Z
updated_at: 2025-11-18T15:48:17Z
url: https://github.com/astral-sh/ruff/pull/21501
synced_at: 2026-01-12T15:57:26Z
```

# Fix panic when formatting comments in unary expressions

---

_@ntBre_

## Summary

This is another attempt at https://github.com/astral-sh/ruff/pull/21410 that fixes https://github.com/astral-sh/ruff/issues/19226.

@MichaReiser helped me get something working in a very helpful pairing session. I pushed one additional commit moving the comments back from leading comments to trailing comments, which I think retains more of the input formatting.

I was inspired by Dylan's PR (#21185) to make one of these tables:

<table>
                <thead>
                    <tr>
                    <th scope="col">Input</th>
                    <th scope="col">Main</th>
                    <th scope="col">PR</th>
                    </tr>
                </thead>
                <tbody>
<tr>
<td><pre lang="python">
if (
    not
    # comment
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass
</pre></td>
<td><pre lang="python">
if (
    # comment
    not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass

</pre></td>
<td><pre lang="python">
if (
    not
    # comment
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass

</pre></td>
</tr>
<tr>
<td><pre lang="python">
if (
    # unary comment
    not
    # operand comment
    (
        # comment
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
        + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    )
):
    pass
</pre></td>
<td><pre lang="python">
if (
    # unary comment
    # operand comment
    not (
        # comment
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
        + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    )
):
    pass

</pre></td>
<td><pre lang="python">
if (
    # unary comment
    not
    # operand comment
    (
        # comment
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
        + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    )
):
    pass

</pre></td>
</tr>
<tr>
<td><pre lang="python">
if (
    not # comment
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass
</pre></td>
<td><pre lang="python">
if (  # comment
    not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass

</pre></td>
<td><pre lang="python">
if (
    not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa  # comment
    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
):
    pass

</pre></td>
</tr>
</tbody>
            </table>

hopefully it helps even though the snippets are much wider here.

The two main differences are (1) that we now retain own-line comments between the unary operator and its operand instead of moving these to leading comments on the operator itself, and (2) that we move end-of-line comments between the operator and operand to dangling end-of-line comments on the operand (the last example in the table).

## Test Plan

Existing tests, plus new ones based on the issue. As I noted below, I also ran the output from main on the unary.py file back through this branch to check that we don't reformat code from main. This made me feel a bit better about not preview-gating the changes in this PR.

```shell
> git show main:crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/unary.py | ruff format - | ./target/debug/ruff format --diff -
> echo $?
0
```

---

_Label `bug` added by @ntBre on 2025-11-17 16:15_

---

_Label `formatter` added by @ntBre on 2025-11-17 16:15_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 16:22_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@ntBre reviewed on 2025-11-17 16:26_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@parentheses__expression_parentheses_comments.py.snap`:200 on 2025-11-17 16:26_

Not sure about this one. 

Input:

```py
x = (
    # 1
    not # 2
    ( # 3
        # 4
        "d"
    )
)
```

Old output:

```py
x = (
    # 1
    # 2
    not (  # 3
        # 4
        "d"
    )
)
```

and before the last commit it was like this:

```py
x = (
    # 1
    not
    # 2
    (  # 3
        # 4
        "d"
    ),
)
```

which I think might be closer to what we want, although `# 2` is an end-of-line comment in the input. That's what motivated the last commit, preserving the end-of-line status of the comments.

---

_Marked ready for review by @ntBre on 2025-11-17 16:28_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-17 16:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@parentheses__expression_parentheses_comments.py.snap`:200 on 2025-11-17 16:31_

I like the new output.



---

_@MichaReiser reviewed on 2025-11-17 16:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1911 on 2025-11-17 16:32_

Can you update the method description to match our new behavior

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:75 on 2025-11-17 16:32_

Let's assign the leading commnts to a variable to avoid retrieving them twice 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:98 on 2025-11-17 16:33_

Do we need to change the logic here too to match the logic for when we insert a hard line break in the unary formatting?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:122 on 2025-11-17 16:35_

I think we can simplify this to returning `Multiline` when there's any dangling comment. 

Does this need to take precedence over the `Never` case when the `operand` is parenthesized?

---

_@MichaReiser reviewed on 2025-11-17 16:36_

I haven't thought it through but does this change require preview gating? 

I think what will help us to make this decision is if you update your summary and describe how the formatting, specifically the comment placement, changes compared to main.

---

_@ntBre reviewed on 2025-11-17 17:00_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:98 on 2025-11-17 17:00_

Do you mean something like this?

```rust
        if !context.comments().has_leading(self.operand.as_ref())
            || is_expression_parenthesized(
                self.operand.as_ref().into(),
                context.comments().ranges(),
                context.source(),
            )
        {
            return OptionalParentheses::Never;
        }
```

I played with a few variations on this and kept running into instabilities. It seems to be working okay without matching the check exactly, like on main.

---

_@ntBre reviewed on 2025-11-17 17:04_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:122 on 2025-11-17 17:04_

It seems to work in both orders, at least with our current tests.

---

_Comment by @ntBre on 2025-11-17 17:09_

> I haven't thought it through but does this change require preview gating?
> 
> I think what will help us to make this decision is if you update your summary and describe how the formatting, specifically the comment placement, changes compared to main.

I did think a little bit about this. I think it would be a bit difficult to preview-gate this since the panic fix and the new formatting seem intertwined.

I verified that taking the formatted output from `main` (the `## Output` section from the `unary.py` snapshot) and running it against this branch shows no changes. So we won't change any code that we previously formatted, at least.

I will also expand the summary, though!

---

_Review requested from @MichaReiser by @ntBre on 2025-11-17 19:55_

---

_@MichaReiser reviewed on 2025-11-18 09:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:98 on 2025-11-18 09:40_

No, more like this:


```
        let parenthesized_operand_range = parenthesized_range(
            operand.into(),
            item.into(),
            comments.ranges(),
            f.context().source(),
        );
        let leading_operand_comments = comments.leading(operand.as_ref());
        let has_leading_comments_before_parens = parenthesized_operand_range.is_some_and(|range| {
            leading_operand_comments
                .iter()
                .any(|comment| comment.start() < range.start())
        });
        if !leading_operand_comments.is_empty()
            && !is_expression_parenthesized(
                operand.as_ref().into(),
                f.context().comments().ranges(),
                f.context().source(),
            )
            || has_leading_comments_before_parens
```

It's important that it exactly mirrors the case when we insert a hard line break in the formatting code because **any** line break will lead to invalid syntax if the `if` formatting doesn't add parentheses. 

Here's an example where your PR produces invalid syntax:

```py
if (
  not  
  # comment
  (a)):
    pass
```

We should add more tests that exercise the new leading comment placement (may even be true for the trailing comment placement, are there more combinations that you could test?)

---

_Comment by @MichaReiser on 2025-11-18 09:43_

Thanks for updating the summary. I think it should be safe to not preview gate this change because:

* It changes the placement of trailing operator comments, but main always moved trailing operator comments off the operator. 
* It changes the placement of leading operator comments, but main always made leading operator comments leading unary comments.

That means, any ruff formatted code can't contain any comment for which we now preserve the position

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:90 on 2025-11-18 15:08_

I think we can even return `Always` here because we know it breaks over multiple lines and will need parentheses

---

_@MichaReiser approved on 2025-11-18 15:09_

Thank you

---

_@ntBre reviewed on 2025-11-18 15:29_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_unary_op.rs`:90 on 2025-11-18 15:29_

Ah right, thanks! And thanks for all of your help here!

---

_Merged by @ntBre on 2025-11-18 15:48_

---

_Closed by @ntBre on 2025-11-18 15:48_

---

_Branch deleted on 2025-11-18 15:48_

---
