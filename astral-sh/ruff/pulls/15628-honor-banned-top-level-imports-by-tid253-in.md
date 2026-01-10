```yaml
number: 15628
title: Honor banned top level imports by TID253 in PLC0415. 
type: pull_request
state: merged
author: mishamsk
labels:
  - rule
assignees: []
merged: true
base: main
head: 12803-fix-tid253-incompatible-with-plc0415
created_at: 2025-01-21T04:05:41Z
updated_at: 2025-01-24T12:38:20Z
url: https://github.com/astral-sh/ruff/pull/15628
synced_at: 2026-01-10T19:57:22Z
```

# Honor banned top level imports by TID253 in PLC0415. 

---

_Pull request opened by @mishamsk on 2025-01-21 04:05_

## Summary

Fixes #12803

I didn't dare to extract the shared module matching logic from tidy-imports into some helper module, so there is some questionable cross-use between lints.

## Test Plan

Added new snapshot test


---

_Comment by @github-actions[bot] on 2025-01-21 04:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -66 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -30 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/example_dags/example_branch_operator.py#L134'>airflow/example_dags/example_branch_operator.py:134:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/example_dags/example_branch_operator.py#L155'>airflow/example_dags/example_branch_operator.py:155:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/example_dags/example_branch_operator_decorator.py#L119'>airflow/example_dags/example_branch_operator_decorator.py:119:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/example_dags/example_branch_operator_decorator.py#L138'>airflow/example_dags/example_branch_operator_decorator.py:138:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/example_dags/tutorial_objectstorage.py#L73'>airflow/example_dags/tutorial_objectstorage.py:73:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/serialization/serializers/numpy.py#L50'>airflow/serialization/serializers/numpy.py:50:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/airflow/serialization/serializers/pandas.py#L39'>airflow/serialization/serializers/pandas.py:39:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L122'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:122:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L95'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:95:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/edge/src/airflow/providers/edge/example_dags/win_test.py#L276'>providers/edge/src/airflow/providers/edge/example_dags/win_test.py:276:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/amazon/aws/hooks/glue.py#L478'>providers/src/airflow/providers/amazon/aws/hooks/glue.py:478:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L158'>providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py:158:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L159'>providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py:159:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L211'>providers/src/airflow/providers/amazon/aws/transfers/sql_to_s3.py:211:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/apache/hive/hooks/hive.py#L1054'>providers/src/airflow/providers/apache/hive/hooks/hive.py:1054:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/common/sql/hooks/sql.py#L368'>providers/src/airflow/providers/common/sql/hooks/sql.py:368:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/common/sql/hooks/sql.py#L395'>providers/src/airflow/providers/common/sql/hooks/sql.py:395:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/oracle/hooks/oracle.py#L277'>providers/src/airflow/providers/oracle/hooks/oracle.py:277:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/presto/hooks/presto.py#L170'>providers/src/airflow/providers/presto/hooks/presto.py:170:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/salesforce/hooks/salesforce.py#L249'>providers/src/airflow/providers/salesforce/hooks/salesforce.py:249:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/salesforce/hooks/salesforce.py#L250'>providers/src/airflow/providers/salesforce/hooks/salesforce.py:250:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/96ca2a61c9d60c2445f8586a7d9fd5e3b6b5584e/providers/src/airflow/providers/salesforce/hooks/salesforce.py#L367'>providers/src/airflow/providers/salesforce/hooks/salesforce.py:367:9:</a> PLC0415 `import` should be at the top-level of a file
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -36 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L256'>src/bokeh/core/property/bases.py:256:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/pd.py#L59'>src/bokeh/core/property/pd.py:59:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/pd.py#L78'>src/bokeh/core/property/pd.py:78:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/visual.py#L154'>src/bokeh/core/property/visual.py:154:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/visual.py#L168'>src/bokeh/core/property/visual.py:168:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L459'>src/bokeh/core/serialization.py:459:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L518'>src/bokeh/document/events.py:518:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L281'>src/bokeh/io/export.py:281:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L368'>src/bokeh/io/export.py:368:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L369'>src/bokeh/io/export.py:369:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L71'>src/bokeh/io/webdriver.py:71:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L72'>src/bokeh/io/webdriver.py:72:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L73'>src/bokeh/io/webdriver.py:73:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L74'>src/bokeh/io/webdriver.py:74:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L90'>src/bokeh/io/webdriver.py:90:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L91'>src/bokeh/io/webdriver.py:91:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L92'>src/bokeh/io/webdriver.py:92:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L227'>src/bokeh/models/sources.py:227:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L257'>src/bokeh/models/sources.py:257:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L373'>src/bokeh/models/sources.py:373:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L508'>src/bokeh/models/sources.py:508:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L308'>src/bokeh/models/util/structure.py:308:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_plot.py#L76'>src/bokeh/plotting/_plot.py:76:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_plot.py#L77'>src/bokeh/plotting/_plot.py:77:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/airports.py#L68'>src/bokeh/sampledata/airports.py:68:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/anscombe.py#L82'>src/bokeh/sampledata/anscombe.py:82:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/antibiotics.py#L89'>src/bokeh/sampledata/antibiotics.py:89:5:</a> PLC0415 `import` should be at the top-level of a file
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 66 | 0 | 66 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-01-21 07:31_

