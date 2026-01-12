```yaml
number: 14454
title: Fix f-string formatting in assignment statement
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/f-string-assignment-2
created_at: 2024-11-19T13:40:11Z
updated_at: 2024-11-26T09:37:21Z
url: https://github.com/astral-sh/ruff/pull/14454
synced_at: 2026-01-12T15:55:47Z
```

# Fix f-string formatting in assignment statement

---

_@dhruvmanila_

## Summary

fixes: #13813

This PR fixes a bug in the formatting assignment statement when the value is an f-string.

This is resolved by using custom best fit layouts if the f-string is (a) not already a flat f-string (thus, cannot be multiline) and (b) is not a multiline string (thus, cannot be flattened). So, it is used in cases like the following:
```py
aaaaaaaaaaaaaaaaaa = f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression}moreeeeeeeeeeeeeeeee"
```
Which is (a) `FStringLayout::Multiline` and (b) not a multiline.

There are various other examples in the PR diff along with additional explanation and context as code comments.

## Test Plan

Add multiple test cases for various scenarios.


---

_Label `formatter` added by @dhruvmanila on 2024-11-19 13:40_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-19 13:40_

---

_Converted to draft by @dhruvmanila on 2024-11-19 13:40_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-11-19 13:40_

---

_Comment by @github-actions[bot] on 2024-11-20 11:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+4 -2 lines in 1 file in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/15374f63556ae0b5a40a3b09d17fdeb5076eb19a/examples/concurrency/multithreading_hello_milvus.py#L71'>examples/concurrency/multithreading_hello_milvus.py~L71</a>
```diff
         print(
             f"Inserted {len(self.batchs)} batches of {num_entities} entities in {duration} seconds"
         )
-        print(f"Expected num_entities: {len(self.batchs)*num_entities}. \
-                Acutal num_entites: {self.get_thread_local_collection().num_entities}")
+        print(
+            f"Expected num_entities: {len(self.batchs)*num_entities}. \
+                Acutal num_entites: {self.get_thread_local_collection().num_entities}"
+        )
 
 
 if __name__ == "__main__":
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+4 -2 lines in 1 file in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/15374f63556ae0b5a40a3b09d17fdeb5076eb19a/examples/concurrency/multithreading_hello_milvus.py#L71'>examples/concurrency/multithreading_hello_milvus.py~L71</a>
```diff
         print(
             f"Inserted {len(self.batchs)} batches of {num_entities} entities in {duration} seconds"
         )
-        print(f"Expected num_entities: {len(self.batchs) * num_entities}. \
-                Acutal num_entites: {self.get_thread_local_collection().num_entities}")
+        print(
+            f"Expected num_entities: {len(self.batchs) * num_entities}. \
+                Acutal num_entites: {self.get_thread_local_collection().num_entities}"
+        )
 
 
 if __name__ == "__main__":
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-11-20 11:55_

This is looking good!

---

_Comment by @dhruvmanila on 2024-11-20 12:56_

I think the ecosystem changes here is an existing f-string bug.

---

_Renamed from "WIP: Fix f-string formatting in assignment statement" to "Fix f-string formatting in assignment statement" by @dhruvmanila on 2024-11-21 11:15_

---

_Marked ready for review by @dhruvmanila on 2024-11-21 11:31_

---

_Label `preview` added by @MichaReiser on 2024-11-21 11:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:190 on 2024-11-21 11:38_

It's unclear to me why this needs a visitor, considering that we only visit f-string elements. It may need to be recursive but I think that would be easier to understood than a full visitor (where it's difficult to understand where and how it recurses)

I see that your implementation differs from https://github.com/astral-sh/ruff/blob/3a681ebb40db459705dedd4beea741a5cbdb96b1/crates/ruff_python_formatter/src/string/implicit.rs#L190-L207 in that it also covers format spec. Do we need to update the implicit concation formatting too? Can we share the logic

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:50 on 2024-11-21 11:42_

I think using `BestFit` for all f-strings that contain multiline expressions isn't correct. For example. It results in 

```python
if f"aaaaaaaaaaa { ttttteeeeeeeeest} more {
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
}": pass
```

being formatted as 

```python
if f"aaaaaaaaaaa {ttttteeeeeeeeest} more {aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa}":
    pass
```

Which seems worse than before. 

I think we should keep using `Multiline` if the f-string has any multiline expression to avoid collapsing them. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_bin_op.rs`:29 on 2024-11-21 11:43_

Can we add an example covering this path with an expression that starts as a mulitline f-string but then gets formatted as a flat f-string?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:228 on 2024-11-21 11:51_

Your change is an improvement to what we had before. 

For example, this was correctly formatted code before

```python
call(f"{
    testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
}")
```

But I don't think it's correct to use the "hug" layout if an inner expression is multiline

```python
call(f"{
    aaaaaa
    + '''test
    more'''
}")
```



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1112 on 2024-11-21 11:59_

Could we instead return a `FormatFString` instead of having a new `FormatFStringAssignment`, considering that we just forward the call?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:930 on 2024-11-21 12:03_

Is it possible to use `format_f_string` here?
```suggestion
                                    value,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:952 on 2024-11-21 12:06_


