```yaml
number: 12480
title: "[ruff] Implement `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: getitem-with-parens-tuple
created_at: 2024-07-23T17:33:00Z
updated_at: 2024-08-08T14:57:49Z
url: https://github.com/astral-sh/ruff/pull/12480
synced_at: 2026-01-12T15:55:41Z
```

# [ruff] Implement `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)

---

_@dylwil3_

## Summary

Implements the new fixable lint rule RUF031 which checks for the use or omission of parentheses around tuples in subscripts, depending on the setting `lint.ruff.parenthesize-tuple-in-subscript`. By default, the use of parentheses is considered a violation.

For example, if `d` is a dictionary then

```python
d[(0,1)]
```

becomes

```python
d[0,1]
```

This closes #11990 and introduces configurations/options/settings for RUF rules.

## Test Plan

Snapshot verified, `cargo test` passed, docs generated and checked that rule appeared.


---

_Comment by @dylwil3 on 2024-07-23 17:34_

I'm not super happy with the names or descriptions for this rule and its structs/module/function/etc., so if folks have better ideas I'm all ears.

---

_Converted to draft by @dylwil3 on 2024-07-24 05:17_

---

_Marked ready for review by @dylwil3 on 2024-07-24 05:20_

---

_Label `rule` added by @MichaReiser on 2024-07-24 07:19_

---

_Comment by @MichaReiser on 2024-07-24 13:19_

Thanks for your contribution. 

@AlexWaygood what's your take on this rule? I'm somewhat concerned of adding the rule to Ruff's rule set because it is a restriction lint rule, it forbids the use of specific syntax, and not a lint rule that improves the quality of the code overall. 

---

_Label `needs-decision` added by @MichaReiser on 2024-07-24 13:19_

---

_Comment by @AlexWaygood on 2024-08-02 12:23_

> I'm somewhat concerned of adding the rule to Ruff's rule set because it is a restriction lint rule, it forbids the use of specific syntax, and not a lint rule that improves the quality of the code overall.

Hmm. I'm not sure I'd call this a "restriction" rule since it doesn't forbid the user from using any Python idioms that they'd otherwise be able to use. The recommendations the rule makes are purely cosmetic; the code's AST doesn't change at all if you follow the rule's recommendations. I'd call it a formatting rule, similar to many of our pycodestyle rules.

This rule is quite opinionated, but I can definitely see that it's useful to enforce either one style or the other. The question is what style is preferable? The code becomes less noisy without the parens, but I'm not sure it's more _readable_ -- it requires additional knowledge of Python's semantics to know that there's an implicit tuple formed by the comma there. So I think I'd support the rule being added, but I'd make it configurable (similar to e.g. our `flake8-pytest-style` rules) -- so you can either enforce that there are _never_ parentheses, or that there are _always_ parentheses.

---

_Comment by @dylwil3 on 2024-08-02 12:31_

Great! A good opportunity to learn about rule-specific configurations. I'll make the changes.

---

_Converted to draft by @dylwil3 on 2024-08-02 22:00_

---

