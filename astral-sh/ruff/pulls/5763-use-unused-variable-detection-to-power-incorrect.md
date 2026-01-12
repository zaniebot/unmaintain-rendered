```yaml
number: 5763
title: "Use unused variable detection to power `incorrect-dict-iterator`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unused-loop
created_at: 2023-07-14T18:09:43Z
updated_at: 2023-07-14T19:42:52Z
url: https://github.com/astral-sh/ruff/pull/5763
synced_at: 2026-01-12T15:55:19Z
```

# Use unused variable detection to power `incorrect-dict-iterator`

---

_@charliermarsh_

## Summary

`PERF102` looks for unused keys or values in `dict.items()` calls, and suggests instead using `dict.keys()` or `dict.values()`. Previously, this check determined usage by looking for underscore-prefixed variables. However, we can use the semantic model to actually detect whether a variable is used. This has two nice effects:

1. We avoid odd false-positives whereby underscore-prefixed variables are actually used.
2. We can catch more cases (fewer false-negatives) by detecting unused loop variables that _aren't_ underscore-prefixed.

Closes #5692.


---

_Review requested from @zanieb by @charliermarsh on 2023-07-14 18:09_

---

_@charliermarsh reviewed on 2023-07-14 18:10_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/perflint/PERF102.py`:6 on 2023-07-14 18:10_

Now that this rule relies on scoping, it's easier to put every case in its own scope. Otherwise, each case kind of pollutes every other case (if they're all in the module scope).

---

_@charliermarsh reviewed on 2023-07-14 18:10_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/perflint/PERF102.py`:94 on 2023-07-14 18:10_

This and the next case are new.

---

_@charliermarsh reviewed on 2023-07-14 18:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:170 on 2023-07-14 18:11_

This does have some weird false negatives, like:

```python
def f():
    # Unfixable (false negative) due to usage of `bar` outside of loop.
    for foo, bar in dict.items():
        break

    bar = 1
    print(bar)
```

We'd consider `bar` used, because it's shadowed by a future `bar`, and that shadowed variable is used. We can fix this with better control flow logic, but it's fine for now, `B007` already suffers from this.

---

_Comment by @github-actions[bot] on 2023-07-14 18:19_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+17, -0, 0 error(s))

<details><summary>airflow (+5, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/42cb9693e473faabaf9fcdd754df35dc9e5e3523/airflow/executors/kubernetes_executor_utils.py#L451'>airflow/executors/kubernetes_executor_utils.py:451:40:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/apache/airflow/blob/42cb9693e473faabaf9fcdd754df35dc9e5e3523/airflow/models/dag.py#L3282'>airflow/models/dag.py:3282:21:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/apache/airflow/blob/42cb9693e473faabaf9fcdd754df35dc9e5e3523/docs/conf.py#L428'>docs/conf.py:428:36:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/apache/airflow/blob/42cb9693e473faabaf9fcdd754df35dc9e5e3523/docs/conf.py#L438'>docs/conf.py:438:33:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/apache/airflow/blob/42cb9693e473faabaf9fcdd754df35dc9e5e3523/tests/utils/test_db_cleanup.py#L287'>tests/utils/test_db_cleanup.py:287:39:</a> PERF102 [*] When using only the values of a dict use the `values()` method
</pre>

</p>
</details>
<details><summary>bokeh (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:21:</a> PERF102 [*] When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary>zulip (+11, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/tools/setup/emoji/emoji_setup_utils.py#L56'>tools/setup/emoji/emoji_setup_utils.py:56:34:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/data_import/slack.py#L368'>zerver/data_import/slack.py:368:25:</a> PERF102 [*] When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/lib/cache.py#L373'>zerver/lib/cache.py:373:21:</a> PERF102 [*] When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/lib/rate_limiter.py#L285'>zerver/lib/rate_limiter.py:285:43:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/lib/streams.py#L74'>zerver/lib/streams.py:74:40:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/lib/test_classes.py#L1503'>zerver/lib/test_classes.py:1503:26:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/management/commands/compilemessages.py#L148'>zerver/management/commands/compilemessages.py:148:31:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/models.py#L1079'>zerver/models.py:1079:43:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/models.py#L1218'>zerver/models.py:1218:33:</a> PERF102 [*] When using only the values of a dict use the `values()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/tests/test_events.py#L1947'>zerver/tests/test_events.py:1947:40:</a> PERF102 [*] When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/tests/test_slack_importer.py#L1348'>zerver/tests/test_slack_importer.py:1348:30:</a> PERF102 [*] When using only the values of a dict use the `values()` method
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF102 | 17 | 17 | 0 |

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     10.0±0.49ms     4.1 MB/sec     1.00      9.8±0.27ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1980.3±102.13µs     8.4 MB/sec    1.02      2.0±0.09ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   215.4±11.30µs    13.7 MB/sec     1.04   224.3±20.52µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.12ms     6.1 MB/sec     1.01      4.2±0.20ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.18     17.1±0.37ms     2.4 MB/sec     1.00     14.5±0.48ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.11      4.1±0.07ms     4.1 MB/sec     1.00      3.7±0.12ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.11    538.7±7.80µs     5.5 MB/sec     1.00   485.1±22.93µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.13      7.5±0.11ms     3.4 MB/sec     1.00      6.6±0.29ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.24      8.8±0.13ms     4.6 MB/sec     1.00      7.1±0.26ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.21   1926.0±9.28µs     8.6 MB/sec     1.00  1590.5±54.38µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.12    209.7±2.53µs    14.1 MB/sec     1.00    187.4±5.54µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.22      3.9±0.04ms     6.5 MB/sec     1.00      3.2±0.08ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.2±0.14ms     3.6 MB/sec    1.00     11.0±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    247.5±4.91µs    11.9 MB/sec    1.02    251.6±9.85µs    11.7 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.4 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     16.5±0.39ms     2.5 MB/sec    1.00     16.2±0.45ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   504.7±15.77µs     5.8 MB/sec    1.00   497.9±10.08µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.13ms     3.5 MB/sec    1.00      7.2±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.02      8.3±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1731.6±24.37µs     9.6 MB/sec    1.00  1704.2±20.45µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.04    203.7±2.84µs    14.5 MB/sec    1.00    196.2±4.14µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-14 18:22_

The ecosystem checks generally look right to me, although in Airflow I think there's one false positive due to a non-dictionary that implements `.items()`.

---

_@zanieb reviewed on 2023-07-14 19:01_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:166 on 2023-07-14 19:01_

Was this `filter` just missing previously or... ?

---

_@zanieb reviewed on 2023-07-14 19:02_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:166 on 2023-07-14 19:02_

(I spent an embarrassing amount of time trying to figure out how this did the change described in the PR title before realizing it was a different rule)

---

_@zanieb approved on 2023-07-14 19:05_

Cool!

Edit: Also read the ecosystem checks and agree they look fine

---

_@charliermarsh reviewed on 2023-07-14 19:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:166 on 2023-07-14 19:42_

Yeah, sorry, the fix to `incorrect-dict-iterator` is based on the way this existing rule operates, and I noticed that they would both benefit from this condition. (We don't care about bindings that occur _before_ the loop.)

---

_Merged by @charliermarsh on 2023-07-14 19:42_

---

_Closed by @charliermarsh on 2023-07-14 19:42_

---

_Branch deleted on 2023-07-14 19:42_

---

_Label `bug` added by @charliermarsh on 2023-07-14 19:42_

---