```suggestion
                    // ```
                    //
                    // Keep the target flat, and use the regular f-string formatting.
                    //
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/implicit.rs`:157 on 2024-11-21 12:10_

Do we still need the check on line 190-204 considering that it's now covered by `string.is_multiline`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1133 on 2024-11-21 12:12_


```suggestion
        // it's useless to try the flattened layout.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:178 on 2024-11-21 12:14_

Do we need to consider the debug text as well?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:386 on 2024-11-21 12:16_

Could you add some examples with inner comments or debug expressions (and format specs)? See https://github.com/astral-sh/ruff/blob/73ee72b665f333c47c9f597d06317854dd59bdd3/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_assignment.py#L236-L293

---

_@MichaReiser reviewed on 2024-11-21 12:19_

This overall looks good, but I think we're now using the `BestFit` layout in too many places, which leads to bad formatting in clause headers (see my inline comment). 

I think we should only use the `BestFit` layout if the `FString` uses the flat layout. In all other cases, we should use the `Multiline` layout. 

We should add some more tests that cover `BestFit` layout usages in non assignment positions to verify that we got the `needs_parentheses` changes correct. 

See  https://github.com/astral-sh/ruff/blob/73ee72b665f333c47c9f597d06317854dd59bdd3/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string.py#L296-L321

and https://github.com/astral-sh/ruff/blob/73ee72b665f333c47c9f597d06317854dd59bdd3/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string.py#L128-L224


---

_@dhruvmanila reviewed on 2024-11-21 14:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:50 on 2024-11-21 14:56_

I think the reason it was formatted like that previously is because the `needs_parentheses` would return `OptionalParentheses::Never`. If we would return `Multiline` then I think parentheses will be added making it:

```py
if (
    f"aaaaaaaaaaa {ttttteeeeeeeeest} more {
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    }"
):
    pass
```

---

_@MichaReiser reviewed on 2024-11-21 15:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:50 on 2024-11-21 15:02_

We can also decide to return `Never`. It's not entirely clear to me what the better and more consistent layout is. I would have to play with a few layouts but I don't think it should be what it is now. 

