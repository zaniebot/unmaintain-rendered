```yaml
number: 9155
title: "Implement `blank_line_after_nested_stub_class` preview style"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/empty-line-after-class
created_at: 2023-12-16T01:04:32Z
updated_at: 2024-01-30T18:39:39Z
url: https://github.com/astral-sh/ruff/pull/9155
synced_at: 2026-01-10T22:57:09Z
```

# Implement `blank_line_after_nested_stub_class` preview style

---

_Pull request opened by @dhruvmanila on 2023-12-16 01:04_

## Summary

This PR implements the `blank_line_after_nested_stub_class` preview style in the formatter.

The logic is divided into 3 parts:
1. In between preceding and following nodes at top level and nested suite
2. When there's a trailing comment after the class
3. When there is no following node from (1) which is the case when it's the last or the only node in a suite

We handle (3) with `FormatLeadingAlternateBranchComments`.

## Test Plan

- Add new test cases and update existing snapshots
- Checked the `typeshed` diff

fixes: #8891


---

_Label `formatter` added by @dhruvmanila on 2023-12-16 01:04_

---

_Label `preview` added by @dhruvmanila on 2023-12-16 01:04_

---

_@dhruvmanila reviewed on 2023-12-16 01:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@stub_files__suite.pyi.snap`:322 on 2023-12-16 01:05_

This seems like a bug in Black's implementation: https://github.com/psf/black/issues/4113

---

_Comment by @github-actions[bot] on 2023-12-16 01:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+5 -0 lines in 5 files in 2 projects; 1 project error; 40 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+4 -0 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/c50a641fc8d37e873b325c234d3e1c2fa68db31d/stdlib/inspect.pyi#L430'>stdlib/inspect.pyi~L430</a>
```diff
         varargs: str | None
         keywords: str | None
         defaults: tuple[Any, ...]
+
     def getargspec(func: object) -> ArgSpec: ...
 
 class FullArgSpec(NamedTuple):
```
<a href='https://github.com/python/typeshed/blob/c50a641fc8d37e873b325c234d3e1c2fa68db31d/stdlib/sqlite3/dbapi2.pyi#L450'>stdlib/sqlite3/dbapi2.pyi~L450</a>
```diff
 
 @final
 class _Statement: ...
+
 class Warning(Exception): ...
 
 if sys.version_info >= (3, 11):
```
<a href='https://github.com/python/typeshed/blob/c50a641fc8d37e873b325c234d3e1c2fa68db31d/stubs/gevent/gevent/events.pyi#L97'>stubs/gevent/gevent/events.pyi~L97</a>
```diff
 
 @implementer(IGeventWillPatchEvent)
 class GeventWillPatchEvent(GeventPatchEvent): ...
+
 class IGeventDidPatchEvent(IGeventPatchEvent): ...
 
 @implementer(IGeventWillPatchEvent)
```
<a href='https://github.com/python/typeshed/blob/c50a641fc8d37e873b325c234d3e1c2fa68db31d/stubs/pyserial/serial/tools/list_ports_windows.pyi#L34'>stubs/pyserial/serial/tools/list_ports_windows.pyi~L34</a>
```diff
         ClassGuid: ctypes._CField[Incomplete, Incomplete, Incomplete]
         DevInst: ctypes._CField[Incomplete, Incomplete, Incomplete]
         Reserved: ctypes._CField[Incomplete, Incomplete, Incomplete]
+
     PSP_DEVINFO_DATA: type[ctypes._Pointer[SP_DEVINFO_DATA]]
     PSP_DEVICE_INTERFACE_DETAIL_DATA = ctypes.c_void_p
     setupapi: ctypes.WinDLL
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/3ff88390c2bf63ff61010243b4f8726cf63f2ebc/reflex/config.pyi#L47'>reflex/config.pyi~L47</a>
```diff
 class Config(Base):
     class Config:
         validate_assignment: bool
+
     app_name: str
     loglevel: constants.LogLevel
     frontend_port: int
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Review requested from @konstin by @MichaReiser on 2023-12-16 02:25_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-16 02:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:542 on 2023-12-16 02:34_

Should this also apply if the class is nested in a if/while or any other control flow block?

---

_@MichaReiser reviewed on 2023-12-16 02:37_

---

_@konstin approved on 2023-12-18 08:20_

---

_@MichaReiser approved on 2023-12-21 23:46_

---

_Comment by @MichaReiser on 2024-01-10 16:45_

@dhruvmanila what's the status of this PR? Also note that there have been some recent changes in Black. It might be worth double checking Black's tests again to ensure we match the formatting precisely. 

---

_Comment by @dhruvmanila on 2024-01-25 11:42_

Here's an update regarding this preview style implementation in Ruff:

tldr; there are bunch of edge cases where it seems to be difficult to add support ...

### What's working today:

#### 1

Cases when the class definition is followed by another statement at the same indent level. This is to say that in a suite, the preceding and following value is set. Why is this easier? Because [the loop] takes care of a lot of conditions.

For example, in the following code snippet there are 2 statements in a suite which is the body of the `Top` class. Here, the preceding would be `Nested1` class and following would be the assignment statement. Because we have the context of the siblings, it's easier to find out if the class needs an empty line or not.

```python
class Top:
	class Nested1:  # <- statement 1
		pass
	foo = 1  # <- statement 2
