```yaml
number: 10271
title: Remove deprecated parsing list functions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/deprecate-list-fns
created_at: 2024-03-07T08:51:54Z
updated_at: 2024-03-08T17:52:35Z
url: https://github.com/astral-sh/ruff/pull/10271
synced_at: 2026-01-10T22:47:01Z
```

# Remove deprecated parsing list functions

---

_Pull request opened by @dhruvmanila on 2024-03-07 08:51_

## Summary

This PR removes the deprecated parsing list functions and updates the references to use the new functions.

There are now 4 functions to accommodate this pattern. They are divided into 2 groups: one to parse a sequence of elements and the other to parse a sequence of elements _separated_ by a comma. In each of the groups, there are 2 functions: one collects and returns all the parsed elements as a vector and the other delegates the collection part to the user. This separation is achieved by using `Fn` and `FnMut` to allow mutation in the later case.

The error recovery context has been updated to accommodate the new sequence kind. Currently, the terminator token kinds only contain the necessary token to end the list and not necessarily the ones which might help in error recovery. This will be updated as I go through the testing phase. This phase is basically coming up with a bunch of invalid programs to check how the parser is acting and how can we help in the recovery phase.


## Test Plan

Currently, my plan is to keep the testing part separate than the actual update. This doesn't mean I'm not testing locally, but it's not thorough. The main reason is to keep the diffs to a minimal and writing test cases will require some effort which I want to decouple with the actual change. This is ok here as it's not getting merged into `main` but the parser PR.


---

_Label `parser` added by @dhruvmanila on 2024-03-07 08:51_

---

_@dhruvmanila reviewed on 2024-03-08 02:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:22 on 2024-03-08 02:17_

This is still incomplete, I'm updating as I go through every parse functions.

---

_@dhruvmanila reviewed on 2024-03-08 02:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:814 on 2024-03-08 02:19_

I might update the terminator tokens when I write some thorough test cases for invalid programs for better error recovery. But, for now, it's only limited to all valid programs.

---

_@dhruvmanila reviewed on 2024-03-08 02:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:921 on 2024-03-08 02:20_

Same as above. The error messages will possibly be improved with new invalid program test cases.

---

_@dhruvmanila reviewed on 2024-03-08 02:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:256 on 2024-03-08 02:20_

In a follow-up PR.

---

_@dhruvmanila reviewed on 2024-03-08 02:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:361 on 2024-03-08 02:21_

Probably not required as we discussed to move "soft" syntax errors into the linter.

(There are other TODOs in this PR)

---

_Comment by @dhruvmanila on 2024-03-08 02:36_

Clippy says that `at_expr_end` isn't used but it'll be used at a later stage. So, ignore that for now. There are no other clippy errors apart from this.

---

_Marked ready for review by @dhruvmanila on 2024-03-08 02:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-08 02:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:446 on 2024-03-08 08:16_

This is excellent

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:40 on 2024-03-08 08:18_

Is this intentional? You don't have to reply to this comment. I just want to double check that it is.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:600 on 2024-03-08 08:21_

I recommend using `parse_comma_separated_list` here because I don't know if the compiler recognize that it can directly move the elements into `slices` without cloning the expressions (which is what `extend_from_slice` does internally). 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:531 on 2024-03-08 08:24_

This is not related to this PR and more feedback for myself. I just had to navigate to the definition to figure out what the meaning of `true` is here. I don't think it's a problem in IDEs but it's difficult to know the meaning when reading the code. Two suggestions (for a separate PR)

* Create an enum `TrailingComma` with variants `Allowed`, `Forbidden`
* We could move it into `RecoveryContextKind` because it should be the same in all places where we use the same kind.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1284 on 2024-03-08 08:25_

Nit: Use `parse_comma_separated_list` to avoid cloning the list elements.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1310 on 2024-03-08 08:26_

Nit: Use `parse_comma_separated_list`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:230 on 2024-03-08 08:32_

What's the motivation for early returning when we see a `{}`. Doesn't the `parse_comma_separated_list` handle this automatically for us because it exits the loop immediately when seeing the `}` (list terminator)?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:355 on 2024-03-08 08:32_

Thank you :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:480 on 2024-03-08 08:36_

Nit: I'm happy to go with what we have today. I just want to throw this out here in case you like this more: How about `parse_list` (parses into a vec) and `parse_list_items` (only parses the items). Although the difference is probably to subtle.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:564 on 2024-03-08 08:38_

Yeah I think that's true by having a `while !self.at(TokenKind::EndOfFile) {}`. I'm just a big fan of `loop` :D

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:746 on 2024-03-08 08:39_

Thank you for writing all this documentation. That's very helpful!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:716 on 2024-03-08 08:42_

Nit: I don't think that's still needed, I should have removed it. I experimented with directly casting between kind and the bitflags and that requires a repr but we don't do this anymore.
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:921 on 2024-03-08 08:45_

Makes sense. What is unclear to me is if we should have a dedicated variant for each possible syntax error in `ParseErrorType` instead of using `OtherError` for everything. But that's nothing that we need to change in this PR. More something to think about.

---

_@MichaReiser approved on 2024-03-08 08:46_

This is great! Thanks for your careful work. 

It might be worth running a perf benchmark as part of the test plan for now until we get CodeSpeed reports. Just to make sure we don't introduce any significant perf regressions.

---

_@dhruvmanila reviewed on 2024-03-08 17:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:921 on 2024-03-08 17:27_

Yeah, I do plan to take a closer look at the error types in another PR.

---

_@dhruvmanila reviewed on 2024-03-08 17:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:230 on 2024-03-08 17:38_

Not much, mainly that it's good to have. Plus I saw this pattern elsewhere so thought it made sense here.

---

_@dhruvmanila reviewed on 2024-03-08 17:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:564 on 2024-03-08 17:42_

Yeah, I like the `loop` version myself, just want consistency so that it's easier to understand both as they're similar :)

---

_Merged by @dhruvmanila on 2024-03-08 17:52_

---

_Closed by @dhruvmanila on 2024-03-08 17:52_

---

_Branch deleted on 2024-03-08 17:52_

---
