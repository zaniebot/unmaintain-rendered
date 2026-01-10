```yaml
number: 17090
title: "[`pycodestyle`] Auto-fix redundant boolean comparison (`E712`)"
type: pull_request
state: merged
author: knavdeep152002
labels:
  - fixes
assignees: []
merged: true
base: main
head: feature/E712-autofix
created_at: 2025-03-31T12:04:28Z
updated_at: 2025-05-10T12:00:33Z
url: https://github.com/astral-sh/ruff/pull/17090
synced_at: 2026-01-10T18:57:02Z
```

# [`pycodestyle`] Auto-fix redundant boolean comparison (`E712`)

---

_Pull request opened by @knavdeep152002 on 2025-03-31 12:04_

This pull request fixes https://github.com/astral-sh/ruff/issues/17014

changes this
```python
from __future__ import annotations

flag1 = True
flag2 = True

if flag1 == True or flag2 == True:
    pass

if flag1 == False and flag2 == False:
    pass

flag3 = True
if flag1 == flag3 and (flag2 == False or flag3 == True):  # Should become: if flag1==flag3 and (not flag2 or flag3)
    pass

if flag1 == True and (flag2 == False or not flag3 == True):  # Should become: if flag1 and (not flag2 or not flag3)
    pass

if flag1 != True and (flag2 != False or not flag3 == True):  # Should become: if not flag1 and (flag2 or not flag3)
    pass


flag = True
while flag == True:  # Should become: while flag
    flag = False

flag = True
x = 5
if flag == True and x > 0:  # Should become: if flag and x > 0
    print("ok")

flag = True
result = "yes" if flag == True else "no"  # Should become: result = "yes" if flag else "no"

x = flag == True < 5

x = (flag == True) == False < 5
```

to this 
```python
from __future__ import annotations

flag1 = True
flag2 = True

if flag1 or flag2:
    pass

if not flag1 and not flag2:
    pass

flag3 = True
if flag1 == flag3 and (not flag2 or flag3):  # Should become: if flag1 == flag3 and (not flag2 or flag3)
    pass

if flag1 and (not flag2 or not flag3):  # Should become: if flag1 and (not flag2 or not flag3)
    pass

if not flag1 and (flag2 or not flag3):  # Should become: if not flag1 and (flag2 or not flag3)
    pass


flag = True
while flag:  # Should become: while flag
    flag = False

flag = True
x = 5
if flag and x > 0:  # Should become: if flag and x > 0
    print("ok")

flag = True
result = "yes" if flag else "no"  # Should become: result = "yes" if flag else "no"

x = flag is True < 5

x = (flag) is False < 5
```


---

_Label `fixes` added by @MichaReiser on 2025-03-31 12:16_

---

_Comment by @MichaReiser on 2025-03-31 12:16_

For review, see the comment in https://github.com/astral-sh/ruff/issues/17014#issuecomment-2765881538

---

_Review requested from @ntBre by @MichaReiser on 2025-04-02 07:50_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1374 on 2025-04-02 14:53_

It looks like you only return `value` when the first `bool` is true, so I think `Option<bool>` would be a more natural return type, with `None` representing the `(false, false)` case. You can also trim this down a bit by checking if `comparator` is a boolean literal once (helped by the `Option` return type):
```suggestion
fn is_redundant_boolean_comparison(op: CmpOp, comparator: &Expr) -> Option<bool> {
    let value = comparator.as_boolean_literal_expr()?.value;
    match op {
        CmpOp::Is => Some(value),
        CmpOp::IsNot => Some(!value),
        _ => None,
    }
}
```

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1403 on 2025-04-02 14:56_

You can also use pattern matching here to tie the length check to the element access (and update this code for the `Option` suggestion above):
```suggestion
    if let ([op], [comparator]) = (ops, comparators) {
        if let Some(kind) = is_redundant_boolean_comparison(*op, comparator) {
            return if kind {
                contents
            } else {
                format!("not {contents}")
            };
        }
    }
```

---

_@ntBre requested changes on 2025-04-02 15:03_

Thanks for working on this! I added some code-style suggestions in-line.

To your question on the issue, I think it does make sense to restrict this to the single-comparison case, especially to start. I think that's what you have here.

More broadly, I'm a bit worried about making these changes in this `helpers` file, which is used by multiple rules. The original issue was just about E712. If you can get CI passing here, we can see some results from our ecosystem check and see if any other rules are affected. It could still be an improvement to multiple rules, which would be good, but it does mean it's a larger change.

---

_Comment by @knavdeep152002 on 2025-04-02 17:57_

Hey, Thanks for the code-style suggestions I have refactored the code accordingly. 

On running the tests it has only failed on the E712 rule-related snapshots ( 2 failed ). So should I move the logic to the rule level ( rules/literal_comparison.rs) or keep it in helpers as it may be helpful for other rules.