```

#### 2

Some other cases which works today is where the class is followed by a trailing comment. This is then handled by the class formatting as then it doesn't matter if there's any other statement following the class or not as we'll always need to add an empty line between the class and the comment. For example,

```python
class Top:
	class Nested1:
		pass
	# comment
```

This will be handled in `empty_lines_before_trailing_comments` builder.

### Problematic cases

Note that some of the following cases have a workaround in the PR but I'm not fully convinced with the implementation which is why they've been put in this section.

The main type of case where there are problems are the ones where the class definition is the last statement in the suite. For example,

#### 1

```python
class Top:
	class Nested:
		pass
foo = 1
```

Here, [the loop] will never be executed because there's just 1 statement in the suite. This means that we need to have a special case implementation for the last statement in the suite. This needs to account for a lot of other conditions to make the decision of whether to add an empty line or not:

Is there a class in the parent suite which already added an empty line? If so, then we don't need to add another empty line.

```python
class Top:
    class Nested11:
        class Nested12:
            pass
    class Nested21:
        pass
```

Here, the `Nested11` class will add an empty line. Now, the `Nested12` class should not be adding an empty line otherwise there'll be 2 empty lines. **This is solved in this PR** by thinking it in a different way. Instead of the `Nested11` class being responsible for adding an empty line, we'll make `Nested12` class responsible for it. This is to say that the innermost class should be responsible to add an empty line. Why? Because as a parent it's easier to traverse through the children to check this condition:

```rust
/// Checks if the last child of the given node is a class definition without a
/// trailing comment.
pub(crate) fn is_last_child_a_class_def(node: AnyNodeRef<'_>, f: &PyFormatter) -> bool {
    let comments = f.context().comments();
    std::iter::successors(node.last_child_in_body(), AnyNodeRef::last_child_in_body)
        .take_while(|last_child| !comments.has_trailing_own_line(*last_child))
        .any(|last_child| last_child.is_stmt_class_def())
}
```

But, this then creates another problem. Take the following code as an example:

```python
if a:
	class Nested:
		pass
else:
	pass
```

Here, the problem is that the new logic will add an empty line after the `Nested` class which will give us:

```python
if a:
	class Nested:
		pass

else:
	pass
```

Now, the same logic will try to _preserve_ the empty line by adding the IR element in the same position. But while formatting the `else` block, the `FormatLeadingAlternateBranchComments` formatter sees that there's an empty line before the `else` block which it tries to maintain by adding an empty line IR element. Thus, two empty line elements are added which creates an unstable formatting.

#### 2

When a comment is not trailing to the class definition but actually to one of the statements in the parent suite:

```python
if top1:
    class Nested:
        pass
# comment
if top2:
    pass
```

#### 3

Similar to the previous case but at end of file:

```python
if top1:
    class Nested:
        pass
# comment
```

### Number of empty lines

We also need to make sure that running the formatter multiple times doesn't keep on adding empty lines (at max it could go to 2). This would happen when the new logic would add an empty line on the first run and then on the second run another logic sees the empty line which it then tries to maintain. This adds multiple empty line IR elements.

## Possible solutions

Instead of looking ahead, we'll always look behind which is to say that for the current node we need to query the absolute previous node. But we'll have to add the logic in multiple places - suite, if, match, try, etc. Here, absolute previous in the literal sense, not just in the same indent level. For example,

```
class Top:
    class Nested11:
        class Nested12:
            pass
    class Nested21:
        pass
```

Here, the previous statement for `Nested21` would be `Nested12` and not `Nested11`.


[the loop]: https://github.com/astral-sh/ruff/blob/86a5375e04b296ca3b6e1717ee07009cf5977930/crates/ruff_python_formatter/src/statement/suite.rs#L190

---

_@dhruvmanila reviewed on 2024-01-29 06:04_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:542 on 2024-01-29 06:04_

Yes

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/format.rs`:102 on 2024-01-29 07:28_

This helps when the class is at the boundary location in nested statement:

```python
if foo:
    class Nested:
        pass
else:
    pass
```