Using Never has the advantage that it avoids unnecessary parentheses and is closer to what we had today (and no one complained?). Adding parentheses is similar to having `"aaaaa" + tttttt + "more(aaaaaaaaaaaaaaaaaaaaaaaaaa)`. It might be good to play with the formatting in other positions where we use `BestFit` to decide what the best layout is 

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/arguments.rs`:228 on 2024-11-21 15:19_

I'm not sure I follow here. Are you saying that both the code snippet should result in the formatting where the f-string is on the same line as the call expression (`call(f"{`)?

---

_@dhruvmanila reviewed on 2024-11-21 15:19_

---

_@dhruvmanila reviewed on 2024-11-21 15:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:190 on 2024-11-21 15:21_

Yeah, I can use recursion instead, I think that might be simpler.

> in that it also covers format spec. Do we need to update the implicit concation formatting too? Can we share the logic

I think so although I'll have to check. I can make it a shared function

---

_@dhruvmanila reviewed on 2024-11-21 15:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:50 on 2024-11-21 15:23_

Yeah, that's a good idea, I can do that.

---

_@dhruvmanila reviewed on 2024-11-21 15:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/implicit.rs`:157 on 2024-11-21 15:25_

I think not, I can remove it. Good catch.

---

_@MichaReiser reviewed on 2024-11-21 16:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:228 on 2024-11-21 16:11_

This is not directly related to your PR, but I noticed it when reviewing it because you changed `is_multiline`. We have to make a decision on the idiomatic formatting for "hugging multiline strings" for f-strings. 

I'm leaning towards that we should only "hug" if the f-string itself contains any multiline string literal, but not if any inner-expression is multiline. Similar to prettier: 

```js
call(
  `${
    aaaaaaaaaaaaaaaaaaaaaaaaaaaa +
    `test more
    aaa` +
    morrrrrrrrrrrrrrrrrrrrrr
  }`,
);
```

So what I'm saying is that we should probably not hug for the both cases above.

---

_@dhruvmanila reviewed on 2024-11-25 07:03_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:930 on 2024-11-25 07:03_

I'm not sure I follow the suggestion here. Do you mean to suggest (if possible) to use `format_f_string` instead of `maybe_parenthesize_expression` here?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1283 on 2024-11-25 16:40_

```suggestion

    /// Returns the single [`FString`] if the string isn't implicitly concatenated, [`None`] otherwise.
    pub fn as_single(&self) -> Option<&FString> {
        match &self.inner {
            FStringValueInner::Single(FStringPart::FString(fstring)) => Some(fstring),
            _ => None,
        }
    }

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:136 on 2024-11-25 17:07_

Do we need to consider debug expressions too?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:228 on 2024-11-25 17:09_

I suggest that we add an explicit `is_triple_quoted` check here. I don't think we ever want this to apply to any non-triple quoted strings. And let's add some test cases that cover f-strings too (cases where we don't want the layout to apply as well as cases where it should apply)

---

_@MichaReiser approved on 2024-11-25 17:19_

This looks good. I still think we should add more tests for f-strings in [clause headers and assert statements](https://github.com/astral-sh/ruff/pull/14454#pullrequestreview-2451115483). For example:


```py
if (f"teaaaaaaaaaaaaaaaaaaaaaaaa{
    expressioneeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee # comment
    }aast".contains('a') 
):
    pass
```

We now remove parentheses from:

```py
aaaaaaaaaaaaaaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{[a, b,
    # comment
    ]}moee" # comment
) 
```

but keep them for 

```py
aaaaaaaaaaaaaaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{[a, b,
    ]}moee"
    # comment
) 
```

The second case is consistent with how we handle parentheses for all other non-fstring assignments. 

But I think what you have now is consistent with how e.g. lists are formatted where the parentheses are removed as well

```py
aaaaaaaaaaaaaaaaaa = (
    [testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee,
        # comment
    ]
) 
```

---

_@MichaReiser reviewed on 2024-11-25 17:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:50 on 2024-11-25 17:20_

We should add a test for this

---

_@dhruvmanila reviewed on 2024-11-26 05:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:136 on 2024-11-26 05:39_

I don't think so. If debug expressions are present, then there are no breakpoints in that f-string.

---

_@MichaReiser reviewed on 2024-11-26 06:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:136 on 2024-11-26 06:35_

But they could be multiline, no? The logic here isn't specific to the assignment formatting but is generally used to determine if a string is known to be multiline and it's unclear to me how a newline in a f-string literal is different from a newline in the debug expression

---

_@dhruvmanila reviewed on 2024-11-26 06:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:136 on 2024-11-26 06:38_

Hmm, so something like the following which cannot be flattened:
```py
aaaaaaaa = f"aaaaaaaaaa {
        aaaaaaa + bbbbbbb + cccccccc = } dddddddddd"
```
So, I think we should check if there's a debug expression and if so then check for line breaks in the expression itself.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:136 on 2024-11-26 06:44_

Yes, but I think we also have to test in the debug expression because the above has no line break in the expression, only in the debug expression but the whole f-string should be considered multiline (whether it can be flattened is irrelevant here because this is a general purpose method to determine if a string contains a hard line break)

---

_@MichaReiser reviewed on 2024-11-26 06:44_

---

_Comment by @dhruvmanila on 2024-11-26 07:20_

I've added a bunch of test cases for f-strings in various positions.

The formatting is different between the one in assignment vs e.g., a `for` statement for type of f-string that's mentioned in the linked issue. This is because the special casing is only done for f-string in the assignment value position.

So, the following:
```py
aaaaaa = f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
        expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee"

aaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee"
)

for a in f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
        expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

for a in (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee"
):
    pass
```

will get formatted to:
```py
aaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
)

aaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee"
)

for a in f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression
}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

for a in (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee"
):
    pass
```

I wonder if this would need to be done instead at the f-string expression level.

---

_Comment by @MichaReiser on 2024-11-26 07:31_

The for case seems fine to me. It's mostly consistent with the formatting in if-statements:

```py

for a in f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
        expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

for a in f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

if f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
        expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

if (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
):
    pass
```

```py
for a in f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression
}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

for a in (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeee"
):
    pass

if f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression
}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass

if f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee":
    pass
```

This seems fine to me and we can iterate on the exact formatting in future style guides based on actual user feedback

---

_Comment by @dhruvmanila on 2024-11-26 09:36_

The ecosystem change looks correct to me and is similar to what we do for regular strings:

```diff
--- /Users/dhruv/playground/ruff/formatter/preview.py
+++ /Users/dhruv/playground/ruff/formatter/preview.py
@@ -1,6 +1,10 @@
 if 1:
     if 1:
-        print("Expected num_entities: {len(self.batchs)*num_entities}. \
-                Acutal num_entites: {self.get_thread_local_collection().num_entities}")
-        print(f"Expected num_entities: {len(self.batchs)*num_entities}. \
-                Acutal num_entites: {self.get_thread_local_collection().num_entities}")
+        print(
+            "Expected num_entities: {len(self.batchs)*num_entities}. \
+                Acutal num_entites: {self.get_thread_local_collection().num_entities}"
+        )
+        print(
+            f"Expected num_entities: {len(self.batchs) * num_entities}. \
+                Acutal num_entites: {self.get_thread_local_collection().num_entities}"
+        )
```

Going to merge this now.

---

_Merged by @dhruvmanila on 2024-11-26 09:37_

---

_Closed by @dhruvmanila on 2024-11-26 09:37_

---

_Branch deleted on 2024-11-26 09:37_

---
