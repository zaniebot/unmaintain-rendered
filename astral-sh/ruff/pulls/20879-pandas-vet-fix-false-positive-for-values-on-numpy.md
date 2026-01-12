```yaml
number: 20879
title: "[`pandas-vet`] Fix false positive for `.values` on NumPy `NamedTuples` (`PD011`)"
type: pull_request
state: closed
author: danparizher
labels:
  - bug
assignees: []
base: main
head: fix-20874
created_at: 2025-10-15T01:41:21Z
updated_at: 2026-01-02T17:12:54Z
url: https://github.com/astral-sh/ruff/pull/20879
synced_at: 2026-01-12T15:57:11Z
```

# [`pandas-vet`] Fix false positive for `.values` on NumPy `NamedTuples` (`PD011`)

---

_@danparizher_

## Summary

Fixes #20874 - False positive PD011: using `.values` on a NumPy NamedTuple, not a Pandas Series or Index object

## Problem Analysis

The PD011 rule (`pandas-use-of-dot-values`) was incorrectly flagging `.values` usage on non-pandas objects, particularly:

1. **NumPy NamedTuples** returned by functions like `numpy.unique_inverse()`, `numpy.unique_all()`, and `numpy.unique_counts()`
2. **Simple non-pandas objects** like integers, strings, or other data types that happen to have a `.values` attribute

The original issue reported a false positive where `unique.values` was flagged, where `unique` was a `UniqueInverseResult` NamedTuple from NumPy, not a pandas object.

## Approach

### General Solution Strategy

Rather than creating NumPy-specific logic, I enhanced the existing `test_expression` helper function in `crates/ruff_linter/src/rules/pandas_vet/helpers.rs` to be more intelligent about determining whether a binding comes from pandas-related sources.

### Implementation Details

1. **Enhanced `test_expression` Function**:
   - Added logic to check if variable bindings come from pandas-related sources
   - Uses `find_binding_value()` to trace back to the original assignment
   - Calls new `is_pandas_related_value()` function to determine pandas relevance

2. **New `is_pandas_related_value()` Function**:
   - Analyzes expressions to determine if they originate from pandas
   - Handles literals (numbers, strings, etc.) - returns `false` (not pandas-related)
   - Handles pandas imports - returns `true` (pandas-related)
   - Handles pandas method calls and function calls - returns `true`
   - Handles unknown expressions - returns `false` (conservative approach)

## Ecosystem Impact Analysis

The ecosystem bot reported **-17 violations** across 2 projects:

### apache/superset (-14 violations)

- **12 PD011 violations removed**: These were false positives where `.values` was accessed on non-pandas objects in pandas postprocessing modules
- **1 PD013 violation removed**: `.stack` usage that was incorrectly flagged
- **1 PD008 violation removed**: `.at` usage that was incorrectly flagged

### bokeh/bokeh (-3 violations)

- **1 PD011 violation removed**: `.values` usage on non-pandas object
- **1 PD013 violation removed**: `.stack` usage that was incorrectly flagged  
- **1 PD010 violation removed**: `.pivot` usage that was incorrectly flagged

---

_Comment by @github-actions[bot] on 2025-10-15 01:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -16 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/superset/common/query_context_processor.py#L187'>superset/common/query_context_processor.py:187:24:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/superset/common/query_context_processor.py#L225'>superset/common/query_context_processor.py:225:64:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L256'>tests/unit_tests/commands/databases/csv_reader_test.py:256:21:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L345'>tests/unit_tests/commands/databases/csv_reader_test.py:345:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L357'>tests/unit_tests/commands/databases/csv_reader_test.py:357:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L369'>tests/unit_tests/commands/databases/csv_reader_test.py:369:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L104'>tests/unit_tests/pandas_postprocessing/test_histogram.py:104:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L45'>tests/unit_tests/pandas_postprocessing/test_histogram.py:45:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L59'>tests/unit_tests/pandas_postprocessing/test_histogram.py:59:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L73'>tests/unit_tests/pandas_postprocessing/test_histogram.py:73:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L90'>tests/unit_tests/pandas_postprocessing/test_histogram.py:90:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/result_set_test.py#L164'>tests/unit_tests/result_set_test.py:164:12:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/heatmap_unemployment.py#L29'>examples/topics/categorical/heatmap_unemployment.py:29:19:</a> PD013 `.melt` is preferred to `.stack`; provides same functionality
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L188'>tests/unit/bokeh/models/test_sources.py:188:20:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L202'>tests/unit/bokeh/models/test_sources.py:202:20:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L214'>tests/unit/bokeh/models/test_sources.py:214:20:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD011 | 15 | 0 | 15 | 0 | 0 |
| PD013 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -16 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/superset/common/query_context_processor.py#L187'>superset/common/query_context_processor.py:187:24:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/superset/common/query_context_processor.py#L225'>superset/common/query_context_processor.py:225:64:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L256'>tests/unit_tests/commands/databases/csv_reader_test.py:256:21:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L345'>tests/unit_tests/commands/databases/csv_reader_test.py:345:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L357'>tests/unit_tests/commands/databases/csv_reader_test.py:357:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/commands/databases/csv_reader_test.py#L369'>tests/unit_tests/commands/databases/csv_reader_test.py:369:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L104'>tests/unit_tests/pandas_postprocessing/test_histogram.py:104:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L45'>tests/unit_tests/pandas_postprocessing/test_histogram.py:45:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L59'>tests/unit_tests/pandas_postprocessing/test_histogram.py:59:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L73'>tests/unit_tests/pandas_postprocessing/test_histogram.py:73:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/pandas_postprocessing/test_histogram.py#L90'>tests/unit_tests/pandas_postprocessing/test_histogram.py:90:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/be3690c22be743e8fbc0cf609feff285181dddb2/tests/unit_tests/result_set_test.py#L164'>tests/unit_tests/result_set_test.py:164:12:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/heatmap_unemployment.py#L29'>examples/topics/categorical/heatmap_unemployment.py:29:19:</a> PD013 `.melt` is preferred to `.stack`; provides same functionality
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L188'>tests/unit/bokeh/models/test_sources.py:188:20:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L202'>tests/unit/bokeh/models/test_sources.py:202:20:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L214'>tests/unit/bokeh/models/test_sources.py:214:20:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD011 | 15 | 0 | 15 | 0 | 0 |
| PD013 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pandas_vet/rules/attr.rs`:46 on 2025-10-15 21:44_

I don't think this is specific to numpy. A very simple [example](https://play.ruff.rs/90cd0ead-1783-400e-abf0-abe203f94b28) like this exhibits the same behavior:

```py
import pandas

