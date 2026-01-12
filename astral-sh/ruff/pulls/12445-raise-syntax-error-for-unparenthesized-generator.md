```yaml
number: 12445
title: Raise syntax error for unparenthesized generator expr in multi-argument call
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/unparenthesized-genexp
created_at: 2024-07-22T03:37:36Z
updated_at: 2024-07-22T09:14:22Z
url: https://github.com/astral-sh/ruff/pull/12445
synced_at: 2026-01-12T15:55:41Z
```

# Raise syntax error for unparenthesized generator expr in multi-argument call

---

_@dhruvmanila_

## Summary

This PR fixes a bug to raise a syntax error when an unparenthesized generator expression is used as an argument to a call when there are more than one argument.

For reference, the grammar is:
```
primary:
    | ...
    | primary genexp 
    | primary '(' [arguments] ')' 
    | ...

genexp:
    | '(' ( assignment_expression | expression !':=') for_if_clauses ')' 
```

The `genexp` requires the parenthesis as mentioned in the grammar. So, the grammar for a call expression is either a name followed by a generator expression or a name followed by a list of argument. In the former case, the parenthesis are excluded because the generator expression provides them while in the later case, the parenthesis are explicitly provided for a list of arguments which means that the generator expression requires it's own parenthesis.

This was discovered in https://github.com/astral-sh/ruff/issues/12420.

## Test Plan

Add test cases for valid and invalid syntax.

Make sure that the parser from CPython also raises this at the parsing step:
```console
$ python3.13 -m ast parser/_.py
  File "parser/_.py", line 1
    total(1, 2, x for x in range(5), 6)
                ^^^^^^^^^^^^^^^^^^^
SyntaxError: Generator expression must be parenthesized

$ python3.13 -m ast parser/_.py
  File "parser/_.py", line 1
    sum(x for x in range(10), 10)
        ^^^^^^^^^^^^^^^^^^^^
SyntaxError: Generator expression must be parenthesized
```

---

_Label `bug` added by @dhruvmanila on 2024-07-22 03:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-22 03:37_

---

_Comment by @github-actions[bot] on 2024-07-22 04:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_canvaslms.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:17:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_canvaslms.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:17:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_@MichaReiser approved on 2024-07-22 07:24_

---

_Merged by @dhruvmanila on 2024-07-22 09:14_

---

_Closed by @dhruvmanila on 2024-07-22 09:14_

---

_Branch deleted on 2024-07-22 09:14_

---
