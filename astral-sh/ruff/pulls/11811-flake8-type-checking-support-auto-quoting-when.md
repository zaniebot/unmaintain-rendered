```yaml
number: 11811
title: "[`flake8-type-checking`] Support auto-quoting when annotations contain quotes"
type: pull_request
state: merged
author: Glyphack
labels:
  - rule
assignees: []
merged: true
base: main
head: auto-quote-annotation-quotes
created_at: 2024-06-09T22:17:46Z
updated_at: 2024-10-23T11:28:37Z
url: https://github.com/astral-sh/ruff/pull/11811
synced_at: 2026-01-12T15:55:39Z
```

# [`flake8-type-checking`] Support auto-quoting when annotations contain quotes

---

_@Glyphack_

## Summary

This PR updates the fix generation logic for auto-quoting an annotation to generate an edit even when there's a quote character present.

The logic uses the visitor pattern, maintaining it's state on where it is and generating the string value one node at a time. This can be considered as a specialized form of `Generator`. The state required to maintain is whether we're currently inside a `typing.Literal` or `typing.Annotated` because the string value in those types should not be un-quoted i.e., `Generic[Literal["int"]]` should become `"Generic[Literal['int']]`, the quotes inside the `Literal` should be preserved.

Fixes: https://github.com/astral-sh/ruff/issues/9137

## Test Plan

Add various test cases to validate this change, validate the snapshots. There are no ecosystem changes to go through.


---

_Comment by @codspeed-hq[bot] on 2024-06-10 19:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Glyphack:auto-quote-annotation-quotes)

### Merging #11811 will **not alter performance**

<sub>Comparing <code>Glyphack:auto-quote-annotation-quotes</code> (2c10896) with <code>main</code> (2f88f84)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Marked ready for review by @Glyphack on 2024-06-10 19:59_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:275 on 2024-06-11 06:28_

We can re-use the `quote` variable from above.

```suggestion
    let annotation_new = if annotation.contains(quote.as_char()) {
        annotation.replace(
            quote.as_char(),
            &quote.opposite().as_char().to_string(),
        )
    } else {
        annotation
    };
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/quote.py`:106 on 2024-06-11 06:29_

Can you also add a test case for the opposite quote (`AbstractBaseUser['int']`) and one where both are being used (`AbstractBaseUser['int', "str"]`)?

---

_@dhruvmanila reviewed on 2024-06-11 06:30_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-06-11 06:30_

---

_Review requested from @dhruvmanila by @Glyphack on 2024-06-11 17:29_

---

_@dhruvmanila reviewed on 2024-06-12 09:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-06-12 09:24_

I'm sorry I didn't notice this earlier but looking at the documentation of `quote_annotation`, I don't know if this is the intended solution. I'm referring to the last two points in the examples which mentions the following conversion:

1. `Series["pd.Timestamp"]` -> `"Series[pd.Timestamp]"`
2. `Series[Literal["pd.Timestamp"]]` -> `"Series[Literal['pd.Timestamp']]"`

In (1), the inner quote isn't required while in (2) it is required. So, all in all, it's a little bit more complicated than just replacing the quote chars 😅

---

_@dhruvmanila reviewed on 2024-06-12 09:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-06-12 09:28_

I discussed this with @AlexWaygood and I think this requires a different solution. Basically, per https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions and what Alex suggests in general is that:

