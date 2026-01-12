```yaml
number: 14920
title: "Fix a typo in `if_key_in_dict_del.rs`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - internal
assignees: []
merged: true
base: main
head: RUF051
created_at: 2024-12-11T15:48:45Z
updated_at: 2024-12-11T16:30:35Z
url: https://github.com/astral-sh/ruff/pull/14920
synced_at: 2026-01-12T15:55:49Z
```

# Fix a typo in `if_key_in_dict_del.rs`

---

_@InSyncWithFoo_

(Accidentally introduced in #14553).

---

_Label `internal` added by @AlexWaygood on 2024-12-11 15:49_

---

_@MichaReiser approved on 2024-12-11 16:27_

Nice find

---

_Merged by @MichaReiser on 2024-12-11 16:28_

---

_Closed by @MichaReiser on 2024-12-11 16:28_

---

_Branch deleted on 2024-12-11 16:28_

---

_Comment by @github-actions[bot] on 2024-12-11 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+16 -16 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/docs/build_docs.py#L604'>docs/build_docs.py:604:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/docs/build_docs.py#L604'>docs/build_docs.py:604:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/docs/build_docs.py#L606'>docs/build_docs.py:606:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/docs/build_docs.py#L606'>docs/build_docs.py:606:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L666'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:666:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L666'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:666:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L680'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:680:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/42dfa7eee1d4816dfb95863c7e472c250eb5765b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L680'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:680:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/dashboards/schemas.py#L185'>superset/dashboards/schemas.py:185:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/dashboards/schemas.py#L185'>superset/dashboards/schemas.py:185:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/corporate/tests/test_stripe.py#L662'>corporate/tests/test_stripe.py:662:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/corporate/tests/test_stripe.py#L662'>corporate/tests/test_stripe.py:662:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/markdown/__init__.py#L2456'>zerver/lib/markdown/__init__.py:2456:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/markdown/__init__.py#L2456'>zerver/lib/markdown/__init__.py:2456:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/test_runner.py#L103'>zerver/lib/test_runner.py:103:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/test_runner.py#L103'>zerver/lib/test_runner.py:103:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/event_queue.py#L542'>zerver/tornado/event_queue.py:542:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/event_queue.py#L542'>zerver/tornado/event_queue.py:542:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/handlers.py#L42'>zerver/tornado/handlers.py:42:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/handlers.py#L42'>zerver/tornado/handlers.py:42:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/0983d58087088fc4a5da0ea11047befab95edc32/astropy/modeling/core.py#L189'>astropy/modeling/core.py:189:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/astropy/astropy/blob/0983d58087088fc4a5da0ea11047befab95edc32/astropy/modeling/core.py#L189'>astropy/modeling/core.py:189:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF051 | 32 | 16 | 16 | 0 | 0 |

</p>
</details>




---