_Comment by @github-actions[bot] on 2024-08-03 00:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+126 -259 violations, +0 -0 fixes in 10 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/b70b608abdb17d36da37e4c3b4fe7c6b1b2e59f9/disnake/ext/commands/slash_core.py#L39'>disnake/ext/commands/slash_core.py:39:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/b70b608abdb17d36da37e4c3b4fe7c6b1b2e59f9/disnake/ui/modal.py#L236'>disnake/ui/modal.py:236:22:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/DisnakeDev/disnake/blob/b70b608abdb17d36da37e4c3b4fe7c6b1b2e59f9/disnake/ui/view.py#L425'>disnake/ui/view.py:425:35:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/DisnakeDev/disnake/blob/b70b608abdb17d36da37e4c3b4fe7c6b1b2e59f9/disnake/ui/view.py#L530'>disnake/ui/view.py:530:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L345'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py:345:25:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/utils/test_train_utils.py#L117'>tests/utils/test_train_utils.py:117:67:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+88 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/configuration.py#L730'>airflow/configuration.py:730:42:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/configuration.py#L758'>airflow/configuration.py:758:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/configuration.py#L793'>airflow/configuration.py:793:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/configuration.py#L980'>airflow/configuration.py:980:69:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/jobs/scheduler_job_runner.py#L120'>airflow/jobs/scheduler_job_runner.py:120:43:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/jobs/scheduler_job_runner.py#L539'>airflow/jobs/scheduler_job_runner.py:539:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/jobs/scheduler_job_runner.py#L559'>airflow/jobs/scheduler_job_runner.py:559:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/jobs/scheduler_job_runner.py#L599'>airflow/jobs/scheduler_job_runner.py:599:54:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/jobs/scheduler_job_runner.py#L601'>airflow/jobs/scheduler_job_runner.py:601:21:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py#L276'>airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py:276:25:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/454b5bbf529ea2a9b0b69a871803ff8920af0bb5/airflow/migrations/versions/0060_2_0_0_remove_id_column_from_xcom.py#L67'>airflow/migrations/versions/0060_2_0_0_remove_id_column_from_xcom.py:67:25:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
... 77 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -108 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/scripts/permissions_cleanup.py#L32'>scripts/permissions_cleanup.py:32:19:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L101'>superset/extensions/metadb.py:101:35:</a> ARG002 Unused method argument: `url`
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L102'>superset/extensions/metadb.py:102:9:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L102'>superset/extensions/metadb.py:102:9:</a> D212 [*] Multi-line docstring summary should start at the first line
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L102'>superset/extensions/metadb.py:102:9:</a> D401 First line of docstring should be in imperative mood: "A custom Shillelagh SQLAlchemy dialect with a single adapter configured."
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L105'>superset/extensions/metadb.py:105:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L114'>superset/extensions/metadb.py:114:22:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L126'>superset/extensions/metadb.py:126:5:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L126'>superset/extensions/metadb.py:126:5:</a> D212 [*] Multi-line docstring summary should start at the first line
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L126'>superset/extensions/metadb.py:126:5:</a> D401 First line of docstring should be in imperative mood: "Decorator that prevents DML against databases where it's not allowed."
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L131'>superset/extensions/metadb.py:131:57:</a> ANN401 Dynamically typed expressions (typing.Any) are disallowed in `*args`
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L131'>superset/extensions/metadb.py:131:72:</a> ANN401 Dynamically typed expressions (typing.Any) are disallowed in `**kwargs`
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L131'>superset/extensions/metadb.py:131:80:</a> ANN401 Dynamically typed expressions (typing.Any) are disallowed in `wrapper`
- <a href='https://github.com/apache/superset/blob/c2202454147936d1770137eaac334719b8e746e2/superset/extensions/metadb.py#L134'>superset/extensions/metadb.py:134:19:</a> TRY003 Avoid specifying long messages outside the exception class
... 99 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/graph/node_and_edge_attributes.py#L21'>examples/topics/graph/node_and_edge_attributes.py:21:16:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/unemployment.py#L82'>src/bokeh/sampledata/unemployment.py:82:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/us_counties.py#L114'>src/bokeh/sampledata/us_counties.py:114:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_util__colors.py#L141'>tests/unit/bokeh/colors/test_util__colors.py:141:24:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L197'>stdlib/tkinter/__init__.pyi:197:16:</a> PYI052 Need type annotation for `Activate`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L198'>stdlib/tkinter/__init__.pyi:198:19:</a> PYI052 Need type annotation for `ButtonPress`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L200'>stdlib/tkinter/__init__.pyi:200:21:</a> PYI052 Need type annotation for `ButtonRelease`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L201'>stdlib/tkinter/__init__.pyi:201:17:</a> PYI052 Need type annotation for `Circulate`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L202'>stdlib/tkinter/__init__.pyi:202:24:</a> PYI052 Need type annotation for `CirculateRequest`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L203'>stdlib/tkinter/__init__.pyi:203:21:</a> PYI052 Need type annotation for `ClientMessage`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L204'>stdlib/tkinter/__init__.pyi:204:16:</a> PYI052 Need type annotation for `Colormap`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L205'>stdlib/tkinter/__init__.pyi:205:17:</a> PYI052 Need type annotation for `Configure`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L206'>stdlib/tkinter/__init__.pyi:206:24:</a> PYI052 Need type annotation for `ConfigureRequest`
- <a href='https://github.com/python/typeshed/blob/92cc5683251eab703468f6f68c813525f38e3251/stdlib/tkinter/__init__.pyi#L207'>stdlib/tkinter/__init__.pyi:207:14:</a> PYI052 Need type annotation for `Create`
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/chain/ethereum/modules/aave/v2/accountant.py#L34'>rotkehlchen/chain/ethereum/modules/aave/v2/accountant.py:34:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/chain/ethereum/modules/aave/v2/accountant.py#L71'>rotkehlchen/chain/ethereum/modules/aave/v2/accountant.py:71:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L199'>rotkehlchen/chain/evm/decoding/cowswap/decoder.py:199:41:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/tasks/assets.py#L441'>rotkehlchen/tasks/assets.py:441:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/tasks/manager.py#L331'>rotkehlchen/tasks/manager.py:331:60:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/tasks/manager.py#L341'>rotkehlchen/tasks/manager.py:341:39:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/tests/api/blockchain/test_evm.py#L432'>rotkehlchen/tests/api/blockchain/test_evm.py:432:43:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/rotki/rotki/blob/b20992a61d6b94ce69c045df61e5dd007fa6b780/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L642'>rotkehlchen/tests/unit/globaldb/test_upgrades.py:642:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/analytics/lib/counts.py#L540'>analytics/lib/counts.py:540:28:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/actions/message_edit.py#L961'>zerver/actions/message_edit.py:961:17:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/email_notifications.py#L636'>zerver/lib/email_notifications.py:636:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/email_notifications.py#L638'>zerver/lib/email_notifications.py:638:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/mention.py#L133'>zerver/lib/mention.py:133:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/rate_limiter.py#L222'>zerver/lib/rate_limiter.py:222:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/rate_limiter.py#L243'>zerver/lib/rate_limiter.py:243:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/rate_limiter.py#L264'>zerver/lib/rate_limiter.py:264:13:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/rate_limiter.py#L266'>zerver/lib/rate_limiter.py:266:42:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d445137d363981374e3b8b69a6374bc0eadf1901/zerver/lib/user_topics.py#L290'>zerver/lib/user_topics.py:290:36:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9799063a643b82b34dbd14b5420ef5abb922110a/indico/modules/rb/models/reservations.py#L82'>indico/modules/rb/models/reservations.py:82:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/indico/indico/blob/9799063a643b82b34dbd14b5420ef5abb922110a/indico/modules/users/util.py#L312'>indico/modules/users/util.py:312:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/indico/indico/blob/9799063a643b82b34dbd14b5420ef5abb922110a/indico/modules/users/util.py#L326'>indico/modules/users/util.py:326:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/indico/indico/blob/9799063a643b82b34dbd14b5420ef5abb922110a/indico/modules/users/util.py#L76'>indico/modules/users/util.py:76:13:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -112 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1005'>testing/test_config.py:1005:9:</a> PLR6301 Method `test_inifilename` could be a function, class method, or static method
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1089'>testing/test_config.py:1089:13:</a> PLR6301 Method `load` could be a function, class method, or static method
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1124'>testing/test_config.py:1124:13:</a> PLR6301 Method `load` could be a function, class method, or static method
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1151'>testing/test_config.py:1151:13:</a> PLR6301 Method `load` could be a function, class method, or static method
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1179'>testing/test_config.py:1179:13:</a> PLR6301 Method `load` could be a function, class method, or static method
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L120'>testing/test_config.py:120:9:</a> PLR6301 Method `test_ini_names` could be a function, class method, or static method
... 86 additional changes omitted for rule PLR6301
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L15'>testing/test_config.py:15:28:</a> PLC2701 Private name import `_pytest`
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L16'>testing/test_config.py:16:28:</a> PLC2701 Private name import `_pytest`
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L17'>testing/test_config.py:17:28:</a> PLC2701 Private name import `_pytest`
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L18'>testing/test_config.py:18:28:</a> PLC2701 Private name import `_pytest`
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1978'>testing/test_config.py:1978:43:</a> PLC2701 Private name import `_pytest`
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L1978'>testing/test_config.py:1978:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L19'>testing/test_config.py:19:28:</a> PLC2701 Private name import `_pytest`
... 10 additional changes omitted for rule PLC2701
- <a href='https://github.com/pytest-dev/pytest/blob/0affd29062b507d9b16495675fe4c0d1b1d213a5/testing/test_config.py#L34'>testing/test_config.py:34:1:</a> PLR0904 Too many public methods (23 > 20)
... 98 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (37 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF031 | 126 | 126 | 0 | 0 | 0 |
| PLR6301 | 92 | 0 | 92 | 0 | 0 |
| PYI052 | 38 | 0 | 38 | 0 | 0 |
| D212 | 19 | 0 | 19 | 0 | 0 |
| PLC2701 | 15 | 0 | 15 | 0 | 0 |
| D200 | 13 | 0 | 13 | 0 | 0 |
| ANN401 | 12 | 0 | 12 | 0 | 0 |
| DOC201 | 9 | 0 | 9 | 0 | 0 |
| TRY003 | 9 | 0 | 9 | 0 | 0 |
| EM102 | 6 | 0 | 6 | 0 | 0 |
| DOC501 | 4 | 0 | 4 | 0 | 0 |
| D401 | 3 | 0 | 3 | 0 | 0 |
| EM101 | 3 | 0 | 3 | 0 | 0 |
| PLC0415 | 3 | 0 | 3 | 0 | 0 |
| TCH002 | 3 | 0 | 3 | 0 | 0 |
| ARG002 | 2 | 0 | 2 | 0 | 0 |
| COM812 | 2 | 0 | 2 | 0 | 0 |
| E501 | 2 | 0 | 2 | 0 | 0 |
| ARG004 | 2 | 0 | 2 | 0 | 0 |
| PLR2004 | 2 | 0 | 2 | 0 | 0 |
| D107 | 2 | 0 | 2 | 0 | 0 |
| PLR0904 | 2 | 0 | 2 | 0 | 0 |
| PLC1901 | 2 | 0 | 2 | 0 | 0 |
| RUF022 | 1 | 0 | 1 | 0 | 0 |
| D102 | 1 | 0 | 1 | 0 | 0 |
| CPY001 | 1 | 0 | 1 | 0 | 0 |
| RUF012 | 1 | 0 | 1 | 0 | 0 |
| FBT001 | 1 | 0 | 1 | 0 | 0 |
| FBT002 | 1 | 0 | 1 | 0 | 0 |
| SIM102 | 1 | 0 | 1 | 0 | 0 |
| ANN204 | 1 | 0 | 1 | 0 | 0 |
| E721 | 1 | 0 | 1 | 0 | 0 |
| B905 | 1 | 0 | 1 | 0 | 0 |
| DOC402 | 1 | 0 | 1 | 0 | 0 |
| ERA001 | 1 | 0 | 1 | 0 | 0 |
| TCH003 | 1 | 0 | 1 | 0 | 0 |
| UP035 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @dylwil3 on 2024-08-03 04:19_

---

_Comment by @dylwil3 on 2024-08-03 04:28_

Any chance a reviewer could check whether the new `lint.ruff` option appears in "Settings" in the docs? For some reason I didn't see it and couldn't figure out why, but I may be messing up the local docs generation.

They appear in the json schema but not in the output of `cargo dev generate-options`.

If it matters, this is the first example of a "plugin" configuration being added to `LintOptions` rather than `LintCommonOptions` as [this dosctring warning](https://github.com/astral-sh/ruff/blob/38e178e91488fd4f3b3f41cc0532a0a31126965c/crates/ruff_workspace/src/options.rs#L531) suggested - so hopefully I interpreted that correctly.

---

_@MichaReiser reviewed on 2024-08-03 07:34_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:460 on 2024-08-03 07:34_

```suggestion
    /// Options for the `ruff` plugin
    #[option_group]
    pub ruff: Option<RuffOptions>,
```

Our options macros are a bit complicated. Especially because you have to add both serde and non serde attributes. Anyway, adding `#[option_group]` should make the group to appear in the documentation

---

_@dylwil3 reviewed on 2024-08-03 12:03_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/options.rs`:460 on 2024-08-03 12:03_

That did the trick, thank you!

---

_Comment by @dylwil3 on 2024-08-03 23:57_

Ok, should be good to go!

---

_Renamed from "[ruff] Implement `parentheses-in-tuple-slices` (`RUF031`)" to "[ruff] Implement `bad-format-tuple-in-getitem` (`RUF031`)" by @dylwil3 on 2024-08-03 23:59_

---

_Comment by @MichaReiser on 2024-08-05 07:57_

> Hmm. I'm not sure I'd call this a "restriction" rule since it doesn't forbid the user from using any Python idioms that they'd otherwise be able to use. 

It doesn't restrict the use of any python idoms but it does restrict the allowed syntax. Any use of subscript > tuple is restricted.

> I'd call it a formatting rule, similar to many of our pycodestyle rules.

Fair enough

> (similar to e.g. our flake8-pytest-style rules) -- so you can either enforce that there are never parentheses, or that there are always parentheses.

I'm sorry to raise this so late. I'm concerned about this option because it limits the formatter's style guide options in the long term. Our formatter already removes unnecessary parentheses in some places, and I could see it removing more parentheses in the long term. That's why I would prefer **not** support the option and merge the rule without.

---

_Comment by @AlexWaygood on 2024-08-05 08:10_

> It doesn't restrict the use of any python idoms but it does restrict the allowed syntax. Any use of subscript > tuple is restricted.

I'm concerned that the implied definition of a "restriction" rule here is quite broad. Almost every rule that we have "restricts" you from doing _something_ that the rule deems bad in terms of correctness, style or formatting :-) But this is getting off-topic.

> I'm concerned about this option because it limits the formatter's style guide options in the long term. Our formatter already removes unnecessary parentheses in some places, and I could see it removing more parentheses in the long term. That's why I would prefer not support the option and merge the rule without.

Hmm, I see. To me it feels very unclear which style is preferable, which is why I think this makes sense as a configurable linter rule rather than something the formatter should have an opinion on. But if we think this is something the formatter might want to take an opinion on in the future, then I suppose I'm not sure that this is a linter rule we should have at all, then -- perhaps we should simply teach the formatter to have an opinion on this.

---

_Comment by @MichaReiser on 2024-08-05 08:19_

> perhaps we should simply teach the formatter to have an opinion on this.

It doesn't seem that we feel comfortable making that decision today. You explicitly state that you are unclear about what style you prefer. 

I wonder if the goal of the rule is to assert consistent formatting or if it is really about specifically restricting parenthesized tuples in subscriptions. What I hear is mainly that some people don't want to have parenthesized tuples in subscripts and less so that they want to enforce parentheses everywhere. Specifically restricting the use of parenthesized tuples also helps to give the rule a better name: `allow(parenthesized-tuple-in-subscript)` . 



---

_Comment by @AlexWaygood on 2024-08-05 08:25_

>> perhaps we should simply teach the formatter to have an opinion on this.

> It doesn't seem that we feel comfortable making that decision today. You explicitly state that you are unclear about what style you prefer.

Yep. That's why I _don't_ think we should teach the formatter to have an opinion on this (ever), and why I would prefer a configurable linter rule ;-)

Anyway, I've said my piece on this. For me, I can see it's useful to enforce a consistent style across a codebase, but I think the rule is too opinionated -- as either something to be incorporated in the formatter or a linter rule -- unless it can be made configurable. I think we're going round in circles now, so I'll bow out at this point :-)

---

_@MichaReiser reviewed on 2024-08-05 10:07_

@AlexWaygood and I discussed this offline and I now agree that this isn't something the formatter should be opinionated about. Therefore, the lint rule as is is fine :)

I would prefer to change the name: `bad` isn't very telling. How about `incorrectly-parenthesized-tuple-in-subscript` or `incorrectly-parenthesized-tuple-in-getitem`?

For the setting, I think I would go with `parenthesize-tuple-in-getitem` to match the rule name. 



---

_Label `needs-decision` removed by @MichaReiser on 2024-08-05 10:07_

---

_Label `accepted` added by @MichaReiser on 2024-08-05 10:07_

---

_Comment by @dylwil3 on 2024-08-05 15:33_

Great! I've made the requested changes. Let me know if there's anything else, and thanks for the discussion!

---

_Renamed from "[ruff] Implement `bad-format-tuple-in-getitem` (`RUF031`)" to "[ruff] Implement `incorrectly-parenthesized-tuple-in-getitem` (`RUF031`)" by @dylwil3 on 2024-08-05 15:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:4 on 2024-08-07 06:46_

I would prefer to use an `if...else` over a match instead of suppressing the clippy warning. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:20 on 2024-08-07 06:46_

```suggestion
/// convention may be preferred.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:82 on 2024-08-07 06:48_

Nit: You can reduce the nesting by using `let .. else`
```suggestion
    let Some(tuple_index) = subscript.slice.as_tuple_expr() else {
    	return;
  	]
    if (tuple_index.parenthesized != prefer_parentheses) && tuple_index.elts.len() > 1 {
        let locator = checker.locator();
        let source_range = subscript.slice.range();
        let new_source = match prefer_parentheses {
            true => {
                format!("({})", locator.slice(source_range))
            }
            false => {
                locator.slice(source_range)[1..source_range.len().to_usize() - 1].to_string()
            }
        };
        let edit = Edit::range_replacement(new_source, source_range);
        checker.diagnostics.push(
            Diagnostic::new(
                IncorrectlyParenthesizedTupleInGetitem { prefer_parentheses },
                source_range,
            )
            .with_fix(Fix::safe_edit(edit)),
        );
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:62 on 2024-08-07 06:50_

I would find a comment explaining why we check for `elts.len() > 1` helpful. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:16 on 2024-08-07 06:51_

@AlexWaygood should we use subscript instead of `__getitem__`? I'm concerned that users will expect that this rule detects `a.__getitem__((a, b))` which it does not

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF031.py`:6 on 2024-08-07 06:53_

Could we add one more example of the form

```python
token_features[
    (window_position, feature_name)
] = self._extract_raw_features_from_token
```

It's quite common in the ecosystem check. 


---

_@MichaReiser approved on 2024-08-07 06:53_

LGTM. Thanks for working on this. I left a few smaller nits and we may want to change the wording of the rule (@AlexWaygood )

---

_@AlexWaygood reviewed on 2024-08-07 10:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:16 on 2024-08-07 10:16_

Yes, I'd prefer to call the rule `parenthesized-tuple-in-subscript`, and refer to the rule as detecting "parenthesized tuples in subscripts" rather than "parenthesized tuples in `__getitem__` calls"

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:4 on 2024-08-07 10:20_

Agreed. Sometimes in this kind of situation it can be more readable to introduce a custom enum and match on that instead, e.g.

```rs
enum ParenthesesPreference {
    Keep,
    Remove,
}
```

but here I'm not sure I really see the need; I think I'd just stick with `bool`

---

_@AlexWaygood reviewed on 2024-08-07 10:20_

---

_@AlexWaygood reviewed on 2024-08-07 10:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:45 on 2024-08-07 10:22_

```suggestion
            true => format!("Use parentheses when evaluating `__getitem__` at a tuple."),
```

(And, like @MichaReiser, I think I'd refer to this rule as "avoiding parentheses for tuples in subscripts" rather than "avoiding parentheses for tuples in `__getitem__` calls")

---

_Review requested from @carljm by @dylwil3 on 2024-08-07 11:26_

---

_Comment by @AlexWaygood on 2024-08-07 11:35_

Looks like you might have done a bad rebase there @dylwil3 -- the diff for this PR is now `+7,002 −438` lines, which feels a bit overkill ;-)

---

_Renamed from "[ruff] Implement `incorrectly-parenthesized-tuple-in-getitem` (`RUF031`)" to "[ruff] Implement `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)" by @dylwil3 on 2024-08-07 11:44_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:62 on 2024-08-07 11:48_

I'm not sure why I did that actually - so I removed that restriction to single-element tuples (and updated the docstring).

---

_@dylwil3 reviewed on 2024-08-07 11:48_

---

_Review request for @carljm removed by @AlexWaygood on 2024-08-07 11:49_

---

_Comment by @dylwil3 on 2024-08-07 12:06_

Oh dear... out of sync commits + a rebase accidentally triggered a new reviewer request and absurd diff. Sorry! If someone more expert in git knows how I can fix that, let me know...

Other than that: all nits have been picked - thank you for your help!

---

_Comment by @AlexWaygood on 2024-08-07 12:08_

I'll try and fix the diff for you. It may involve force-pushing to your branch, FYI.

---

_Comment by @AlexWaygood on 2024-08-07 12:18_

@dylwil3, I fixed up your PR history for you. Before pushing any further updates to this PR, you'll now need to run the following commands locally in order to fetch your PR branch as it exists on GitHub (assuming your GitHub remote is called `origin`):

```
git fetch origin
git reset origin/getitem-with-parens-tuple --hard
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:1 on 2024-08-07 12:19_

Could you also rename this file to reflect the change of the rule name?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:4 on 2024-08-07 12:20_

It looks like this has been marked as resolved, but the comment hasn't been addressed

---

_@AlexWaygood reviewed on 2024-08-07 12:20_

Nearly there!

---

_@dylwil3 reviewed on 2024-08-07 12:25_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:1 on 2024-08-07 12:25_

Done! And it looks like the match vs. if/else changes were undone in the gitfusion. I'll redo them. (I think that's actually what started the cascade of changes when rebasing - I commited those from github, then pulled with rebase because I was out of sync... one moment while I fix that...)

---

_@AlexWaygood reviewed on 2024-08-07 12:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_getitem.rs`:1 on 2024-08-07 12:31_

Ahh, I've always wondered how this particular gitfusion happens! (We see it reasonably often at CPython.) I always pull with merge if my remote branch contains work that my local branch doesn't :-)

---

_Comment by @dylwil3 on 2024-08-07 12:39_

Thanks for your help cleaning up my mess  😄 

Sorry for the last minute trouble and let me know if you find any other issues!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/RUF031_prefer_parens.py`:27 on 2024-08-07 12:51_

nit: can you add a newline to the end of this file?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:11 on 2024-08-07 12:53_

```suggestion
/// Checks for consistent style regarding whether tuples in subscripts
/// are parenthesized.
///
/// The exact nature of this violation depends on the setting
/// [`lint.ruff.parenthesize-tuple-in-subscript`]. By default, the use of
/// parentheses is considered a violation.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:42 on 2024-08-07 12:54_

```suggestion
            format!("Avoid parentheses for tuples in subscripts.")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:51 on 2024-08-07 12:55_

I think these could be slightly more concise, so that we're not just repeating the violation message:

```suggestion
        if self.prefer_parentheses {
            "Parenthesize the tuple.".to_string()
        } else {
            "Remove the parentheses.".to_string()
        }
```

---

_@AlexWaygood approved on 2024-08-07 12:58_

LGTM! I'll apply my final suggestions and then merge

---

_Merged by @AlexWaygood on 2024-08-07 13:11_

---

_Closed by @AlexWaygood on 2024-08-07 13:11_

---

_Branch deleted on 2024-08-07 13:30_

---

_Comment by @AlexWaygood on 2024-08-07 13:34_

Thanks for the PR @dylwil3 and for your patience -- this was a great contribution!

---

_Comment by @neutrinoceros on 2024-08-07 16:27_

Many thanks to y'all, I'm thrilled this is now a thing ! 

---

_Label `preview` added by @dhruvmanila on 2024-08-08 14:57_

---