> Broadly speaking, any sub-element in the typing grammar that is marked as `expression` cannot have the quotes removed, but all other sub-elements (whether they're annotation expressions or type expressions, etc.), you should be able to remove the quotes from if a broader type/annotation expression that the sub-element is part of is quoted

So,
* We shouldn't removes quotes from all parameters in `Literal` and all except the first parameter in `Annotated`
* While, we can remove quotes from other types which is why in (1) the quotes around `pd.Timestamp` is removed

---

_@Glyphack reviewed on 2024-06-12 16:04_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-06-12 16:04_

No worries, I also expected my initial implementation to be wrong. Just wanted to open the PR and see the difference in test cases. Let me go over this again but it will be in a few days I'll notify when it's ready.

---

_@dhruvmanila reviewed on 2024-06-12 16:16_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-06-12 16:16_

Thank you for understanding and take your time.

---

_@MichaReiser requested changes on 2024-08-01 21:44_

Putting back in your queue according to Dhruv's comment

---

_Comment by @Glyphack on 2024-08-02 08:13_

Thanks, I was away for a while I want to work on this tomorrow.

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-08-14 19:54_

@dhruvmanila I tried to make some adjustment to make it work but I can use your help.

So now we can remove the inner quotes if there's any all the type except for when the inner quote is around values inside a Literal and specific parameters of Annotated.

To find out if the quotes are around those structures we need to take the expression and traverse down the AST and see if the quotes are at these specific places or not.
I'm thinking to have a function that does something similar to https://github.com/Glyphack/ruff/blob/14f51e3d9e96829deab91dd6e24cda5d0ec2fe36/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L229 and checks all children of that annotation expression to make sure the quotes inside are not belonging to those two types and we can safely remove them.

I'm thinking how can I limit the check here since traversing and checking all the node types and visiting the children might be a lot of code. For example do you think if we move this logic somewhere else it could help? Or maybe I'm missing something here.

An alternative way could be only checking if the annotation contains the work Literal or Annotated and then check based on characters in the string to determine if the quotes are there or not. I'm not entirely sure if this would work or not. Sounds very hacky but less code than checking node types of children.

What do you think?

---

_@Glyphack reviewed on 2024-08-14 19:54_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-08-15 11:08_

---

_@dhruvmanila reviewed on 2024-08-16 12:52_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-08-16 12:52_

> So now we can remove the inner quotes if there's any all the type except for when the inner quote is around values inside a Literal and specific parameters of Annotated.

Do you have any code that I can play around with? It's ok if you haven't implemented it yet. And, does this mean that this solves the problem as seen in the example (1) of the first comment in this thread?

> To find out if the quotes are around those structures we need to take the expression and traverse down the AST and see if the quotes are at these specific places or not.
> I'm thinking to have a function that does something similar to [Glyphack/ruff@`14f51e3`/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L229](https://github.com/Glyphack/ruff/blob/14f51e3d9e96829deab91dd6e24cda5d0ec2fe36/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L229) and checks all children of that annotation expression to make sure the quotes inside are not belonging to those two types and we can safely remove them.

Using a visitor should be able to provide you the traversal that you need. I think you'd need to define a custom struct which will contain all the necessary state variables from the `quote_annotation` function, then call the `visit_expr` method on the expression corresponding to the `node_id` as given in the `quote_annotation` function. This will start the traversal.

You can also create the final quote annotation on the go as you traversed down the node. For instance, the struct would be initiated with an empty string or you can also pre-allocate the string using the expression range (number of characters in the quoted string should be same as the node range). Then, you could maintain state variables which will tell you whether you're in `Annotated` or `Literal` or any other type.

One thing to note, and might very well be unlikely, is that we should make sure that we don't end up repeating the same quote where the outermost node has a double quote and then there's a single quote inside which then again contains a double quote.

Does this make sense? Feel free to ask any questions, quotes are hard ;)

---

_Comment by @github-actions[bot] on 2024-08-23 23:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Glyphack reviewed on 2024-08-25 15:40_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-08-25 15:40_

I tried your way and I could implement something that solves the initial problem and the Litral and Annotated cases. What do you think about this?
Is there any edge cases that I'm not seeing in this scenario?

I am not checking all ast node types right now but will complete that if this is not missing any critical parts.

---

_@dhruvmanila reviewed on 2024-08-26 04:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-08-26 04:21_

Perfect, I'll look at this today. Thanks for working on this!

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-09-02 13:12_

---

_Label `rule` added by @dhruvmanila on 2024-09-02 13:13_

---

_@dhruvmanila reviewed on 2024-09-05 11:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-09-05 11:09_

> I tried your way and I could implement something that solves the initial problem and the Litral and Annotated cases. What do you think about this?

I think the visitor pattern seems to be working well.

> I am not checking all ast node types right now but will complete that if this is not missing any critical parts.

Can you give me an example of what do you mean here?

---

_@dhruvmanila reviewed on 2024-09-05 11:10_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:335 on 2024-09-05 11:10_

We should try to avoid doing any intermediate allocations
```suggestion
                    let value = generator.expr(value);
                    self.annotation.push_str(&value);
                    self.annotation.push_str("[");
```

---

