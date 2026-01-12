```yaml
number: 8463
title: "Use characters instead of `u32` in confusable map"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/chars
created_at: 2023-11-03T04:59:56Z
updated_at: 2023-11-03T14:01:21Z
url: https://github.com/astral-sh/ruff/pull/8463
synced_at: 2026-01-12T15:55:26Z
```

# Use characters instead of `u32` in confusable map

---

_@charliermarsh_

_No description provided._

---

_Review requested from @MichaReiser by @charliermarsh on 2023-11-03 04:59_

---

_Label `internal` added by @charliermarsh on 2023-11-03 05:00_

---

_Comment by @charliermarsh on 2023-11-03 05:00_

I don't feel strongly about this but it came up once before. Sadly it really buffs my commit stats.

---

_@charliermarsh reviewed on 2023-11-03 05:01_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:166 on 2023-11-03 05:01_

The motivation for doing this _now_ is that we can avoid this unwrap.

---

_@MichaReiser approved on 2023-11-03 05:11_

I like being explicit about type.

---

_Comment by @github-actions[bot] on 2023-11-03 05:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58'>pandas/_libs/tslibs/timedeltas.pyi:58:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352'>pandas/core/indexes/base.py:5352:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353'>pandas/core/indexes/base.py:5353:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354'>pandas/core/indexes/base.py:5354:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67'>pandas/tests/io/test_clipboard.py:67:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306'>pandas/tests/scalar/timedelta/test_constructors.py:306:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_invite.py#L1281'>zerver/tests/test_invite.py:1281:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_signup.py#L1021'>zerver/tests/test_signup.py:1021:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 6 | 0 | 0 | 0 |
| RUF003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58'>pandas/_libs/tslibs/timedeltas.pyi:58:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352'>pandas/core/indexes/base.py:5352:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353'>pandas/core/indexes/base.py:5353:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354'>pandas/core/indexes/base.py:5354:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67'>pandas/tests/io/test_clipboard.py:67:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306'>pandas/tests/scalar/timedelta/test_constructors.py:306:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_invite.py#L1281'>zerver/tests/test_invite.py:1281:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_signup.py#L1021'>zerver/tests/test_signup.py:1021:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 6 | 0 | 0 | 0 |
| RUF003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2023-11-03 13:57_

---

_Closed by @charliermarsh on 2023-11-03 13:57_

---

_Branch deleted on 2023-11-03 13:57_

---

_Comment by @zanieb on 2023-11-03 14:00_

@charliermarsh why did this cause ecosystem changes?

---

_Comment by @charliermarsh on 2023-11-03 14:01_

@zanieb - I think those are incorrect -- those are from https://github.com/astral-sh/ruff/pull/4430 which I tagged you on.

---