The `FormatLeadingAlternateBranchComments` will be called before the `else` block formatting. Similarly for other blocks like `match`, `case`, `try .. except ..`, etc.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/format.rs`:546 on 2024-01-29 07:30_

Logic for when a class is followed by a trailing comment.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:496 on 2024-01-29 07:36_

It's difficult to merge the `is_*_enabled && *_condition` logic with `empty_line_condition` variable up above like so:

```rust
    let empty_line_condition = preceding_comments.has_trailing()
        || following_comments.has_leading()
        || !stub_suite_can_omit_empty_line(preceding, following, f)
        || (is_blank_line_after_nested_stub_class_enabled(f.context())
                    && blank_line_after_nested_stub_class_condition(
                        preceding.into(),
                        Some(following.into()),
                        f,
                    ))
```

The reason being that in the top level condition the boolean check is direct but in other suite kind there's an additional check of the number of empty lines. That additional check is to _only_ preserve the empty lines if they're present while the preview style adds an empty line conditionally. So, the condition should OR the complete check which was existing earlier to complement it.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:511 on 2024-01-29 07:38_

This is the core condition which checks whether we need to add an empty line between the preceding and following node or not.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:530 on 2024-01-29 07:39_

Class with a trailing comment is handled by `empty_lines_before_trailing_comments` builder.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:536 on 2024-01-29 07:40_

If the preceding node isn't a class, then we check if there's a class node as the last child node of the body of nested nodes. This nesting could be arbitrarily deep.

---

_@dhruvmanila reviewed on 2024-01-29 07:40_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-01-29 07:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:511 on 2024-01-29 13:57_

Nit: The condition is the logic inside of the function, but the function returns a value whether the condition is true. 

That's why I would rename the function, maybe `should_insert_blank_line_after_nested_class_in_stub`?


What's unclear to me is what makes the check specific to nested classes. Isn't it more about whether an empty line should be inserted between any two statements? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:482 on 2024-01-29 14:05_

Moving the `is_blank_line_after_nested_stub_class_enabled` into `blank_lines_after_nested_stub_class_condition` simplifies the calling code a lot

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:514 on 2024-01-29 14:05_

I prefer passing only what's absolutely necessary. Taking a smaller type can also help to avoid borrow checker issue where it is e.g. impossible to hold on to a read-only `PyFormatter` but keeping hold of an immutable `context` or `comments` is possible
```suggestion
    context: &PyFormatContext,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:89 on 2024-01-29 14:14_

Do we need to check here if we're inside a stub file? 

Nit: A more expressive name than `empty_line_condition` would help with readability: `requires_blank_line`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:146 on 2024-01-29 14:18_