_@dhruvmanila reviewed on 2024-09-05 11:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:305 on 2024-09-05 11:13_

I think I would switch this around and have `AnnotatedFirst` and `AnnotatedRest` or something similar to make it explicit about the state.

---

_@dhruvmanila reviewed on 2024-09-05 11:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:363 on 2024-09-05 11:13_

```suggestion
                self.annotation.push_str(op.as_str());
```

---

_@dhruvmanila reviewed on 2024-09-05 11:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/test.rs`:93 on 2024-09-05 11:14_

What's the reason for changing this? Can you specify which test case required this?

---

_@dhruvmanila reviewed on 2024-09-05 11:15_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote.py.snap`:499 on 2024-09-05 11:15_

This doesn't seem correct, changing `"1"` to `1` in the `Annotated` type

---

_@dhruvmanila reviewed on 2024-09-05 11:20_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote.py.snap`:499 on 2024-09-05 11:20_

I think this is because in the catch all case within the visitor, you're not checking for `AnnotatedNonFirstElm`

---

_Comment by @dhruvmanila on 2024-09-05 11:21_

Thanks for working on this. Overall, the approach looks good. I'll give it a detailed review once you think it's ready. I'd also recommend adding test cases with mixed quote style i.e., scenarios where we might not be able to provide a fix.

---

_@Glyphack reviewed on 2024-09-11 15:59_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/test.rs`:93 on 2024-09-11 15:59_

This test case fails after the new changes:
```
rules::flake8_type_checking::tests::quote::rule_typingonlythirdpartyimport_path_new_quote_py_expects
```

I initially did this before trying the visitor approach. I'm not sure if now it can be removed or not. I will look into it to make sure we are not flipping the quotes multiple times. Please let me know if you have a suggestion for figuring out if this is indication of a bug or it's okay to increase the number.

<details>
```
Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:
quote.py:24:24: TCH002 [*] Move third-party import `pandas.DataFrame` into a type-checking block
   |
23 | def f():
24 |     from pandas import DataFrame
   |                        ^^^^^^^^^ TCH002
25 |
26 |     def baz() -> DataFrame[int]:
   |
   = help: Move into type-checking block

ℹ Unsafe fix
2  2  |
3  3  | if TYPE_CHECKING:
4  4  |     from pandas import DataFrame
   5  |+    from pandas import DataFrame
5  6  |     import pandas as pd
6  7  |     import pandas as pd
7  8  |     import pandas as pd
--------------------------------------------------------------------------------
21 22 |
22 23 |
23 24 | def f():
24    |-    from pandas import DataFrame
25 25 |
26    |-    def baz() -> DataFrame[int]:
   26 |+    def baz() -> "DataFrame[int]":
27 27 |         ...
28 28 |
29 29 |

quote.py:17:24: TCH002 [*] Move third-party import `pandas.DataFrame` into a type-checking block
   |
16 | def f():
17 |     from pandas import DataFrame
   |                        ^^^^^^^^^ TCH002
18 |
19 |     def baz() -> DataFrame:
   |
   = help: Move into type-checking block

ℹ Unsafe fix
2  2  |
3  3  | if TYPE_CHECKING:
4  4  |     from pandas import DataFrame
   5  |+    from pandas import DataFrame
5  6  |     import pandas as pd
6  7  |     import pandas as pd
7  8  |     import pandas as pd
--------------------------------------------------------------------------------
14 15 |
15 16 |
16 17 | def f():
17    |-    from pandas import DataFrame
18 18 |
19    |-    def baz() -> DataFrame:
   19 |+    def baz() -> "DataFrame":
20 20 |         ...
21 21 |
22 22 |
```
</details>

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-09-11 16:18_

> Can you give me an example of what do you mean here?

I thought there are more expression enum values that I should handle here(not send everything to catch all), after checking them now I did not see any of them that require special attention like the subscript and tuple so I think it's fine.

I applied the comments but this is not ready for review yet please wait until I add more test cases.

---

