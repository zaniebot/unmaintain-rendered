```yaml
number: 10036
title: Replace LALRPOP parser with hand-written parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - performance
  - parser
assignees: []
merged: true
base: main
head: dhruv/parser
created_at: 2024-02-19T04:13:35Z
updated_at: 2024-04-18T19:48:00Z
url: https://github.com/astral-sh/ruff/pull/10036
synced_at: 2026-01-12T15:55:31Z
```

# Replace LALRPOP parser with hand-written parser

---

_@dhruvmanila_

(Supersedes #9152, authored by @LaBatata101)

## Summary

This PR replaces the current parser generated from LALRPOP to a hand-written recursive descent parser.

It also updates the grammar for [PEP 646](https://peps.python.org/pep-0646/) so that the parser outputs the correct AST. For example, in `data[*x]`, the index expression is now a tuple with a single starred expression instead of just a starred expression.

Beyond the performance improvements, the parser is also error resilient and can provide better error messages. The behavior as seen by any downstream tools isn't changed. That is, the linter and formatter can still assume that the parser will _stop_ at the first syntax error. This will be updated in the following months.

For more details about the change here, refer to the PR corresponding to the individual commits and the [release blog post](https://astral.sh/blog/ruff-v0.4.0).

## Test Plan

Write _lots_ and _lots_ of tests for both valid and invalid syntax and verify the output.

## Acknowledgements

- @MichaReiser for reviewing 100+ parser PRs and continuously providing guidance throughout the project
- @LaBatata101 for initiating the transition to a hand-written parser in #9152 
- @addisoncrump for implementing the fuzzer which helped [catch](https://github.com/astral-sh/ruff/pull/10903) [a](https://github.com/astral-sh/ruff/pull/10910) [lot](https://github.com/astral-sh/ruff/pull/10966) [of](https://github.com/astral-sh/ruff/pull/10896) [bugs](https://github.com/astral-sh/ruff/pull/10877)

---

_Label `parser` added by @dhruvmanila on 2024-02-19 04:13_

---

_Converted to draft by @dhruvmanila on 2024-02-19 04:16_

---

_Comment by @codspeed-hq[bot] on 2024-03-07 10:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/parser)

### Merging #10036 will **improve performances by ×2.4**

<sub>Comparing <code>dhruv/parser</code> (98a7669) with <code>main</code> (e09180b)</sub>



### Summary

`⚡ 7` improvements
`✅ 23` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/parser` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 9.6 ms | 8.6 ms | +11.63% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 11.1 ms | 10.1 ms | +9.95% |
| ⚡ | `parser[large/dataset.py]` | 63.6 ms | 26.6 ms | ×2.4 |
| ⚡ | `parser[numpy/ctypeslib.py]` | 10.8 ms | 5 ms | ×2.2 |
| ⚡ | `parser[numpy/globals.py]` | 964.6 µs | 424.9 µs | ×2.3 |
| ⚡ | `parser[pydantic/types.py]` | 24.4 ms | 10.9 ms | ×2.2 |
| ⚡ | `parser[unicode/pypinyin.py]` | 3.8 ms | 1.7 ms | ×2.2 |


---

_Comment by @github-actions[bot] on 2024-03-12 12:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/ab3a6a1d58ab6943adf406e58d12c9b1b1cddfc1/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:12:</a> E999 SyntaxError: Multiple exception types must be parenthesized
- <a href='https://github.com/demisto/content/blob/ab3a6a1d58ab6943adf406e58d12c9b1b1cddfc1/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:21:</a> E999 SyntaxError: Unexpected token ','
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/ab3a6a1d58ab6943adf406e58d12c9b1b1cddfc1/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:12:</a> E999 SyntaxError: Multiple exception types must be parenthesized
- <a href='https://github.com/demisto/content/blob/ab3a6a1d58ab6943adf406e58d12c9b1b1cddfc1/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:21:</a> E999 SyntaxError: Unexpected token ','
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Added to milestone `v0.4.0` by @dhruvmanila on 2024-04-15 13:49_

---

_Comment by @MichaReiser on 2024-04-17 19:03_

<img src="https://media3.giphy.com/media/bZBmitwUwKtDa/200w.gif?cid=6c09b952wajidy9mwmiqqi2y0rvn0o7tktanlk5g60j4p2kv&ep=v1_gifs_search&rid=200w.gif&ct=g" />

Ship It! ;)

---

_Label `performance` added by @dhruvmanila on 2024-04-18 10:05_

---

_Marked ready for review by @dhruvmanila on 2024-04-18 10:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-18 10:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:237 on 2024-04-18 10:43_

Do we still need this `Invalid`?

---

_@MichaReiser approved on 2024-04-18 10:44_

---

_Comment by @MichaReiser on 2024-04-18 10:45_

One obligatory comment :P Excellent work. Looking forward to my next git pull

---

_@dhruvmanila reviewed on 2024-04-18 10:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:237 on 2024-04-18 10:55_

How did you even find this? xD

No, I'll remove it.

---

_Comment by @addisoncrump on 2024-04-18 11:29_

:rocket: Let me know if I can help with further testing.

---

_Merged by @dhruvmanila on 2024-04-18 12:27_

---

_Closed by @dhruvmanila on 2024-04-18 12:27_

---

_Branch deleted on 2024-04-18 12:27_

---
