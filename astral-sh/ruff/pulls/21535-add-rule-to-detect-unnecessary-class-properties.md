```yaml
number: 21535
title: Add rule to detect unnecessary class properties
type: pull_request
state: merged
author: ShaharNaveh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: property-no-return
created_at: 2025-11-20T08:49:43Z
updated_at: 2025-11-26T08:31:22Z
url: https://github.com/astral-sh/ruff/pull/21535
synced_at: 2026-01-10T16:48:02Z
```

# Add rule to detect unnecessary class properties

---

_Pull request opened by @ShaharNaveh on 2025-11-20 08:49_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/21403

## Test Plan

Added tests

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 09:01_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/53c5e30e7149019d18fcec9e34e62923213500fd/pandas/tests/io/excel/test_writers.py#L1708'>pandas/tests/io/excel/test_writers.py:1708:17:</a> RUF069 `sheets` is a property without a `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/94f4922d9a73236d88d637e71316ceb446695158/testing/python/raises.py#L346'>testing/python/raises.py:346:17:</a> RUF069 `__class__` is a property without a `return` statement
+ <a href='https://github.com/pytest-dev/pytest/blob/94f4922d9a73236d88d637e71316ceb446695158/testing/test_compat.py#L117'>testing/test_compat.py:117:13:</a> RUF069 `__class__` is a property without a `return` statement
+ <a href='https://github.com/pytest-dev/pytest/blob/94f4922d9a73236d88d637e71316ceb446695158/testing/test_compat.py#L91'>testing/test_compat.py:91:9:</a> RUF069 `raise_fail_outcome` is a property without a `return` statement
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF069 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>





---

_Converted to draft by @ShaharNaveh on 2025-11-20 09:10_

---

_Renamed from "Add rule to detect class properties without a return statement" to "Add rule to detect unnecessary class properties" by @ShaharNaveh on 2025-11-20 11:31_

---

_Marked ready for review by @ShaharNaveh on 2025-11-20 11:39_

---

_Label `rule` added by @amyreese on 2025-11-20 16:40_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:3 on 2025-11-20 18:23_

Would you mind adding inline comments marking each case as okay or an error instead of one comment at the top? That way it will appear in each of the snapshots. That makes it a bit easier to review.

