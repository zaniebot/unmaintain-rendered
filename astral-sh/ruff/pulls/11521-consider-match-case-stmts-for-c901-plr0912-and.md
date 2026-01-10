```yaml
number: 11521
title: "Consider match-case stmts for `C901`, `PLR0912`, and `PLR0915`"
type: pull_request
state: merged
author: blueraft
labels:
  - rule
assignees: []
merged: true
base: main
head: match-case-statement
created_at: 2024-05-23T19:34:21Z
updated_at: 2024-05-27T10:35:04Z
url: https://github.com/astral-sh/ruff/pull/11521
synced_at: 2026-01-10T22:05:26Z
```

# Consider match-case stmts for `C901`, `PLR0912`, and `PLR0915`

---

_Pull request opened by @blueraft on 2024-05-23 19:34_

Resolves #11421

## Summary

Instead of counting match/case as one statement, consider each `case` as a conditional. 

## Test Plan

`cargo test`


---

_@blueraft reviewed on 2024-05-23 19:38_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-23 19:38_

I'm not so sure about this, the equivalent `else statement` has the following check: 

```rust
                   if clause.test.is_some() {
                        complexity += 1;
                    }
```

---

_Comment by @github-actions[bot] on 2024-05-23 19:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:183 on 2024-05-24 06:27_

I don't think we should count the `match` statement itself as a branch because it's the case blocks which does the branching.


```suggestion
                cases.len()
                    + cases
                        .iter()
                        .map(|case| num_branches(&case.body))
                        .sum::<usize>()
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:103 on 2024-05-24 06:29_

Similar to the `too-many-branches`, the `match` statement itself doesn't add the complexity but it's the `case` blocks which does.
```suggestion
                for case in cases {
                    complexity += 1;
                    complexity += get_complexity_number(&case.body);
                }
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_statements.rs`:95 on 2024-05-24 06:31_

I'm unsure of whether `case` should be counted here but looking at the logic for other statements, I think it should.

---

_@dhruvmanila requested changes on 2024-05-24 06:32_

Thank you for working on this!

---

_@dhruvmanila reviewed on 2024-05-24 06:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-24 06:32_

Can you clarify what you're not sure about? Is it that the catch-all case (`_`) acts as an `else` block of an `if` statement?

---

_Label `rule` added by @dhruvmanila on 2024-05-24 06:33_

---

_@blueraft reviewed on 2024-05-24 07:51_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-24 07:51_

> Is it that the catch-all case (_) acts as an else block of an if statement?

Yep, complexity wise to me it's equivalent to the following `if else` statement.

```python
if x == 30:
    print("foo")
else:
    print("bar")
```


---

_@blueraft reviewed on 2024-05-24 07:59_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:183 on 2024-05-24 07:59_

Makes sense, done.

---

_@blueraft reviewed on 2024-05-24 08:07_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/pylint/rules/too_many_statements.rs`:95 on 2024-05-24 08:07_

Hm, feel like we should count it yeah. 

---

_@dhruvmanila reviewed on 2024-05-24 09:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-24 09:13_

Aren't they equivalent? We'd consider the above `if else` statement with complexity 2 and similarly for the following match statement:

```py
match subject:
	case foo: ...
	case _: ...
```

---

_@dhruvmanila reviewed on 2024-05-24 09:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_statements.rs`:95 on 2024-05-24 09:13_

Yeah, I was just thinking out loud :)

---

_Renamed from "Consider match-case stmts for C901, PLR0912, and PLR0915" to "Consider match-case stmts for `C901`, `PLR0912`, and `PLR0915`" by @dhruvmanila on 2024-05-24 09:14_

---

_@dhruvmanila approved on 2024-05-24 09:14_

Thank you!

---

_Merged by @dhruvmanila on 2024-05-24 09:14_

---

_Closed by @dhruvmanila on 2024-05-24 09:14_

---

_@blueraft reviewed on 2024-05-24 11:16_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-24 11:16_

The `if else` statement has a complexity of 1, the ` &clause.test.is_some()` evaluates to false for the `else`.

```sh
[crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs:413:9] &source = "\nif x == 3:\n    print(\"foo\")\nelse:\n    print(\"bar\")\n "
[crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs:79:21] &clause.test.is_some() = false
[crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs:415:9] &get_complexity_number(&stmts) = 1
```

---

_@dhruvmanila reviewed on 2024-05-24 14:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-24 14:06_

Oh I see. Hmm, that's interesting. Yeah, I think we could do that i.e., check if the last `case` pattern is either a `_` or any variable as both act as a catch-all pattern.

---

_@blueraft reviewed on 2024-05-25 09:04_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-25 09:04_

I think only `_` acts as a wild card pattern if my understanding of the [documentation](https://docs.python.org/3/reference/compound_stmts.html#wildcard-patterns) is correct. In any case, I can make a follow up PR to handle this. :) 

---

_@dhruvmanila reviewed on 2024-05-27 04:48_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:440 on 2024-05-27 04:48_

> I think only `_` acts as a wild card pattern if my understanding of the [documentation](https://docs.python.org/3/reference/compound_stmts.html#wildcard-patterns) is correct.

Yes, although a name pattern would also _act_ in a similar way except that it also binds the value to the variable. So, in the following example `foo` will be same as `subject`. It's like doing `foo = subject`.

```py
match subject:
	case 2: ...
	case foo: ...
```

You can even see that in CPython where it raises a syntax error similar to `_` if the name pattern is not the last pattern:

```py
match 1:
    case x:
        print(x)
    case 2:
        ...
```

Running `python test.py`:

```console
$ python3.13 parser/_.py
  File "/Users/dhruv/playground/ruff/parser/_.py", line 2
    case x:
         ^
SyntaxError: name capture 'x' makes remaining patterns unreachable
```

> In any case, I can make a follow up PR to handle this. :)

Sounds good.

---

_Branch deleted on 2024-05-27 10:35_

---