---

_Review requested from @ntBre by @knavdeep152002 on 2025-04-02 18:15_

---

_Comment by @ntBre on 2025-04-02 18:21_

> So should I move the logic to the rule level ( rules/literal_comparison.rs) or keep it in helpers as it may be helpful for other rules.

I'm still not sure. Can you fix the test failures? If the tests run successfully, we will get a helpful comment [like this](https://github.com/astral-sh/ruff/pull/17129#issuecomment-2770314027) on this PR showing changes in ruff output on a set of ecosystem packages. That will help us to decide if we need to restrict the changes to E712 or if we can change the helper, which might be more convenient or useful.

If there are many changes, or especially if any of them look incorrect, that would strongly suggest limiting the change to E712.

---

_Comment by @knavdeep152002 on 2025-04-02 18:32_

The below is an example of the failed testcase. Previous snapshot was if res == True was changed to if res is True, Now its if res instead.

```bash
Snapshot file: crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap
Snapshot: E712_E712.py
Source: crates/ruff_linter/src/rules/pycodestyle/mod.rs:74
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    9     9 │
   10    10 │ ℹ Unsafe fix
   11    11 │ 1 1 | #: E712
   12    12 │ 2   |-if res == True:
   13       │-  2 |+if res is True:
         13 │+  2 |+if res:
   14    14 │ 3 3 |     pass
   15    15 │ 4 4 | #: E712
   16    16 │ 5 5 | if res != False:
   17    17 │
```

I am not sure about snapshots. Should I edit this snapshot and replace it with what is expected? Whats the general approach with snapshots?

---

_Comment by @ntBre on 2025-04-02 18:44_