Is it possible to move the writting of `empty_line` out of the branches?
```suggestion
        let empty_line_condition = if is_blank_line_after_nested_stub_class_enabled(f.context()) {
            self.last_node.map_or(false, |preceding| {
                blank_line_after_nested_stub_class_condition(preceding, None, f)
            })
        } else {
            false
        };

        if empty_line_condition {
            write!(f, [empty_line(), leading_comments(self.comments)])
        } else if let Some(first_leading) = self.comments.first() {
            // Leading comments only preserves the lines after the comment but not before.
            // Insert the necessary lines.
            write!(
                f,
                [
                    empty_lines(lines_before(first_leading.start(), f.context().source())),
                    leading_comments(self.comments)
                ]
            )
        } else if let Some(last_preceding) = self.last_node {
            // The leading comments formatting ensures that it preserves the right amount of lines
            // after We need to take care of this ourselves, if there's no leading `else` comment.
            // Since the `last_node` could be a compound node, we need to skip _all_ trivia.
            //
            // For example, here, when formatting the `if` statement, the `last_node` (the `while`)
            // would end at the end of `pass`, but we want to skip _all_ comments:
            // ```python
            // if True:
            //     while True:
            //         pass
            //         # comment
            //
            //     # comment
            // else:
            //     ...
            // ```
            //
            // `lines_after_ignoring_trivia` is safe here, as we _know_ that the `else` doesn't
            // have any leading comments.
            write!(
                f,
                [empty_lines(lines_after_ignoring_trivia(
                    last_preceding.end(),
                    f.context().source()
                ))]
            )?;
        } else {
            Ok(())
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:546 on 2024-01-29 14:20_

I think this comment would be helpful in code, maybe along an example? 


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:496 on 2024-01-29 14:26_

I think what would help here is to rename `empty_line_condition` into `preserve_empty_line` which is different from your check which forces empty lines. 

I recommend moving the `blank_line_after_nested_stub_class_condition` out of the `match` and next to `empty_line_condition`, and name the variable `require_empty_line`

I would rename `blank_line_after_nested_stub_class_condition` to `requires_empty_line_between_stub_file_statements` (not just preserving, enforcing an empty line).

We can then rewrite the checks in the match branches to 

```
if requires_empty_line || preserve_empty_line {
    empty_line.fmt(f)
}
```

which I find easier to understand.

Edit: Maybe better. Could we move the check into `stub_suite_can_omit_empty_line`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:528 on 2024-01-29 14:34_

Nit: A match in a binary expression feels very heavy. Some temporary variables could also help to document the conditions. 
```suggestion
            let has_decorators = !class.decorator_list.is_empty();
            let followed_by_class_or_function = match following {
                Some(AnyNodeRef::StmtClassDef(ast::StmtClassDef {
                    body,
                    decorator_list,
                    ..
                })) => !contains_only_an_ellipsis(body, comments) || !decorator_list.is_empty(),
                Some(AnyNodeRef::StmtFunctionDef(_)) | None => true,
                _ => false,
            };

            has_decorators || followed_by_class_or_function
```

I think it would be helpful to add some comments here explaining why the conditions are as they are.

---

_@MichaReiser reviewed on 2024-01-29 14:35_

The code changes overall look good to me. I think we can make some improvements to improve readability and extend documentation a bit.


I'm very surprised that there are no changed black snapshots. Do you know why that is? Are we sure we match Black's formatting?



It would help reviewers if you add an example of the expected formatting (how is it different from today) with a short explanation of the preview style to the PR. I know it's in the linked PR, but it requires multiple steps by the reviewer: Open issue, read the summary, go back to the PR, read the summary, and finally read the code.

---

_Comment by @MichaReiser on 2024-01-29 14:44_

#9674 should ensure that black stub files with options run as stub tests. 

---

_@MichaReiser reviewed on 2024-01-29 14:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:89 on 2024-01-29 14:48_

Can you explain why this special handling is required here but not for other cases for which `stub_suite_can_omit_empty_line` returns false?

---

_Review requested from @konstin by @MichaReiser on 2024-01-29 14:57_

---

_@dhruvmanila reviewed on 2024-01-30 11:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/format.rs`:546 on 2024-01-30 11:45_

I did update the docs for the function :)

https://github.com/astral-sh/ruff/blob/a768fc35ca1f73012ae9d76cba2eedda95c9db1e/crates/ruff_python_formatter/src/comments/format.rs#L534-L546

Is this what you meant?

---

_@dhruvmanila reviewed on 2024-01-30 12:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:496 on 2024-01-30 12:38_

> I think what would help here is to rename `empty_line_condition` into `preserve_empty_line` which is different from your check which forces empty lines.

I'm not sure if this is 100% correct. For top-level, if the condition is true, we always make sure that there is just one empty line. It could be that there isn't any empty line in the source code which means we're adding an additional empty line. But, for the nested cases, we'd only preserve any existing empty lines and limit it to 1.

> I recommend moving the `blank_line_after_nested_stub_class_condition` out of the `match` and next to `empty_line_condition`, and name the variable `require_empty_line`

Thanks, I'll update the code accordingly.

> I would rename `blank_line_after_nested_stub_class_condition` to `requires_empty_line_between_stub_file_statements` (not just preserving, enforcing an empty line).

Yeah, I'm going with `is_blank_line_required_after_nested_stub_class`.

---

_@dhruvmanila reviewed on 2024-01-30 12:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:496 on 2024-01-30 12:41_

> Yeah, I'm going with `is_blank_line_required_after_nested_stub_class`.

Actually, with your other comment, I'm thinking of `should_insert_blank_line_after_class_in_stub_file`.

---

_@dhruvmanila reviewed on 2024-01-30 12:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:511 on 2024-01-30 12:42_

Good point. It isn't specific to nested class. I'll update the name.

---

_@dhruvmanila reviewed on 2024-01-30 12:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:482 on 2024-01-30 12:43_

Thanks for pointing this out. I've extracted this check into a `require_empty_line` variable which I think should be similar in terms of simplification.

---

_@dhruvmanila reviewed on 2024-01-30 14:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/format.rs`:89 on 2024-01-30 14:24_

This is to handle cases when a class definition is the last statement in a suite. The empty line formatting in suite is a loop which handles only for in between statements i.e., both preceding and following are present. This is required only before an alternate branch because otherwise it'll be handled by the top-level suite formatting.

---

_@dhruvmanila reviewed on 2024-01-30 14:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:528 on 2024-01-30 14:24_

Thanks for the suggestion. I've added some comments with examples.

---

_@dhruvmanila reviewed on 2024-01-30 14:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/format.rs`:89 on 2024-01-30 14:24_

Yes, thanks for noticing that.

---

_@MichaReiser reviewed on 2024-01-30 17:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:482 on 2024-01-30 17:04_

Yeah that helps. It might still be worth to move the check into the "condition" function because the check is necessary at all call-sites of that function (and leads to very long conditions)

---

_@MichaReiser approved on 2024-01-30 17:05_

Awesome. Thanks for the added documentation

---

_Merged by @dhruvmanila on 2024-01-30 18:39_

---

_Closed by @dhruvmanila on 2024-01-30 18:39_

---

_Branch deleted on 2024-01-30 18:39_

---