And could you renumber the rule to RUF069? I think we have PRs in flight for [66](https://github.com/astral-sh/ruff/pull/9911), [67](https://github.com/astral-sh/ruff/pull/20585), and [68](https://github.com/astral-sh/ruff/pull/21079) already.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:16 on 2025-11-20 18:25_

Should this apply to yield and raises? I kind of expected it only return to suppress the diagnostic, but maybe yield and raise are more common than I thought.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:11 on 2025-11-20 18:25_

"Unnecessary" doesn't feel like exactly the right description to me. I think something like `property-without-return` for the rule name, along the lines of the issue title, might make more sense.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:106 on 2025-11-20 18:50_

nit: Can we early return here too, like in `visit_expr` above?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:109 on 2025-11-20 18:52_

Ah okay, this probably answers my question above about the raise and yield. I was mostly just curious.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:126 on 2025-11-20 18:55_

I think it's _probably_ okay to skip this since we bail out of `visit_stmt` itself, but it's also okay with me either way.

---

_@ntBre requested changes on 2025-11-20 19:03_

Thanks! This looks good to me overall, just some minor nits and a suggestion about the rule name and code.

---

_Label `preview` added by @ntBre on 2025-11-20 19:04_

---

_@ShaharNaveh reviewed on 2025-11-21 09:00_

---

_Review comment by @ShaharNaveh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:16 on 2025-11-21 09:00_

Raise is more common than yields. The reason why I included `yield` is because of the ecosystem CI report that showed an [instance where apache-airflow would use `yield` inside a property called "stream"](https://github.com/apache/airflow/blob/e9d41dc60d3e88b838b8de6464a1322f2667d078/airflow-core/src/airflow/utils/log/log_stream_accumulator.py#L111-L136) and that made a lot of sense to me, so I decided to include it.

I can remove the `yield` inclusion if that's desired

---

_@ShaharNaveh reviewed on 2025-11-21 09:02_

---

_Review comment by @ShaharNaveh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:11 on 2025-11-21 09:02_

The initial name was something like that, but I decided it's no longer accurate after the inclusion of `raise` and `yield`, would you still want me to revert it to the old name?

---

_@ShaharNaveh reviewed on 2025-11-21 09:37_

---

_Review comment by @ShaharNaveh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:126 on 2025-11-21 09:37_

Good catch. I prefer to have less code as possible

---

_Review requested from @ntBre by @ShaharNaveh on 2025-11-21 09:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:37 on 2025-11-21 19:29_

tiny nit since I did a release this morning:


```suggestion
#[violation_metadata(preview_since = "0.14.7")]
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:16 on 2025-11-21 19:32_

Thanks, yeah that makes sense to me. I chatted with @amyreese, and I think it makes sense to her too, so it sounds good to leave all three in the rule.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_property.rs`:11 on 2025-11-21 19:35_

Ah, I see what you mean, but I think it's still okay for the name and diagnostic message only to mention `return`. @amyreese had a good point that `return` will probably be the most common fix when the diagnostic fires too, so it seems good to emphasize that option.

---

_@ntBre requested changes on 2025-11-21 19:37_

Thank you, this looks great!

I think we just need to bump the `preview_since` version and update the rule name/docs and this is good to go.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/rules/property_without_return.rs`:82 on 2025-11-24 16:45_

Sorry to add one more change needed. We should pick a smaller range for the diagnostic, rather than throwing a yellow squiggle on an entire function in an editor, as changing the diagnostic range later will potentially be a [breaking change](https://github.com/astral-sh/ruff/issues/14900#issuecomment-2543923625).

I think I'd suggest just marking the `@property` decorator itself, since that's the part that's unfulfilled by the body of the function.

---

_@amyreese requested changes on 2025-11-24 16:46_

---

_@MichaReiser reviewed on 2025-11-24 17:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/property_without_return.rs`:82 on 2025-11-24 17:49_

In some cases, we highlight the range of the function's name. You can see other usages by searching for references to the `Identifier` trait

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_python_ast/src/identifier.rs#L18-L21

---

_@ShaharNaveh reviewed on 2025-11-25 08:31_

---

_Review comment by @ShaharNaveh on `crates/ruff_linter/src/rules/ruff/rules/property_without_return.rs`:82 on 2025-11-25 08:31_

Thank you @amyreese & @MichaReiser for the suggestions.

I agree with highlighting only one line instead of the entire function block. 

I ended up highlighting only the function name (@MichaReiser suggestion), just because it's a personal preference of mine. LMK if you want me to change that to highlight something else instead

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-11-25 08:32_

---

_Review requested from @amyreese by @ShaharNaveh on 2025-11-25 08:32_

---

_Review requested from @ntBre by @ShaharNaveh on 2025-11-25 08:32_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/rules/property_without_return.rs`:46 on 2025-11-25 22:04_

I think we prefer to put the name in backticks for clarity:

```suggestion
        format!("`{name}` is a property without a `return` statement")
```

---

_@amyreese approved on 2025-11-25 22:05_

One last nit, but otherwise looks good to me!

---

_Review requested from @amyreese by @ShaharNaveh on 2025-11-26 07:55_

---

_Comment by @MichaReiser on 2025-11-26 08:24_

Thanks this looks good. I just noticed that you recoded the rule from RUF066 to RUF069 before. Can you tell me why? It seems to me that RUF066 is still available.

---

_Comment by @MichaReiser on 2025-11-26 08:26_

Oh, never mind. It's because there are other in-flight PRs using that code. I recoded it back to RUF066, because I want to avoid holes and your PR is going to land first :)

---

_@MichaReiser approved on 2025-11-26 08:26_

---

_Review request for @ntBre removed by @MichaReiser on 2025-11-26 08:30_

---

_Review requested from @ntBre by @MichaReiser on 2025-11-26 08:30_

---

_Merged by @MichaReiser on 2025-11-26 08:31_

---

_Closed by @MichaReiser on 2025-11-26 08:31_

---