You should use [cargo-insta](https://insta.rs/docs/cli/) to update the snapshots. See the [Prerequisites](https://insta.rs/docs/cli/) section of the ruff CONTRIBUTING.md file.

---

_Comment by @github-actions[bot] on 2025-04-02 22:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/74ff8dc7249bb52f54592f2dc22f2805221f7a56/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>




---

_Comment by @knavdeep152002 on 2025-04-03 20:17_

Hey, I was looking into the E712.py and there were a few edge cases which I missed (the left is a bool and the right is a comparator ). 
So I decided to move this redundancy check to the parent block in literal_comparision.rs and have added the redundancy check for left as well as right.

There was this edge case 

```python
if(True) == TrueElement or x == TrueElement:
    pass
```

If this was generally checked for redundancy and converted it would have been
```python
ifTrueElement or x == TrueElement:
    pass
```

which is syntactically wrong so I added a special case for this where the compare range and left range don't start on the same index. I enclose the comparator in parentheses. So it becomes

```python
if(TrueElement) or x == TrueElement:
    pass
```

I tried looking at the ast-parsed tree to find its parentheses or not, but I couldn't find any apart from the range numbers so I have used them to check whether to add parentheses to the comparator or not

Can you review the changes and let me know if I have to make any other changes?
Thank you.

---

_Assigned to @ntBre by @ntBre on 2025-04-03 20:33_

---

_Comment by @ntBre on 2025-04-04 20:26_

I think you can actually rely on [yoda-conditions (SIM300)](https://docs.astral.sh/ruff/rules/yoda-conditions/#yoda-conditions-sim300) to handle these edge cases ([playground](https://play.ruff.rs/faf480f1-2013-4a47-b99e-d259f47fe522) link) and avoid the added complexity in this PR. Fixing the simple case like your first two commits would be plenty for this PR, in my opinion!

Edit: to be a bit more explicit, SIM300 will recommend reversing the condition so that your initial changes catch the issue, without having to handle the redundancy on the left-hand side separately.

---

_Comment by @NavdeepK15 on 2025-04-04 20:30_

So should i revert to my previous commit?

---

_Comment by @ntBre on 2025-04-04 20:31_

> So should i revert to my previous commit?

Yes, that is what I was thinking. Does that sound reasonable to you?

---

_Comment by @knavdeep152002 on 2025-04-05 11:27_

Yeah, if ruff --fix does SIM300 fix first and then if it does the E712 fix then it is completely fine. 

i tried running this command 
```bash
cargo run -p ruff check --fix --unsafe-fixes test.py
```

the below was fixed as 
```python
if (True) == TrueElement or x == TrueElement:
    pass
```

```python
if (True) is TrueElement or x == TrueElement:
    pass
```

So it seems like E712 is being fixed first and then 'yoda-condition' isn't being raised anymore. So I thought to include the redundancy comparison on the left expression as well.

-----

Currently, in the expression.rs The literal comparison is being done first and then the Yoda detection. We can change it as below (but I am not sure why SIM300 fix was placed at the end before. Was this intentional?).

```rust
    if checker.enabled(Rule::YodaConditions) {
        flake8_simplify::rules::yoda_conditions(checker, expr, left, ops, comparators);
    }
    if checker.any_enabled(&[Rule::NoneComparison, Rule::TrueFalseComparison]) {
        pycodestyle::rules::literal_comparisons(checker, compare);
    }
```

Also the test case on  #[test_case(Rule::TrueFalseComparison, Path::new("E712.py"))]
would still be the older one (since its individual test on literal comparison alone) will that be ok?

```bash
   54    54 │ 8   |-if True != res:
   55       │-  8 |+if not res:
         55 │+  8 |+if True is not res:
```



---

_Comment by @ntBre on 2025-04-07 14:52_

Thanks for looking into this! I asked internally, and it sounds like my suggestion was actually wrong, and your handling of both sides is the right way to go because we want to avoid these kinds of ordering dependencies between rules.

I'll give this another review to see if we can simplify some of the parentheses handling, but this is looking good.

I also realized that the parentheses might be an issue on the right side of the comparison. Could you add a test case like this (if there's not one already):

```python
if x is (True)or False: ...
```

I didn't realize you could do something like that until your example here, so thank you for catching that!

---

_Comment by @knavdeep152002 on 2025-04-07 15:21_

Hey, that was a case I hadn’t accounted for earlier. I’ve made the necessary changes along with some code cleanup.

I believe this should now handle all relevant cases. To ensure validity, we can either enclose the expression in parentheses or add an extra space. I couldn’t identify any other edge cases beyond the need for parentheses, so I’ve opted to wrap the expression in them for now.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:383 on 2025-04-10 13:16_

Instead of factoring out `build_conditional_string` and `maybe_wrap`, I tried factoring out the bodies of these `if-let`s into something like this:

```suggestion
                    let needs_wrap = comparator.range().end() != compare.range().end();
                    generate_redundant_comparison(
                        compare,
                        comment_ranges,
                        source,
                        &compare.left,
                        kind,
                        needs_wrap,
                    )
```

where `generate_redundant_comparison` is:

```rust
fn generate_redundant_comparison(
    compare: &ast::ExprCompare,
    comment_ranges: &ruff_python_trivia::CommentRanges,
    source: &str,
    comparator: &Expr,
    kind: bool,
    needs_wrap: bool,
) -> String {
    let comparator_range =
        parenthesized_range(comparator.into(), compare.into(), comment_ranges, source)
            .unwrap_or(comparator.range());

    let comparator_str = &source[comparator_range];

    let result = if kind {
        comparator_str.to_string()
    } else {
        format!("not {comparator_str}")
    };

    if needs_wrap {
        format!("({result})")
    } else {
        result
    }
}
```

with the other two helper functions in-lined into it.

I also don't like having to have the same `generate_comparison` call in the `else` and `_ =>` blocks, but I really can't see a nice way around it (maybe with another function returning an `Option` or something).

Feel free to refactor like I'm suggesting here, or not if you prefer it how it is now. I think this looks good overall.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:184 on 2025-04-10 13:19_

Oh, is the parenthesis detection wrong here? I don't think we need these parens.

---

_@ntBre reviewed on 2025-04-10 13:20_

This looks good. I only had one refactoring suggestion until I noticed a question about parentheses too.

---

_@knavdeep152002 reviewed on 2025-04-15 07:29_

---

_Review comment by @knavdeep152002 on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:184 on 2025-04-15 07:29_

According to the logic, the comparison of the start indexes is on the `(true) == TrueElement`, and the comparator is `true`. So, there is a mismatch, so a bracket was included. 

---

_@ntBre reviewed on 2025-04-18 18:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:184 on 2025-04-18 18:16_

I guess the point I was trying to make is that I would rather not have these parentheses because the expression is valid without them. I would have expected this output here:

```py
if TrueElement or x == TrueElement:
    ...
```

The current logic may not be correct if it preserves unnecessary parentheses.

I tried a couple of approaches to get rid of the `range` comparisons in an earlier review and couldn't come up with something better, though, so this doesn't necessarily need to block this PR. It's still an improvement over the old fix.

---

_@ntBre reviewed on 2025-04-18 18:17_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1366 on 2025-04-18 18:17_

Let's move this into the `literal_comparison.rs` file and make it not `pub` until it's actually needed somewhere else.

---

_Renamed from "feat: add redundant boolean comparison" to "[`pycodestyle`] Auto-fix redundant boolean comparison (`E712`)" by @ntBre on 2025-04-23 15:31_

---

_@ntBre approved on 2025-04-23 15:48_

Thanks again!

---

_Merged by @ntBre on 2025-04-23 15:49_

---

_Closed by @ntBre on 2025-04-23 15:49_

---

_Branch deleted on 2025-05-10 12:00_

---