p = 1
p.values
```

We also already have the `test_expression` helper that tries to filter out non-pandas bindings. It might be worth trying to improve that, which would be shared by the other pandas rules.

---

_@ntBre requested changes on 2025-10-15 21:44_

---

_Label `bug` added by @ntBre on 2025-10-15 21:44_

---

_Review requested from @ntBre by @danparizher on 2025-10-16 00:11_

---

_Comment by @MichaReiser on 2025-10-16 08:11_

Hi @danparizher. Thanks for working on this. 

If you want to help us as reviewers. What I'd find very useful is if you add a few more details to the PR summary explaining why you took this specific approach. What constraints did you consider? What are its up or downsides? 

I'd also find it very helpful if you could look at the ecosystem changes and summarize the result. This gives me confidence that there are no unexpected surprises and that this PR is ready to review. 

I understand that this will take more time on your side, but it will allow us to review your PRs more quickly.

---

_Comment by @danparizher on 2025-10-19 19:56_

Hi @MichaReiser, thanks for the feedback. I've updated the description for this PR and will continue to do so for future PRs!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pandas_vet/helpers.rs`:106 on 2025-10-30 17:06_

This looks a lot like the first part of the `match` in `test_expression`. Should we just recurse in `test_expression` for names, attributes, and calls? (And add `match` arms for attributes and calls like you have here)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pandas_vet/mod.rs`:272 on 2025-10-30 17:09_

Could we use the `paths` test runner just below this instead of `contents`? I think  it's a lot easier to read and modify files than these snippets.

You can probably put these all in one file if you isolate them in separate functions like:

```py
def f():
	import pandas as pd
	import numpy as np
	unique = np.unique_inverse([1, 2, 3, 2, 1])
	result = unique.values
```

at least I've seen that pattern in other cases.

---

_@ntBre requested changes on 2025-10-30 17:22_

I made a couple of inline comments, but then I started looking at the ecosystem check, and it looks like we've gone too far in the opposite direction. The first  two cases I clicked on:

- [superset/charts/client_processing.py:110:14:](https://github.com/apache/superset/blob/4ddc3f14ed32c99805d124814f5d3e76f1a9922d/superset/charts/client_processing.py#L110) PD013 `.melt` is preferred to `.stack`; provides same functionality
- [superset/utils/csv.py:79:21:](https://github.com/apache/superset/blob/4ddc3f14ed32c99805d124814f5d3e76f1a9922d/superset/utils/csv.py#L79) PD008 Use `.loc` instead of `.at`. If speed is important, use NumPy.

seem like clear false negatives, at least to me. I guess we're having trouble detecting  them because the function arguments have been shadowed, but the parameters themselves are even annotated as pandas dataframes.

This doesn't seem like an improvement to me and seems to be a step toward making the rules less useful, as Micha mentioned on the issue.

I would probably lean toward closing this, especially since the user closed the issue as well.



---

_Comment by @danparizher on 2025-11-01 17:31_

I pushed a change with your feedback. Feel free to close if it makes more sense, since the original issue was also closed.

---

_Review requested from @ntBre by @danparizher on 2025-11-01 17:31_

---

_Comment by @ntBre on 2026-01-02 16:59_

Thanks again for your work here and sorry for the delay in getting back to it. I think I will go ahead and close this for now based on the discussion above.

---

_Closed by @ntBre on 2026-01-02 16:59_

---

_Branch deleted on 2026-01-02 17:12_

---
