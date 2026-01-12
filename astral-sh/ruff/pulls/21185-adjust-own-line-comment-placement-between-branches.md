```yaml
number: 21185
title: Adjust own-line comment placement between branches
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: comment-placement-else-finally
created_at: 2025-11-01T16:10:56Z
updated_at: 2025-11-17T13:30:35Z
url: https://github.com/astral-sh/ruff/pull/21185
synced_at: 2026-01-12T15:57:18Z
```

# Adjust own-line comment placement between branches

---

_@dylwil3_

This PR attempts to improve the placement of own-line comments between branches in the setting where the comment is more indented than the preceding node.

There are two main changes.

### First change: Preceding node has leading content

If the preceding node has leading content, we now regard the comment as automatically _less_ indented than the preceding node, and format accordingly.

For example, 

```python
if True: preceding_node
# leading on `else`, not trailing on `preceding_node`
else: ...
```

This is more compatible with `black`, although there is a (presumably very uncommon) edge case:

```python
if True:
    this;that
    # leading on `else`, but trailing in `black`
else: ...
```

I'm sort of okay with this - presumably if one wanted a comment for those semi-colon separated statements, one should have put it _above_ them, and one wanted a comment only for `that` then it ought to have been on the same line?

### Second change: searching for last child in body

While searching for the (recursively) last child in the body of the preceding _branch_, we implicitly assumed that the preceding node had to have a body to begin the recursion. But actually, in the base case, the preceding node _is_ the last child in the body of the preceding branch. So, for example:

```python
if True:
    something
    last_child_but_no_body
    # leading on else for `main` but trailing in this PR
else: ...
```

### More examples

The table below is an attempt to summarize the changes in behavior. The rows alternate between an example snippet with `while` and the same example with `if` - in the former case we do _not_ have an `else` node and in the latter we do.

Notice that:

1. On `main` our handling of `if` vs. `while` is not consistent, whereas it is consistent in the present PR
2. We disagree with `black` in all cases except that last example on `main`, but agree in all cases for the present PR (though see above for a wonky edge case where we disagree).

<table>
<tr>
<th>Original &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th>
<th><code>main</code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th>
<th>This PR&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th>
<th><code>black</code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th>
</tr>
<tr>
<td>

<pre lang="python">
while True: 
    pass
        # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
else:
    # comment
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
    # comment
else:
    pass
</pre>

</td>
</tr>
<tr>
<td>

<pre lang="python">
if True:
    pass
        # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
    # comment
else:
    pass
</pre>

</td>
</tr>
<tr>
<td>

<pre lang="python">
while True: pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
# comment
else:
    pass
</pre>

</td>
</tr>
<tr>
<td>

<pre lang="python">
if True: pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
</tr>
<tr>
<td>

<pre lang="python">
while True: pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
else:
    # comment
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
while True:
    pass
# comment
else:
    pass
</pre>

</td>
</tr>
<tr>
<td>

<pre lang="python">
if True: pass
    # comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
<td>

<pre lang="python">
if True:
    pass
# comment
else:
    pass
</pre>

</td>
</tr>
</table>

---

_Label `bug` added by @dylwil3 on 2025-11-01 16:10_

---

_Label `formatter` added by @dylwil3 on 2025-11-01 16:10_

---

