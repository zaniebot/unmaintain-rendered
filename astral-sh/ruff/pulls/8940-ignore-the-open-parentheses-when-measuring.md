```yaml
number: 8940
title: "Ignore the open parentheses when measuring `BestFitsParenthesize`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
base: main
head: best-fit-content-only
created_at: 2023-12-01T04:00:43Z
updated_at: 2025-02-20T09:00:32Z
url: https://github.com/astral-sh/ruff/pull/8940
synced_at: 2026-01-10T19:57:22Z
```

# Ignore the open parentheses when measuring `BestFitsParenthesize`

---

_Pull request opened by @MichaReiser on 2023-12-01 04:00_

## Summary

This PR changes the definition of `BestFitParenthesize` to only consider whether its content fits when parenthesizing and not whether the whole parenthesized expression fits. 

The difference is subtle but relevant when what comes before `BestFitParenthesize` already exceeds the configured line width:

```python
this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = aaaaaa
```

The `this_is_a...` exceeds the configured line width. The existing implementation doesn't parenthesize `aaaaa` because the opening `(` doesn't fit on the line. This is unfortunate, because parenthesizing would make `aaaaa` fit in the configured line width and is what Black does:

```python
this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = (
    aaaaaa
)
```

This PR implements Black's behavior by ignoring whether there's sufficient space to place the `(` and instead only measures whether its content (`aaaaaa`) fits when indenting. 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

The similarity index remains unchanged.

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               213 |
| poetry         |           0.96208 |               317 |                35 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99957 |              1459 |                36 |



---

_Comment by @MichaReiser on 2023-12-01 04:00_

Current dependencies on/for this PR:
* main
  * **PR #8920** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8943** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #9102** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8941** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8940** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment).

---

_Label `formatter` added by @MichaReiser on 2023-12-01 04:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__comments6.py.snap`:139 on 2023-12-01 04:05_

The style here is different because black doesn't inline the `type: ` comment. Changing the comment to e.g. `ctype` matches Ruff's formatting

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep604_union_types_line_breaks.py.snap`:126 on 2023-12-01 04:05_

Requires other preview styles to match black

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_cantfit.py.snap`:58 on 2023-12-01 04:06_

Requires other preview styles to match Black 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__yield.py.snap`:168 on 2023-12-01 04:08_

This doesn't match black although black's formatting here is very peculiar 

```python
for ridiculouslylongelementnameeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee in (l):
    pass

```

It ends parenthesizing `l` but without splitting the line. IMO, both styles are not ideal but I prefer ours for consistency.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__type_alias.py.snap`:122 on 2023-12-01 04:09_

This is a divergence from Black. Black doesn't parenthesize the right side. I don't see why and recommend keeping it because I don't see why type aliases should be different from other assignments.

---

_@MichaReiser reviewed on 2023-12-01 04:09_

---

_Comment by @github-actions[bot] on 2023-12-01 04:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+4 -2 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+4 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/latchbio/latch/blob/965a74f9c4aaefc1f8cb33b7113e429535f5fb8a/latch/idl/core/interface.py#L58'>latch/idl/core/interface.py~L58</a>
```diff
     var: Variable
     """+required Variable. Defines the type of the variable backing this parameter."""
 
-    behavior: "Optional[typing.Union[ParameterBehaviorDefault, ParameterBehaviorRequired]]" = None
+    behavior: "Optional[typing.Union[ParameterBehaviorDefault, ParameterBehaviorRequired]]" = (
+        None
+    )
 
     def to_idl(self) -> pb.Parameter:
         return merged_pb(pb.Parameter(var=self.var.to_idl()), self.behavior)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+4 -2 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/965a74f9c4aaefc1f8cb33b7113e429535f5fb8a/latch/idl/core/interface.py#L58'>latch/idl/core/interface.py~L58</a>
```diff
     var: Variable
     """+required Variable. Defines the type of the variable backing this parameter."""
 
-    behavior: "Optional[typing.Union[ParameterBehaviorDefault, ParameterBehaviorRequired]]" = None
+    behavior: "Optional[typing.Union[ParameterBehaviorDefault, ParameterBehaviorRequired]]" = (
+        None
+    )
 
     def to_idl(self) -> pb.Parameter:
         return merged_pb(pb.Parameter(var=self.var.to_idl()), self.behavior)
```

</p>
</details>




---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-01 04:25_

---

_Marked ready for review by @MichaReiser on 2023-12-01 04:26_

---

_Comment by @MichaReiser on 2023-12-01 04:56_

Hmm, obviously, it is more complicated than this 🤯 

Black does not parenthesize the right side here (for reasons?)

```python
this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = (
    function(arg1, arg2, arg3)
)
```

Although `function` is clearly over the configured width and parenthesizing helps make it fit. Instead, black picks

```python
this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = function(
    arg1, arg2, arg3
)
```

Ultimately, the vertical spacing is about the same, although Black's layout requires considerably more vertical spacing. However, it avoids one pair of parentheses, which is something Black generally optimizes for.

Which makes me wonder what their rule is. 


---

_Comment by @MichaReiser on 2023-12-01 06:54_

Okay this is interesting. Black does the exact opposite if the right hand side is a function

```python
this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_i = (
    function([1, 2, 3], arg1, [1, 2, 3], arg2, [1, 2, 3], arg3)
)

this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = function(
    [1, 2, 3], arg1, [1, 2, 3], arg2, [1, 2, 3], arg3
)
```

The `(` fits for the first example, which is why Black parenthesizes the entire function (at least it seems). The `(` doesn't fit for the second example, which is why Black avoids parenthesizing the function. 

I compared the similarity index for Poetry using either layout, and it remained unchanged. This feels edge-casey enough that I feel it isn't worth spending too much time on. However, I'm undecided which version I prefer:

* Always parenthesizing if it makes the content fit gets the result closer to the ultimate goal of fitting code into the configured line width. It's also closer to how our formatter works overall, which generally eagerly parenthesizes expressions. For example, this gets always parenthesized `this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = a + b` 
* Avoiding the parentheses if even the `(` doesn't fit results in fewer parentheses overall, which is a secondary goal. It can be slightly more readable if you e.g. have

  ```python
  this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = function(
      [1, 2, 3],
      arg1,
      [1, 2, 3],
      arg2,
      [1, 2, 3],
      arg3,
      dddddddddddddddddddd,
      eeeeeeeeeeeeee,
  )
  # instead of
  this_is_a_ridiculously_long_name_and_nobody_in_their_right_mind_would_use_one_like_it = (
      function(
          [1, 2, 3],
          arg1,
          [1, 2, 3],
          arg2,
          [1, 2, 3],
          arg3,
          dddddddddddddddddddd,
          eeeeeeeeeeeeee,
      )
  )
  ```

Would love to hear some more opinions on this

---

_@charliermarsh approved on 2023-12-01 18:30_

---

_Closed by @MichaReiser on 2023-12-14 03:08_

---

_Branch deleted on 2024-01-16 10:03_

---

_Branch restored on 2024-06-10 08:17_

---

_Branch deleted on 2025-02-20 09:00_

---

_Branch restored on 2025-02-20 09:00_

---
