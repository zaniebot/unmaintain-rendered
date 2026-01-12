```yaml
number: 7588
title: "Separate `Q003` to accomodate f-string context"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/issue-7297
created_at: 2023-09-22T05:14:22Z
updated_at: 2023-09-28T03:58:59Z
url: https://github.com/astral-sh/ruff/pull/7588
synced_at: 2026-01-12T02:39:10Z
```

# Separate `Q003` to accomodate f-string context

---

_Pull request opened by @dhruvmanila on 2023-09-22 05:14_

## Summary

This PR updates the `Q003` rule to accommodate the new f-string context. The logic here takes into consideration the nested f-strings and the configured target version.

The rule checks for escaped quotes within a string and determines if they are avoidable or not. It is avoidable if:
1. Outer quote matches the user preferred quote
2. Not a raw string
3. Not a triple-quoted string
4. String content contains the same quote as the outer one
5. String content _doesn't_ contain the opposite quote

For f-string, the way it works is by using a context stack to keep track of certain things but mainly the text range (`FStringMiddle`) where the escapes exists. It contains the following:

1. Do we want to check for escaped quotes in the current f-string? This is required to:
  	* Preserve the context for `FStringMiddle` tokens where we need to check for escaped quotes. But, the answer to whether we need to check or not lies with the `FStringStart` token which contains the quotes. So, when the context starts, we'll store this information.
  	* Disallow nesting for pre 3.12 target versions
2. Store the `FStringStart` token range. This is required to create the edit to replace the quote if this f-string contains escaped quote(s).
3. All the `FStringMiddle` ranges where there are escaped quote(s).

## Test Plan

* Add new test cases for nested f-strings.
* Write new tests for old Python versions as existing ones test it on the latest version by default which is 3.12 as of this writing.
* Verify the snapshots

---

_Comment by @dhruvmanila on 2023-09-22 05:14_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7588?utm_source=stack-comment).

---

_Label `rule` added by @dhruvmanila on 2023-09-22 05:54_

---

_Label `python312` added by @dhruvmanila on 2023-09-22 05:54_

---

_Marked ready for review by @dhruvmanila on 2023-09-22 05:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-22 05:55_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-09-22 05:55_

---

_Comment by @MichaReiser on 2023-09-22 13:39_

I hope this needs a rebase xD 16k lines sounds painful to review

---

_Comment by @dhruvmanila on 2023-09-22 13:48_

> I hope this needs a rebase xD 16k lines sounds painful to review

Oh wait, what happened here lol. Let me rebase.

Edit: forgot to push the rebased branch.

---

_Comment by @codspeed-hq[bot] on 2023-09-22 13:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/issue-7297)

### Merging #7588 will **degrade performances by 2.33%**

<sub>Comparing <code>dhruv/issue-7297</code> (7753ce8) with <code>dhruv/pep-701</code> (917cace)</sub>



### Summary

`❌ 2` regressions
`✅ 23` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/issue-7297)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/pep-701` | `dhruv/issue-7297` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 72.3 ms | 74 ms | -2.33% |
| ❌ | `linter/all-rules[large/dataset.py]` | 162.7 ms | 166.6 ms | -2.31% |


---

_Review request for @MichaReiser removed by @dhruvmanila on 2023-09-25 09:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-25 09:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/tokens.rs`:123 on 2023-09-26 06:24_

What's the reason for moving the rule out of `from_tokens`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:67 on 2023-09-26 06:27_

I would have expected a `enter_`...`exit_...` function pair How does it start checking for quotes again? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:50 on 2023-09-26 06:29_

When I understand this correctly, we allocate a `fstring_middle_ranges` for each f-string. Can we flatten this datastructure so that it uses a single vector instead of N nested vectors?

From what I understand, it may be sufficient to track the fstring middle ranges for the outermost fstring only (just push the ranges of the inner fstring middles into the same vec)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:109 on 2023-09-26 06:33_

In case this is a nested string or f-string: We disable the tracking for the entire outer fstring. Isn't this too late in case we already flagged an escape early in the fstring?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:175 on 2023-09-26 06:37_