_Comment by @github-actions[bot] on 2025-11-01 16:22_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Renamed from "Adjust comment placement before `else`/`finally` clauses" to "Adjust indented comment placement before `else`/`finally` clauses" by @dylwil3 on 2025-11-10 16:23_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/comments/placement.rs`:580 on 2025-11-10 21:32_

Note that `max_empty_lines` also uses a `SimpleTokenizer`. We could combine that into a single pass here, I think, but it would be less readable. Is it worth it?

---

_@dylwil3 reviewed on 2025-11-10 21:32_

---

_Marked ready for review by @dylwil3 on 2025-11-10 21:51_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-11-10 21:51_

---

_@MichaReiser reviewed on 2025-11-11 10:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:572 on 2025-11-11 10:54_

Can you say more why we're handling this case here? I'd expect this to be handled by `handle_own_line_comment_between_branches` 

---

_Renamed from "Adjust indented comment placement before `else`/`finally` clauses" to "Adjust comment placement between branches" by @dylwil3 on 2025-11-11 16:24_

---

_Renamed from "Adjust comment placement between branches" to "Adjust own-line comment placement between branches" by @dylwil3 on 2025-11-11 16:26_

---

_@dylwil3 reviewed on 2025-11-11 16:53_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/comments/placement.rs`:572 on 2025-11-11 16:53_

After our offline discussion I've tried a hopefully more reasonable spot in the logic for the fix

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:626 on 2025-11-11 18:01_

What's the `+1` for? I like to use `char.text_len`() (e.g. `' '.text_len()`) to more clearly what character we're skipping.

Is it important what value we use for `comment_indentation` or can it be any value for as long as it is larger than `comment_indentation` (so that we hit the `Less` case?)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__while.py.snap`:39 on 2025-11-11 18:04_

I often find it useful to number the comments. It makes it easier to match the input/output (and debug panics if we forgot to format a comment)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:726 on 2025-11-11 18:08_

This seems risky. There isn't really anything enforcing that we're within a block statement. I think we, at least, want to assert that the enclosing node is a block statement.

---

_@MichaReiser requested changes on 2025-11-11 18:10_

This looks much better to me and it's nice that this matches black in almost all cases.

However, there are a couple of tests that now fail because of instable formatting (the invariant `format(source) == format(format(source))` doesn't hold anymore). I think it's because the fallback to `preceding` is too liberal.

---

_@dylwil3 reviewed on 2025-11-11 20:43_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/format@statement__while.py.snap`:39 on 2025-11-11 20:43_

Here I didn't do that because there is only one comment in each example - do you want me to the number them across the whole fixture? Among these examples? Or just make them all `#1`?

---

_@dylwil3 reviewed on 2025-11-11 20:44_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/comments/placement.rs`:626 on 2025-11-11 20:44_

We are just trying to hit the `Less` case - it can be any positive number. I've clarified with a comment.

---

_Comment by @dylwil3 on 2025-11-12 00:25_

Sorry about the failing tests! I made the beginner mistake of running tests, fixing snapshots, but then not re-running tests after that. Whoops!

---

_Review requested from @MichaReiser by @dylwil3 on 2025-11-14 18:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:743 on 2025-11-15 18:13_

Could we use `if following.is_first_statement_in_body()` or even `is_first_statement_in_alternate_body`

I'm surprised that we need `MatchCase` and `ExceptHandler` here, as those nodes don't have alternate branches? How did you pick the nodes listed here? Did you add tests demonstrating that they're all necessary?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__while.py.snap`:39 on 2025-11-15 18:16_

I would give them unique numbers within the `# Compare behavior with `while`/`else` comment placement` section

---

_@MichaReiser approved on 2025-11-15 18:22_

Thank you. This overall looks good. I just don't feel 100 confident about the new `Non` if `matches` case. Did you pick them individually or did you list all compound statements just to be sure? If it's the latter, than I'd prefer to only pick the once that are indeed necessary. 

---

_@dylwil3 reviewed on 2025-11-17 13:20_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/comments/placement.rs`:743 on 2025-11-17 13:20_

Yes, sorry - I added all of those because that was my interpretation of

> There isn't really anything enforcing that we're within a block statement. I think we, at least, want to assert that the enclosing node is a block statement.

But you're right that `is_first_statement_in_alternate_body` is more targeted, so I've changed it to that, thanks!

---

_Merged by @dylwil3 on 2025-11-17 13:30_

---

_Closed by @dylwil3 on 2025-11-17 13:30_

---
