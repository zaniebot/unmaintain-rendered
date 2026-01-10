```yaml
number: 10771
title: "Add tests for `import` and `from ... import` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/import-stmt
created_at: 2024-04-04T06:54:34Z
updated_at: 2024-04-04T10:38:59Z
url: https://github.com/astral-sh/ruff/pull/10771
synced_at: 2026-01-10T22:47:03Z
```

# Add tests for `import` and `from ... import` statement

---

_Pull request opened by @dhruvmanila on 2024-04-04 06:54_

## Summary

This PR adds test cases for `import` and `import ... from` statement.

There are few more things in this PR related to the import statement parsing:
1. Move helper function `parse_alias` and `parse_dotted_name` close to import statement parsing function
2. Improve error handling, especially when the field is `Option<T>`. We'll use `None` if the value is missing instead of an invalid node and report an appropriate error.
3. Rename `TupleParenthesized` to `Parenthesized` and use it to generally inform the parser whether the elements are surrounded by parentheses or not.
4. Update logic to raise syntax error where it didn't earlier. For example, a star import isn't allowed in `import` statement or a star import cannot be mixed with other names in a `from ... import` statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-04 06:54_

---

_Label `testing` added by @dhruvmanila on 2024-04-04 06:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-04 06:54_

---

_Comment by @codspeed-hq[bot] on 2024-04-04 06:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/import-stmt)

### Merging #10771 will **degrade performances by 4.46%**

<sub>Comparing <code>dhruv/import-stmt</code> (260acc2) with <code>dhruv/parser</code> (4a4e2fb)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/import-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/import-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 4.8 ms | 5.1 ms | -4.46% |


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:1119 on 2024-04-04 07:37_

I recommend updating all the numbers. It's a bit painful but it otherwise becomes difficult to spot what the next free bit is.
```suggestion
        const IMPORT_FROM_AS_NAMES_PARENTHESIZED = 1 << 6;
        const IMPORT_FROM_AS_NAMES_UNPARENTHESIZED = 1 << 7;
        const IMPORT_NAMES = 1 << 8;
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:1119 on 2024-04-04 07:38_

Another alternative (considering that it is a bitflag) is to have a flag `PARENTHESIZED` and for `IMPORT_FROM_AS_NAMES_PARENTHESIZED` you set `IMPORT_FROM_AS_NAMES` and `PARENTHESIZED`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:4 on 2024-04-04 07:39_

Nit: Uh no :P I don't like itertools haha. It has a few extensions that make it non obvious that they're allocating. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:434 on 2024-04-04 07:41_

Can we change this to `0` considering that the first ellipsis will always be eaten by the while loop?
```suggestion
        let mut leading_dots = 0;
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:446 on 2024-04-04 07:42_

Nit: I think I would rewrite this to a loop. It avoids checking twice if the parser is at a DOT or Ellipsis token
```suggestion
        loop {
            progress.assert_progressing(self);

            if self.eat(TokenKind::Dot) {
                leading_dots += 1;
            } else if self.eat(TokenKind::Ellipsis) {
                leading_dots += 3;
            } else {
                break;
            }
        }
```

I otherwise recommend you to use `bump` for the Ellipsis case
```suggestion
        while self.at_ts(DOT_ELLIPSIS_SET) {
            progress.assert_progressing(self);

            if self.eat(TokenKind::Dot) {
                leading_dots += 1;
            } else {
                self.bump(TokenKind::Ellipsis);
                leading_dots += 3;
            }
        }

```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:460 on 2024-04-04 07:44_

Nit
```suggestion
        let module = if self.at(TokenKind::Name) {
            Some(self.parse_dotted_name())
        } else {
            if leading_dots == 0 {
                // test_err from_import_missing_module
                // from
                // from import x
                self.add_error(
                    ParseErrorType::OtherError("Expected a module name".to_string()),
                    self.current_token_range(),
                );
            }
            None
        };

```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:503 on 2024-04-04 07:45_

What about `from x import *, *, a`?

---

_@MichaReiser approved on 2024-04-04 07:47_

---

_@dhruvmanila reviewed on 2024-04-04 10:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:1119 on 2024-04-04 10:09_

Yeah, I like the alternative, will make it as a follow-up for other variants as well.

---

_@dhruvmanila reviewed on 2024-04-04 10:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:4 on 2024-04-04 10:11_

I think I only use it for `join` which can be done with fold as well.

---

_@dhruvmanila reviewed on 2024-04-04 10:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:503 on 2024-04-04 10:25_

It does report an error. Is the worry that there are 2 star imports in your test case? I'll add it.

---

_Merged by @dhruvmanila on 2024-04-04 10:35_

---

_Closed by @dhruvmanila on 2024-04-04 10:35_

---

_Branch deleted on 2024-04-04 10:35_

---