_@Glyphack reviewed on 2024-09-11 16:18_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote.py.snap`:499 on 2024-09-11 16:18_

Yes, I mistakenly kept the first argument quoted but now it's fixed. Thanks.

---

_@Glyphack reviewed on 2024-09-11 16:18_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:224 on 2024-09-11 18:19_

> I applied the comments but this is not ready for review yet please wait until I add more test cases.

No worries, feel free to ping me if you're stuck or when it's ready to review.

---

_@dhruvmanila reviewed on 2024-09-11 18:19_

---

_@dhruvmanila reviewed on 2024-09-12 13:44_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/test.rs`:93 on 2024-09-12 13:44_

I just ran it on your branch with the original value and it works. I think it can be reverted back.

---

_@Glyphack reviewed on 2024-09-16 15:16_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/test.rs`:93 on 2024-09-16 15:16_

I changed it back to original config in this commit but CI fails:
https://github.com/astral-sh/ruff/pull/11811/commits/5079a8c85cce20c47ca6507c50babe74efd0d8b9

https://github.com/astral-sh/ruff/actions/runs/10886671657/job/30207264819?pr=11811#step:8:3383

---

_Review requested from @dhruvmanila by @Glyphack on 2024-09-16 15:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:312 on 2024-10-03 13:13_

nit: we can avoid storing the `final_quote_type` as that can be queried from `stylist.quote()` wherever required

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:331 on 2024-10-03 13:15_

Should we only look for `Name` node here? What about attribute nodes like `pd.DataFrame` or `typing.Optional[int]` which is more relevant in this context as it's a subscript expression.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:377 on 2024-10-03 13:18_

I think we're only interested in a binary or operation here, right? And, I think at this stage we'll only be getting them because others are filtered out in `quote_annotation` function above. But, it might make sense to still enforce it here by using a `debug_assert`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/quote.py`:104 on 2024-10-03 13:30_

I think I know why the test is failing within the current `MAX_ITERATIONS` value. All of the different test cases are within the same function here so when some of them tries to add the `if TYPE_CHECKING` block along with the `TYPE_CHECKING` import, the edits collide and thus it won't be applied which will then require another iteration. We should split the test cases, grouping them based on similarity, in similar way as done in the top half of the file.

---

_@dhruvmanila reviewed on 2024-10-03 13:30_

---

_Comment by @dhruvmanila on 2024-10-03 13:48_

Thanks for writing an extensive test suite. I think the changes are looking pretty great and close to being merged. I would suggest to separate the test cases in groups where the first group raises the diagnostics and the second group doesn't. The first group would need to be split into individual isolated functions like existing test cases. The second group can be in a single function because that won't raise any diagnostics.

Also, apologies for the late review, I was on vacation for the past 2 weeks which is why the review was delayed.

---

_@Glyphack reviewed on 2024-10-14 19:33_

---

_Review comment by @Glyphack on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/quote.py`:104 on 2024-10-14 19:33_

You are right. I think I can fix that easily. There's just another problem I encountered. Aside from the new tests I've written the old ones also have this problem that each time the type annotation is changed a new import is added and the duplicate imports I think are removed in the end. So now the file fails again.
<output>
```
---- rules::flake8_type_checking::tests::quote::rule_typingonlythirdpartyimport_path_new_quote_py_expects stdout ----
thread 'rules::flake8_type_checking::tests::quote::rule_typingonlythirdpartyimport_path_new_quote_py_expects' panicked at crates/ruff_linter/src/test.rs:167:17:
Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:
quote.py:17:24: TCH002 [*] Move third-party import `pandas.DataFrame` into a type-checking block
   |
16 | def f():
17 |     from pandas import DataFrame
   |                        ^^^^^^^^^ TCH002
18 |
19 |     def baz() -> DataFrame:
   |
   = help: Move into type-checking block

ℹ Unsafe fix
3  3  | if TYPE_CHECKING:
4  4  |     from pandas import DataFrame
5  5  |     from pandas import DataFrame
   6  |+    from pandas import DataFrame
6  7  |     import pandas as pd
7  8  |     import pandas as pd
8  9  |     import pandas as pd
--------------------------------------------------------------------------------
14 15 |
15 16 |
16 17 | def f():
17    |-    from pandas import DataFrame
18 18 |
19    |-    def baz() -> DataFrame:
   19 |+    def baz() -> "DataFrame":
20 20 |         ...
21 21 |
22 22 |

note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

<output>

It can be reproduced by just duplicating one of the functions multiple times in master:

