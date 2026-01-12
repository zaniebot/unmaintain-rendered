```yaml
number: 9964
title: "`PLR2004`: Accept 0.0 and 1.0 as common magic values"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: magic-values
created_at: 2024-02-12T22:50:00Z
updated_at: 2024-03-07T02:22:55Z
url: https://github.com/astral-sh/ruff/pull/9964
synced_at: 2026-01-12T15:55:30Z
```

# `PLR2004`: Accept 0.0 and 1.0 as common magic values

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Accept 0.0 and 1.0 as common magic values. This is in line with the pylint behaviour, and I think makes sense conceptually.


## Test Plan

Test cases were added to `crates/ruff_linter/resources/test/fixtures/pylint/magic_value_comparison.py`

---

_Comment by @github-actions[bot] on 2024-02-12 23:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -35 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/core/test_configuration.py#L229'>tests/core/test_configuration.py:229:16:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/jobs/test_scheduler_job.py#L4941'>tests/jobs/test_scheduler_job.py:4941:24:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/jobs/test_scheduler_job.py#L4966'>tests/jobs/test_scheduler_job.py:4966:24:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py#L87'>tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py:87:41:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/utils/test_sqlalchemy.py#L89'>tests/utils/test_sqlalchemy.py:89:62:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/utils/test_sqlalchemy.py#L90'>tests/utils/test_sqlalchemy.py:90:58:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -29 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L321'>src/bokeh/colors/color.py:321:22:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L337'>src/bokeh/colors/color.py:337:21:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L460'>src/bokeh/colors/color.py:460:22:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L280'>src/bokeh/core/property/numeric.py:280:12:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L280'>src/bokeh/core/property/numeric.py:280:28:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1621'>src/bokeh/palettes.py:1621:17:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L414'>src/bokeh/plotting/contour.py:414:53:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L107'>tests/unit/bokeh/colors/test_color__colors.py:107:23:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L156'>tests/unit/bokeh/colors/test_color__colors.py:156:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L164'>tests/unit/bokeh/colors/test_color__colors.py:164:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L194'>tests/unit/bokeh/colors/test_color__colors.py:194:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L211'>tests/unit/bokeh/colors/test_color__colors.py:211:23:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L283'>tests/unit/bokeh/colors/test_color__colors.py:283:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L333'>tests/unit/bokeh/colors/test_color__colors.py:333:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L341'>tests/unit/bokeh/colors/test_color__colors.py:341:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L362'>tests/unit/bokeh/colors/test_color__colors.py:362:63:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L365'>tests/unit/bokeh/colors/test_color__colors.py:365:63:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L85'>tests/unit/bokeh/colors/test_color__colors.py:85:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L189'>tests/unit/bokeh/colors/test_named.py:189:19:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/_util_models.py#L121'>tests/unit/bokeh/models/_util_models.py:121:30:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/_util_models.py#L130'>tests/unit/bokeh/models/_util_models.py:130:30:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L155'>tests/unit/bokeh/models/test_annotations.py:155:37:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L497'>tests/unit/bokeh/models/test_annotations.py:497:32:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L498'>tests/unit/bokeh/models/test_annotations.py:498:38:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L547'>tests/unit/bokeh/models/test_annotations.py:547:28:</a> PLR2004 Magic value used in comparison, consider replacing `-1.` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L551'>tests/unit/bokeh/models/test_annotations.py:551:25:</a> PLR2004 Magic value used in comparison, consider replacing `-1.` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_glyphs.py#L356'>tests/unit/bokeh/models/test_glyphs.py:356:34:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_ranges.py#L160'>tests/unit/bokeh/models/test_ranges.py:160:37:</a> PLR2004 Magic value used in comparison, consider replacing `-1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_ranges.py#L71'>tests/unit/bokeh/models/test_ranges.py:71:33:</a> PLR2004 Magic value used in comparison, consider replacing `-1.0` with a constant variable
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR2004 | 35 | 0 | 35 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -35 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/core/test_configuration.py#L229'>tests/core/test_configuration.py:229:16:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/jobs/test_scheduler_job.py#L4941'>tests/jobs/test_scheduler_job.py:4941:24:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/jobs/test_scheduler_job.py#L4966'>tests/jobs/test_scheduler_job.py:4966:24:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py#L87'>tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py:87:41:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/utils/test_sqlalchemy.py#L89'>tests/utils/test_sqlalchemy.py:89:62:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/utils/test_sqlalchemy.py#L90'>tests/utils/test_sqlalchemy.py:90:58:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -29 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L321'>src/bokeh/colors/color.py:321:22:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L337'>src/bokeh/colors/color.py:337:21:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L460'>src/bokeh/colors/color.py:460:22:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L280'>src/bokeh/core/property/numeric.py:280:12:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L280'>src/bokeh/core/property/numeric.py:280:28:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1621'>src/bokeh/palettes.py:1621:17:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L414'>src/bokeh/plotting/contour.py:414:53:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L107'>tests/unit/bokeh/colors/test_color__colors.py:107:23:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L156'>tests/unit/bokeh/colors/test_color__colors.py:156:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L164'>tests/unit/bokeh/colors/test_color__colors.py:164:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L194'>tests/unit/bokeh/colors/test_color__colors.py:194:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L211'>tests/unit/bokeh/colors/test_color__colors.py:211:23:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L283'>tests/unit/bokeh/colors/test_color__colors.py:283:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L333'>tests/unit/bokeh/colors/test_color__colors.py:333:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L341'>tests/unit/bokeh/colors/test_color__colors.py:341:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L362'>tests/unit/bokeh/colors/test_color__colors.py:362:63:</a> PLR2004 Magic value used in comparison, consider replacing `0.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L365'>tests/unit/bokeh/colors/test_color__colors.py:365:63:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_color__colors.py#L85'>tests/unit/bokeh/colors/test_color__colors.py:85:24:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L189'>tests/unit/bokeh/colors/test_named.py:189:19:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/_util_models.py#L121'>tests/unit/bokeh/models/_util_models.py:121:30:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/_util_models.py#L130'>tests/unit/bokeh/models/_util_models.py:130:30:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L155'>tests/unit/bokeh/models/test_annotations.py:155:37:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L497'>tests/unit/bokeh/models/test_annotations.py:497:32:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L498'>tests/unit/bokeh/models/test_annotations.py:498:38:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L547'>tests/unit/bokeh/models/test_annotations.py:547:28:</a> PLR2004 Magic value used in comparison, consider replacing `-1.` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_annotations.py#L551'>tests/unit/bokeh/models/test_annotations.py:551:25:</a> PLR2004 Magic value used in comparison, consider replacing `-1.` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_glyphs.py#L356'>tests/unit/bokeh/models/test_glyphs.py:356:34:</a> PLR2004 Magic value used in comparison, consider replacing `1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_ranges.py#L160'>tests/unit/bokeh/models/test_ranges.py:160:37:</a> PLR2004 Magic value used in comparison, consider replacing `-1.0` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_ranges.py#L71'>tests/unit/bokeh/models/test_ranges.py:71:33:</a> PLR2004 Magic value used in comparison, consider replacing `-1.0` with a constant variable
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR2004 | 35 | 0 | 35 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-02-12 23:05_

I think this is a good change and arguably a "bug" (or at least an oversight) given that we already allow `0` and `1`. Wdyt @zanieb (with respect to gating this in preview vs. not)?

---

_@zanieb approved on 2024-02-13 00:57_

Seems reasonable without preview — looks like a bug.

---

_@charliermarsh approved on 2024-02-13 01:13_

---

_Label `bug` added by @charliermarsh on 2024-02-13 01:13_

---

_Merged by @charliermarsh on 2024-02-13 01:21_

---

_Closed by @charliermarsh on 2024-02-13 01:21_

---

_Branch deleted on 2024-03-07 02:22_

---
