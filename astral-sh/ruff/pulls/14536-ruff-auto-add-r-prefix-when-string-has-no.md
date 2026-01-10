```yaml
number: 14536
title: "[`ruff`] Auto-add `r` prefix when string has no backslashes for `unraw-re-pattern (RUF039)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: raw-fix
created_at: 2024-11-22T18:18:34Z
updated_at: 2024-11-22T21:09:58Z
url: https://github.com/astral-sh/ruff/pull/14536
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Auto-add `r` prefix when string has no backslashes for `unraw-re-pattern (RUF039)`

---

_Pull request opened by @dylwil3 on 2024-11-22 18:18_

This PR adds a sometimes-available, safe autofix for [unraw-re-pattern (RUF039)](https://docs.astral.sh/ruff/rules/unraw-re-pattern/#unraw-re-pattern-ruf039), which prepends an `r` prefix. It is used only when the string in question has no backslahses (and also does not have a `u` prefix, since that causes a syntax error.)

Closes #14527

Notes: 
- Test fixture unchanged, but snapshot changed to include fix messages.
- This fix is automatically only available in preview since the rule itself is in preview


---

_Label `fixes` added by @dylwil3 on 2024-11-22 18:18_

---

_Label `preview` added by @dylwil3 on 2024-11-22 18:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:170 on 2024-11-22 18:21_

I don't the we need the performance of memchr here. That's why I would use .contains which is also easier to read 

---

_@MichaReiser approved on 2024-11-22 18:21_

---

_Comment by @github-actions[bot] on 2024-11-22 18:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -20 violations, +272 -0 fixes in 13 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L222'>rasa/utils/io.py:222:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L222'>rasa/utils/io.py:222:9:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L231'>rasa/utils/io.py:231:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L231'>rasa/utils/io.py:231:9:</a> RUF039 [*] First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +64 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/breeze/src/airflow_breeze/params/build_prod_params.py#L81'>dev/breeze/src/airflow_breeze/params/build_prod_params.py:81:21:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/breeze/src/airflow_breeze/params/build_prod_params.py#L81'>dev/breeze/src/airflow_breeze/params/build_prod_params.py:81:21:</a> RUF039 [*] First argument to `re.match()` is not raw string
- <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/breeze/src/airflow_breeze/utils/run_tests.py#L109'>dev/breeze/src/airflow_breeze/utils/run_tests.py:109:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/breeze/src/airflow_breeze/utils/run_tests.py#L109'>dev/breeze/src/airflow_breeze/utils/run_tests.py:109:19:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/perf/dags/elastic_dag.py#L73'>dev/perf/dags/elastic_dag.py:73:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/dev/perf/dags/elastic_dag.py#L73'>dev/perf/dags/elastic_dag.py:73:19:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/docs/exts/docs_build/lint_checks.py#L46'>docs/exts/docs_build/lint_checks.py:46:46:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/docs/exts/docs_build/lint_checks.py#L46'>docs/exts/docs_build/lint_checks.py:46:46:</a> RUF039 [*] First argument to `re.findall()` is not raw string
- <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/helm_tests/airflow_aux/test_pod_template_file.py#L358'>helm_tests/airflow_aux/test_pod_template_file.py:358:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/5b2a96ee72a4bd7b7fbd4925c2f8c8468f047a64/helm_tests/airflow_aux/test_pod_template_file.py#L358'>helm_tests/airflow_aux/test_pod_template_file.py:358:26:</a> RUF039 [*] First argument to `re.search()` is not raw string
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +138 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/RELEASING/changelog.py#L276'>RELEASING/changelog.py:276:26:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/RELEASING/changelog.py#L276'>RELEASING/changelog.py:276:26:</a> RUF039 [*] First argument to `re.match()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L66'>scripts/build_docker.py:66:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L66'>scripts/build_docker.py:66:23:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L68'>scripts/build_docker.py:68:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L68'>scripts/build_docker.py:68:23:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L70'>scripts/build_docker.py:70:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L70'>scripts/build_docker.py:70:23:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L70'>scripts/build_docker.py:70:51:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/scripts/build_docker.py#L70'>scripts/build_docker.py:70:51:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/athena.py#L30'>superset/db_engine_specs/athena.py:30:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/athena.py#L30'>superset/db_engine_specs/athena.py:30:5:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L76'>superset/db_engine_specs/bigquery.py:76:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L76'>superset/db_engine_specs/bigquery.py:76:5:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L77'>superset/db_engine_specs/bigquery.py:77:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L77'>superset/db_engine_specs/bigquery.py:77:5:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L90'>superset/db_engine_specs/bigquery.py:90:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/bigquery.py#L90'>superset/db_engine_specs/bigquery.py:90:5:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/denodo.py#L29'>superset/db_engine_specs/denodo.py:29:46:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/denodo.py#L29'>superset/db_engine_specs/denodo.py:29:46:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/denodo.py#L30'>superset/db_engine_specs/denodo.py:30:48:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/denodo.py#L30'>superset/db_engine_specs/denodo.py:30:48:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/apache/superset/blob/abf3790ea6ada4b5ec07020d691d0659388caf52/superset/db_engine_specs/denodo.py#L32'>superset/db_engine_specs/denodo.py:32:9:</a> RUF039 First argument to `re.compile()` is not raw string
... 115 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/strings.py#L91'>src/bokeh/util/strings.py:91:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/strings.py#L91'>src/bokeh/util/strings.py:91:19:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L203'>tests/unit/bokeh/io/test_export.py:203:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L203'>tests/unit/bokeh/io/test_export.py:203:9:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L204'>tests/unit/bokeh/io/test_export.py:204:13:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L204'>tests/unit/bokeh/io/test_export.py:204:13:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L205'>tests/unit/bokeh/io/test_export.py:205:13:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L205'>tests/unit/bokeh/io/test_export.py:205:13:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L206'>tests/unit/bokeh/io/test_export.py:206:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L206'>tests/unit/bokeh/io/test_export.py:206:9:</a> RUF039 [*] First argument to `re.compile()` is not raw string
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/__init__.py#L1396'>ibis/backends/__init__.py:1396:17:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/__init__.py#L1396'>ibis/backends/__init__.py:1396:17:</a> RUF039 [*] First argument to `re.match()` is not raw string
- <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/flink/__init__.py#L320'>ibis/backends/flink/__init__.py:320:17:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/flink/__init__.py#L320'>ibis/backends/flink/__init__.py:320:17:</a> RUF039 [*] First argument to `re.search()` is not raw string
- <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/sql/compilers/pyspark.py#L362'>ibis/backends/sql/compilers/pyspark.py:362:27:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/sql/compilers/pyspark.py#L362'>ibis/backends/sql/compilers/pyspark.py:362:27:</a> RUF039 [*] First argument to `re.sub()` is not raw string
- <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/tests/test_client.py#L1047'>ibis/backends/tests/test_client.py:1047:13:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/tests/test_client.py#L1047'>ibis/backends/tests/test_client.py:1047:13:</a> RUF039 [*] First argument to `re.search()` is not raw string
- <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/tests/test_client.py#L1068'>ibis/backends/tests/test_client.py:1068:13:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/b32a6f37a55550695ce33c1cdd88d01b90e8e979/ibis/backends/tests/test_client.py#L1068'>ibis/backends/tests/test_client.py:1068:13:</a> RUF039 [*] First argument to `re.search()` is not raw string
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/centromere/ctx.py#L464'>src/latch_cli/centromere/ctx.py:464:26:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/centromere/ctx.py#L464'>src/latch_cli/centromere/ctx.py:464:26:</a> RUF039 [*] First argument to `re.match()` is not raw string
- <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/init/init.py#L309'>src/latch_cli/services/init/init.py:309:18:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/init/init.py#L309'>src/latch_cli/services/init/init.py:309:18:</a> RUF039 [*] First argument to `re.search()` is not raw string
- <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/init/init.py#L316'>src/latch_cli/services/init/init.py:316:18:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/init/init.py#L316'>src/latch_cli/services/init/init.py:316:18:</a> RUF039 [*] First argument to `re.search()` is not raw string
- <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/register/register.py#L59'>src/latch_cli/services/register/register.py:59:36:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/register/register.py#L59'>src/latch_cli/services/register/register.py:59:36:</a> RUF039 [*] First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/db.py#L151'>lnbits/db.py:151:34:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/db.py#L151'>lnbits/db.py:151:34:</a> RUF039 [*] First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/indexes/datetimes/test_date_range.py#L787'>pandas/tests/indexes/datetimes/test_date_range.py:787:29:</a> RUF039 First argument to `re.split()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/io/formats/style/test_style.py#L834'>pandas/tests/io/formats/style/test_style.py:834:26:</a> RUF039 First argument to `re.findall()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/io/formats/test_format.py#L1115'>pandas/tests/io/formats/test_format.py:1115:35:</a> RUF039 First argument to `re.findall()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/io/test_html.py#L477'>pandas/tests/io/test_html.py:477:41:</a> RUF039 First argument to `re.compile()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/strings/test_find_replace.py#L613'>pandas/tests/strings/test_find_replace.py:613:22:</a> RUF039 First argument to `re.compile()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/pandas/tests/strings/test_find_replace.py#L639'>pandas/tests/strings/test_find_replace.py:639:22:</a> RUF039 First argument to `re.compile()` is not raw string
- <a href='https://github.com/pandas-dev/pandas/blob/d4ae654b18ec6a42b1bee9a7df8d786f02aca21b/web/pandas_web.py#L99'>web/pandas_web.py:99:31:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_integration.py#L31'>tests/test_integration.py:31:21:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_integration.py#L31'>tests/test_integration.py:31:21:</a> RUF039 [*] First argument to `re.compile()` is not raw string
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_integration.py#L32'>tests/test_integration.py:32:21:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_integration.py#L32'>tests/test_integration.py:32:21:</a> RUF039 [*] First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/e1c74f08f12b8bec6afe266951f0756dc1b43ebe/stdlib/csv.pyi#L148'>stdlib/csv.pyi:148:31:</a> W292 No newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L11'>src/poetry/console/logging/formatters/builder_formatter.py:11:26:</a> RUF039 First argument to `re.sub()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L13'>src/poetry/console/logging/formatters/builder_formatter.py:13:26:</a> RUF039 First argument to `re.sub()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L15'>src/poetry/console/logging/formatters/builder_formatter.py:15:26:</a> RUF039 First argument to `re.sub()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L18'>src/poetry/console/logging/formatters/builder_formatter.py:18:17:</a> RUF039 First argument to `re.sub()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py#L22'>src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py:22:13:</a> RUF039 First argument to `re.match()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py#L23'>src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py:23:13:</a> RUF039 First argument to `re.match()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/dependency_specification.py#L192'>src/poetry/utils/dependency_specification.py:192:13:</a> RUF039 First argument to `re.sub()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/conftest.py#L378'>tests/conftest.py:378:20:</a> RUF039 First argument to `re.compile()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/installation/conftest.py#L43'>tests/installation/conftest.py:43:20:</a> RUF039 First argument to `re.compile()` is not raw string
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/installation/test_chooser.py#L108'>tests/installation/test_chooser.py:108:20:</a> RUF039 First argument to `re.compile()` is not raw string
... 3 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF039 | 292 | 0 | 20 | 272 | 0 |
| W292 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@dylwil3 reviewed on 2024-11-22 18:44_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:170 on 2024-11-22 18:44_

Can you help me get a sense of what size strings the performance starts to make a difference? I only mention it because I have seen some _long_ regexes in my life...

But happy to change it to `contains`!

---

_@MichaReiser reviewed on 2024-11-22 18:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:170 on 2024-11-22 18:47_

It's less about the length of a string and more that the fix code isn't a hot path. 

I don't have too good a sense for when to use memchr otherwise, beyond what the [crate documentation](https://docs.rs/memchr/latest/memchr/#why-use-this-crate) mentions

---

_Comment by @MichaReiser on 2024-11-22 18:48_

Can you manually run your change on the poetry repo. I suspect that it panics and that this is the reason why we see fewer violations

---

_Comment by @dylwil3 on 2024-11-22 18:52_

> Can you manually run your change on the poetry repo. I suspect that it panics and that this is the reason why we see fewer violations

Good catch, will do!

---

_Comment by @MichaReiser on 2024-11-22 18:53_

Or is this the same as with TC006 that both poetry and panda set `fix = true`.

---

_Comment by @dylwil3 on 2024-11-22 18:57_

> Or is this the same as with TC006 that both poetry and panda set `fix = true`.

Yep it's that, just checked!

---

_Merged by @dylwil3 on 2024-11-22 21:09_

---

_Closed by @dylwil3 on 2024-11-22 21:09_

---

_Branch deleted on 2024-11-22 21:09_

---