```
def f():
    from pandas import DataFrame

    def baz() -> DataFrame[int]:
        ...

def f():
    from pandas import DataFrame

    def baz() -> DataFrame[int]:
        ...

def f():
    from pandas import DataFrame

    def baz() -> DataFrame[int]:
        ...
```

My guess is that two rules are conflicting here. One is moving the import to type checking block but since now that import is in type checking block [another function later](https://github.com/astral-sh/ruff/blob/04b636cba2c2eff5539c846fe4e654583b7ee472/crates/ruff_linter/resources/test/fixtures/flake8_type_checking/quote.py#L89) in the quote.py is using that import and it is resolved when the symbol is looked up [here](https://github.com/Glyphack/ruff/blob/5079a8c85cce20c47ca6507c50babe74efd0d8b9/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs#L131)

I'm not sure if I'm right because the import should be resolved to the import inside the function itself. It's my guess. I will investigate this later after resolving other issues.
For now I'm going to create a new file that will avoid this issue. Moving my new test cases to a file works fine.

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:377 on 2024-10-14 20:00_

We should be right. Added it.

---

_@Glyphack reviewed on 2024-10-14 20:00_

---

_@Glyphack reviewed on 2024-10-14 20:37_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:331 on 2024-10-14 20:37_

Correct. Is it enough if we rely on an attribute with value of "typing" and attr of "Optional" or "Literal", etc.?
Or should we fully resolved and check the qualified name in this case that the symbol is resolved to that class or not?

---

_@Glyphack reviewed on 2024-10-14 20:53_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:331 on 2024-10-14 20:53_

I added another case for attributes.

---

_Renamed from "quote annotations that already contain quotes" to "[`flake8-type-checking`] Support auto-quoting when annotations contain quotes" by @dhruvmanila on 2024-10-23 06:20_

---

_@dhruvmanila reviewed on 2024-10-23 06:25_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:353 on 2024-10-23 06:25_

I've updated the code to use the semantic model to resolve the expression into its qualified name and we use that to match against the typing import. This way we make sure that any import aliases are correctly resolved.

---

_@dhruvmanila reviewed on 2024-10-23 06:26_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:368 on 2024-10-23 06:26_

Previously, the state would only append so it would be `[..., AnnotatedFirst, AnnotatedRest]` and later it would only pop the `AnnotatedRest` state. I've updated this to replace `AnnotatedFirst` with `AnnotatedRest` instead.

---

_@dhruvmanila reviewed on 2024-10-23 06:26_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:383 on 2024-10-23 06:26_

There was a branch for `Expr::BoolOp` although I don't think that's required. Can you confirm this? @Glyphack 

---

_@dhruvmanila reviewed on 2024-10-23 06:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:66 on 2024-10-23 06:28_

I've split the files because otherwise we run into the iteration limit in tests which is set to 10. We'd need to increase it substantially (~20) to fit all the test cases in a single file. The reason the limit needs to be increased seems unrelated to what's being changed in this PR and my guess for why it's happening is because the import edits are colliding.

---

_@Glyphack reviewed on 2024-10-23 08:39_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:383 on 2024-10-23 08:39_

You are correct. I was not very familiar with syntax in type annotation but I don't see anything with a boolean operation.

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:353 on 2024-10-23 08:40_

I really appreciate your help and the reviews it thought me a lot.

---

_@Glyphack reviewed on 2024-10-23 08:40_

---

_@dhruvmanila approved on 2024-10-23 10:51_

Thanks for contributing this change and for your patience throughout the review cycle! I think this is ready to land, I'll go through the snapshots once and move ahead.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote.py.snap`:1 on 2024-10-23 10:58_

This is a large snapshot diff because one of the test case was removed which had `DataFrame["int"]` as an annotation. Previously, we would highlight this but won't be auto-fixable but now we do. Once that happens, it hits the iteration limit in tests and so I moved it to a different file which means the line numbers for all subsequent test cases have been updated. I verified that this snapshot update is correct by keeping the test case and updating the limit which gives reduced diff in the snapshot.



---

_@dhruvmanila reviewed on 2024-10-23 10:58_

---

_Merged by @dhruvmanila on 2024-10-23 11:04_

---

_Closed by @dhruvmanila on 2024-10-23 11:04_

---

_Branch deleted on 2024-10-23 11:28_

---