The ecosystem changes look off. Would you mind taking a look? E.g. I don't think `random` is a banned import but for some reason, it no longer gets flagged in https://github.com/apache/airflow/blob/a76af4f22c0f34813ec51f00cd0e2a4909c77cbf/airflow/example_dags/example_branch_operator.py#L132

---

_Comment by @mishamsk on 2025-01-21 13:16_

> The ecosystem changes look off. Would you mind taking a look? E.g. I don't think `random` is a banned import but for some reason, it no longer gets flagged in https://github.com/apache/airflow/blob/a76af4f22c0f34813ec51f00cd0e2a4909c77cbf/airflow/example_dags/example_branch_operator.py#L132

yes, sure, I will. I wanted to run these checks locally yesterday, but it was getting late, so I haven’t had time to figure that out. First time making smth. with a lint. Thanks for the heads up

---

_Comment by @mishamsk on 2025-01-22 05:07_

@MichaReiser I believe this is now all fixed. Spot checked half of Airflow changes, all numpy/pandas that are indeed banned by TID253 [here](https://github.com/apache/airflow/blob/3edd78a24ee079081a373f6f5f152b2c5fce82cb/pyproject.toml#L385)

and checked one case in bokeh, PIL is also banned [here](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/.ruff.toml#L7)

Let me know if this looks good!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:580 on 2025-01-23 17:35_

Collecting the names here will be quiet bad for performance because it means we have to allocate for each import. I'd suggest to instead grab the nodes inside of `import_outside_top_level` (by either passing names or getting it from the `stmt`)

---

_@MichaReiser requested changes on 2025-01-23 17:39_

Thanks. This looks good from a functionality but we should avoid collecting the names when calling the checker to avoid unnecessary overhead. I suggest moving the logic into `import_outside_top_level` and extract the names from `stmt`. 

---

_@mishamsk reviewed on 2025-01-24 01:26_

---

_Review comment by @mishamsk on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:580 on 2025-01-24 01:26_

Sorry, my bad. Fixed.

---

_Review requested from @carljm by @MichaReiser on 2025-01-24 09:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-24 09:52_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-24 09:52_

---

_Review request for @carljm removed by @AlexWaygood on 2025-01-24 09:54_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-01-24 09:54_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-01-24 09:54_

---

_@MichaReiser approved on 2025-01-24 09:54_

---

_Merged by @MichaReiser on 2025-01-24 10:07_

---

_Closed by @MichaReiser on 2025-01-24 10:07_

---

_Label `rule` added by @MichaReiser on 2025-01-24 10:07_

---

_Branch deleted on 2025-01-24 12:38_

---
