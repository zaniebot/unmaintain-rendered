```yaml
number: 15929
title: "[`flake8-comprehensions`] Handle trailing comma in fixes for `unnecessary-generator-list/set` (`C400`,`C401`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: generator-comma
created_at: 2025-02-04T11:31:04Z
updated_at: 2025-02-12T04:34:28Z
url: https://github.com/astral-sh/ruff/pull/15929
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-comprehensions`] Handle trailing comma in fixes for `unnecessary-generator-list/set` (`C400`,`C401`)

---

_Pull request opened by @dylwil3 on 2025-02-04 11:31_

The unsafe fixes for the rules [unnecessary-generator-list (C400)](https://docs.astral.sh/ruff/rules/unnecessary-generator-list/#unnecessary-generator-list-c400) and [unnecessary-generator-set (C401)](https://docs.astral.sh/ruff/rules/unnecessary-generator-set/#unnecessary-generator-set-c401) used to introduce syntax errors if the argument to `list` or `set` had a trailing comma, because the fix would retain the comma after transforming the function call to a comprehension. 

This PR accounts for the trailing comma when replacing the end of the call with a `]` or `}`.

Closes #15852


---

_Label `bug` added by @dylwil3 on 2025-02-04 11:31_

---

_Label `fixes` added by @dylwil3 on 2025-02-04 11:31_

---

_Comment by @github-actions[bot] on 2025-02-04 11:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs`:136 on 2025-02-05 08:48_

```suggestion
        let right_bracket_loc = tokenizer
            .find(|token| token.kind == SimpleTokenKind::Comma)
            .map_or(call.arguments.end(), |comma| {
                comma.end()
            }) - TextSize::from(1);
```


or 

```suggestion
        let right_bracket_loc = tokenizer
            .find(|token| token.kind == SimpleTokenKind::Comma)
            .map_or(call.arguments.end() - ')'.text_len(), Ranged::start);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs`:131 on 2025-02-05 08:49_

```suggestion
        let mut tokenizer = SimpleTokenizer::new(
        		checker.source(), // Not sure if this method exists :D
        		TextRange::new(argument.end(), call.end())
        );
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_set.rs`:131 on 2025-02-05 08:49_

Same as for the other rule

---

_@MichaReiser approved on 2025-02-05 08:50_

Nice, I've only two nit comments that apply to both rules

---

_Renamed from "[`flake8-comprehensions`] Handle trailing comma in fixes for `unnecessary-generator-list/comma` (`C400`,`C401`)" to "[`flake8-comprehensions`] Handle trailing comma in fixes for `unnecessary-generator-list/set` (`C400`,`C401`)" by @dylwil3 on 2025-02-05 13:37_

---

_Merged by @dylwil3 on 2025-02-05 13:38_

---

_Closed by @dylwil3 on 2025-02-05 13:38_

---

_Branch deleted on 2025-02-05 13:38_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs`:134 on 2025-02-12 04:34_

I was wondering if we can use `checker.tokens().in_range(...)` instead here?

---

_@dhruvmanila reviewed on 2025-02-12 04:34_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_set.rs`:136 on 2025-02-12 04:34_

Same as above.

---

_@dhruvmanila reviewed on 2025-02-12 04:34_

---
