```yaml
number: 18964
title: "Autofix for `none-not-at-end-of-union` #15136 (RUF036)"
type: pull_request
state: closed
author: jordyjwilliams
labels:
  - fixes
  - preview
assignees: []
base: main
head: 15136_autofix_none_union
created_at: 2025-06-26T16:23:33Z
updated_at: 2025-11-21T08:03:35Z
url: https://github.com/astral-sh/ruff/pull/18964
synced_at: 2026-01-10T16:48:01Z
```

# Autofix for `none-not-at-end-of-union` #15136 (RUF036)

---

_Pull request opened by @jordyjwilliams on 2025-06-26 16:23_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
* Should hopefully allow for closure of #15136.
    * Implements `autofix` for `none-not-at-end-of-union`
    * Takes [previous PR feedback on board ](https://github.com/astral-sh/ruff/pull/15139)
    * https://github.com/astral-sh/ruff/pull/15139#discussion_r1899557275
    * https://github.com/astral-sh/ruff/pull/15139#issuecomment-2561914430

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
* Test cases added.
* I am still fiarly new to `rust` and `ruff` contributions in general.
* I may have very much dropped the ball here or missed something. But keen to learn.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-26 20:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/09736ee42cebc3b4c752f05626ac6f3bcc54b722/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
- <a href='https://github.com/apache/superset/blob/09736ee42cebc3b4c752f05626ac6f3bcc54b722/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -3 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/09736ee42cebc3b4c752f05626ac6f3bcc54b722/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
- <a href='https://github.com/apache/superset/blob/09736ee42cebc3b4c752f05626ac6f3bcc54b722/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/428c27694841de692ed305b33a871cf0d89fe33a/libs/core/langchain_core/language_models/llms.py#L158'>libs/core/langchain_core/language_models/llms.py:158:44:</a> PYI016 [*] Duplicate union member `None`
- <a href='https://github.com/langchain-ai/langchain/blob/428c27694841de692ed305b33a871cf0d89fe33a/libs/core/langchain_core/language_models/llms.py#L194'>libs/core/langchain_core/language_models/llms.py:194:44:</a> PYI016 [*] Duplicate union member `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |
| PYI016 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-06-27 17:39_

---

_Comment by @ntBre on 2025-07-28 21:26_

I realized this has been in my notification inbox for quite a while, so I wanted to say thanks for your work on this! I'm still planning to review it, I just need to set aside a good chunk of time to go through the issue and previous PR reviews as well as the new changes.

---

_Label `fixes` added by @ntBre on 2025-07-28 21:27_

---

_Label `preview` added by @ntBre on 2025-07-28 21:27_

---

_Comment by @jordyjwilliams on 2025-07-28 23:13_

@ntBre all good. Happy to address any changes or refactor the approach if needed. Was somewhat a combination and extension of closed PRs.

Being totally honest I am pretty new to rust and ruff as a repo so I may have missed something here!

---

_Comment by @MichaReiser on 2025-08-18 07:40_

@ntBre could you try to take a look at this PR the coming week?

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF036.py`:1 on 2025-08-29 18:14_

Would you mind pulling in the tests from the previous PR? I think that would make it a little easier for me to compare between the two.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:89 on 2025-08-29 18:16_

Nice, I think this is a great idea to cut the scope.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:113 on 2025-08-29 18:18_

I don't think we can build the union like this, unfortunately. We need to handle the case of a `typing.Union` and preserve that union style, not always replace it with a PEP-604-style union. Micha's patch [here](https://github.com/astral-sh/ruff/pull/15139#pullrequestreview-2525607890) might be helpful, although he said there were still some edge cases we might have to watch out for.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:91 on 2025-08-29 18:21_

I also agree with Micha about just emitting a single diagnostic for the whole union instead of one diagnostic per `none_expr` (https://github.com/astral-sh/ruff/pull/15139#discussion_r1899557275). It might make sense to spin that off into a small, separate PR. Basically we'd just call `checker.report_diagnostic` once and I guess pass in the whole union range. And we can do that without any special preview checks because the rule itself is in preview.

That won't make this PR much easier, but at least we can move the fix generation out of the loop.

---

_@ntBre requested changes on 2025-08-29 18:26_

Thanks for working on this and apologies again for the delay in reviewing. I have a few high-level suggestions/requests here.

This is mostly for my future reference, but I also tried to pull out a list of things to look for based on the previous reviews:
- [ ] single diagnostic and fix for the whole union (https://github.com/astral-sh/ruff/pull/15139#discussion_r1899557275)
- [ ] additional mixed union tests (https://github.com/astral-sh/ruff/pull/15139#issuecomment-2561914430)
- [ ] double check ecosystem check doesn't remove violations (https://github.com/astral-sh/ruff/pull/15139#issuecomment-2561916861)
- [ ] avoid removing duplicate None entries (https://github.com/astral-sh/ruff/pull/15139#discussion_r1899557748)
- [ ] avoid flattening nested U types (https://github.com/astral-sh/ruff/pull/15139#discussion_r1899561169)

I think you've already handled the last two of these 👍 

---

_Comment by @MichaReiser on 2025-11-21 08:03_

Thanks for your work on this. I'll close this PR due to inactivity but we're happy to take a look at a re-submission that accounts for Brent's feedback

---

_Closed by @MichaReiser on 2025-11-21 08:03_

---