Nit: The name of the method is misleading (or the field name is). It doesn't track **all** fstring middle parts. It tracks the fstring middle parts that contain the preferred quotes only. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:223 on 2023-09-26 06:38_

```suggestion
fn unescape_string(value: &str) -> String {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:226 on 2023-09-26 06:39_

I don't think it's necessary to allocate here. We should be able to implement this with `Peekable`/`Cursor`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:86 on 2023-09-26 06:40_

This logic is fairly involved. I wonder if it would be easier to understand if we split the logic in two parts:

1. Hot loop for when not inside of an f-string
2. Loop that process an fstring (or skips it if pep701 isn't supported)



---

_@MichaReiser reviewed on 2023-09-26 06:42_

LGTM although the code is  a bit difficult to understand. I would love to get @charliermarsh's opinion on too because I'm not very familiar with the rule

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/tokens.rs`:123 on 2023-09-26 10:19_

The idea was that the escaped quote check needs to be done in the content value only i.e., for f-string it's only the `FStringMiddle` tokens. 

The f-string context which needs to be tracked for this rule is different than the one for the quote rules where the main difference is how implicitly concatenated strings are handled. For this rule, implicit string concatenation doesn't matter because we only care about the content while for the quote rules it matters.

For quote rules, each sequence needs to be independently built. For nested f-strings, the nested sequence will be different than the outer one and so on. This is the main reason for the separation. It becomes easier to reason about.

---

_@dhruvmanila reviewed on 2023-09-26 10:19_

---

_@dhruvmanila reviewed on 2023-09-26 10:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:67 on 2023-09-26 10:21_

We don't start again. For example,

```python
f"\"foo\" {'nested'}"
# ^^(1)^^^ ^^^(2)^^
```

Here, (1) will be checked first and the range will be added to `fstring_middle_ranges`. Then we encountered a nested string (2) but our target version doesn't support PEP 701, so we'll stop checking for the current f-string by inverting the flag and removing any previously diagnosed ranges.


---

_@dhruvmanila reviewed on 2023-09-26 10:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:109 on 2023-09-26 10:24_

Yes, that's why the `do_not_check_for_escaped_quote` method exists to stop checking any further and remove any existing ranges. Note that we don't extend the diagnostics until we reach the end of the f-string.

---

_@dhruvmanila reviewed on 2023-09-26 10:33_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:226 on 2023-09-26 10:33_

I didn't look at the implementation in detail but yeah I agree that it can be implemented without allocation. Thanks!

---

_@dhruvmanila reviewed on 2023-09-26 11:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:50 on 2023-09-26 11:32_

We need to associate the edits for the f-string it belongs to. The context helps in preserving that information. So, once we're at the end of a f-string (could be nested) we'll pop the current f-string context which will give us the range of `FStringStart` token and all the `FStringMiddle` tokens which contains escaped quotes.

It could also be that the nested f-strings need not be checked but the outer one should be. So, once we're out of the nested f-string which will pop the context of that f-string, we need to reset the "check for escaped quote" field as per the context of the _now_ current f-string (the outer f-string).

---

_@dhruvmanila reviewed on 2023-09-26 11:44_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:86 on 2023-09-26 11:44_

I'm looking into this and I think this might simplify other logic as well.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:86 on 2023-09-26 12:22_

This is taking some time so I'm skipping this for now. I'll probably take this up later as a refactor.

---

_@dhruvmanila reviewed on 2023-09-26 12:22_

---

_@charliermarsh reviewed on 2023-09-26 19:46_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:226 on 2023-09-26 19:46_

Ah ok so this code already exists and was moved.

---

_@charliermarsh approved on 2023-09-26 19:55_

---

_@charliermarsh reviewed on 2023-09-26 19:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:64 on 2023-09-26 19:58_

Maybe: `ignore_escaped_quotes`?

---

_Merged by @dhruvmanila on 2023-09-27 07:57_

---

_Closed by @dhruvmanila on 2023-09-27 07:57_

---

_Branch deleted on 2023-09-27 07:57_

---
