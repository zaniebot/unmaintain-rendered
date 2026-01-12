```yaml
number: 8473
title: Make Unicode-to-Unicode confusables a preview change
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/preview
created_at: 2023-11-03T15:53:21Z
updated_at: 2023-11-03T16:17:30Z
url: https://github.com/astral-sh/ruff/pull/8473
synced_at: 2026-01-12T15:55:26Z
```

# Make Unicode-to-Unicode confusables a preview change

---

_@charliermarsh_

_No description provided._

---

_Review requested from @zanieb by @charliermarsh on 2023-11-03 15:53_

---

_Label `preview` added by @charliermarsh on 2023-11-03 15:53_

---

_Comment by @github-actions[bot] on 2023-11-03 16:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -9 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58'>pandas/_libs/tslibs/timedeltas.pyi:58:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352'>pandas/core/indexes/base.py:5352:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353'>pandas/core/indexes/base.py:5353:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354'>pandas/core/indexes/base.py:5354:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67'>pandas/tests/io/test_clipboard.py:67:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306'>pandas/tests/scalar/timedelta/test_constructors.py:306:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/666ac2c445d9b6e173fbdf2bb62b7be754ca43b5/zerver/tests/test_invite.py#L1281'>zerver/tests/test_invite.py:1281:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
- <a href='https://github.com/zulip/zulip/blob/666ac2c445d9b6e173fbdf2bb62b7be754ca43b5/zerver/tests/test_signup.py#L1021'>zerver/tests/test_signup.py:1021:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 0 | 6 | 0 | 0 |
| RUF003 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-11-03 16:17_

---

_Closed by @charliermarsh on 2023-11-03 16:17_

---

_Branch deleted on 2023-11-03 16:17_

---
