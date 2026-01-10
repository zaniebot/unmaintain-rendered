```yaml
number: 11950
title: Enable token-based rules on source with syntax errors
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/token-rules-with-syntax-errors
created_at: 2024-06-20T11:46:30Z
updated_at: 2024-07-02T09:11:40Z
url: https://github.com/astral-sh/ruff/pull/11950
synced_at: 2026-01-10T21:56:00Z
```

# Enable token-based rules on source with syntax errors

---

_Pull request opened by @dhruvmanila on 2024-06-20 11:46_

## Summary

This PR updates the linter, specifically the token-based rules, to work on the tokens that come after a syntax error.

For context, the token-based rules only diagnose the tokens up to the first lexical error. This PR builds up an error resilience by introducing a `TokenIterWithContext` which updates the `nesting` level and tries to reflect it with what the lexer is seeing. This isn't 100% accurate because if the parser recovered from an unclosed parenthesis in the middle of the line, the context won't reduce the nesting level until it sees the newline token at the end of the line.

resolves: #11915

## Test Plan

* Add test cases for a bunch of rules that are affected by this change.
* Run the fuzzer for a long time, making sure to fix any other bugs.


---

_Label `rule` added by @dhruvmanila on 2024-06-20 11:46_

---

_@dhruvmanila reviewed on 2024-06-28 05:04_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/tokens.rs`:97 on 2024-06-28 05:04_

This is looking at string tokens and the lexer doesn't emit them if it's unterminated. So, we might get away with not doing anything in this case for now.

---

_@dhruvmanila reviewed on 2024-06-28 05:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/doc_lines.rs`:29 on 2024-06-28 05:11_

This extracts a specific set of comments so it doesn't require any specific update.

---

_@dhruvmanila reviewed on 2024-06-28 05:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:260 on 2024-06-28 05:14_

I think this should work as the newline tokens within f-strings are actually `NonLogicalNewline`, I'll move this into `TokenIterWithContext`. I'll test this a lot because f-strings are complex.

---

_@dhruvmanila reviewed on 2024-06-28 05:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/directives.rs`:115 on 2024-06-28 05:58_

The token stream doesn't contain the `EndOfFile` token.

---

_Comment by @github-actions[bot] on 2024-06-28 06:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
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
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-07-01 11:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/token-rules-with-syntax-errors)

### Merging #11950 will **not alter performance**

<sub>Comparing <code>dhruv/token-rules-with-syntax-errors</code> (2e932e3) with <code>main</code> (88a4cc4)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Marked ready for review by @dhruvmanila on 2024-07-01 14:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-01 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:502 on 2024-07-02 05:55_

This is an improvement even without the error recoverability :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:564 on 2024-07-02 05:57_

That's simpler than I expected. Nice

---

_@MichaReiser approved on 2024-07-02 05:57_

This is nice!

---

_Merged by @dhruvmanila on 2024-07-02 08:57_

---

_Closed by @dhruvmanila on 2024-07-02 08:57_

---

_Branch deleted on 2024-07-02 08:57_

---
