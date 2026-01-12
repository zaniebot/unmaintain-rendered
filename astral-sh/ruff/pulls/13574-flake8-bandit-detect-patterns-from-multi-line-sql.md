```yaml
number: 13574
title: "[`flake8-bandit`] Detect patterns from multi line SQL statements (`S608`)"
type: pull_request
state: merged
author: DataEnggNerd
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-09-30T17:47:38Z
updated_at: 2024-10-17T07:13:32Z
url: https://github.com/astral-sh/ruff/pull/13574
synced_at: 2026-01-12T15:55:44Z
```

# [`flake8-bandit`] Detect patterns from multi line SQL statements (`S608`)

---

_@DataEnggNerd_

## Summary

The regex written for identifying possible sql injections was not identifying some patterns, mostly multiple lined queries. 

Fixes include:
- updated regex with the pattern from https://github.com/PyCQA/bandit/blob/4ac55dfaaacf2083497831294b2dcd7e679f8428/bandit/plugins/injection_sql.py
- removed `concatenated_f_string`  function, which was escaping `\n`

Fixes #12044

## Test Plan

- Added reported patterns from #12044 to `S608.py` test fixtures. 


---

_Comment by @github-actions[bot] on 2024-09-30 18:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -1 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b3b48501c790d0b10dadca08ce403a820eb16972/providers/tests/system/google/cloud/dataproc_metastore/example_dataproc_metastore_hive_partition_sensor.py#L105'>providers/tests/system/google/cloud/dataproc_metastore/example_dataproc_metastore_hive_partition_sensor.py:105:35:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L871'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:871:9:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/databases/filters_test.py#L125'>tests/unit_tests/databases/filters_test.py:125:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L477'>tests/unit_tests/jinja_context_test.py:477:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L485'>tests/unit_tests/jinja_context_test.py:485:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L493'>tests/unit_tests/jinja_context_test.py:493:12:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 6 | 5 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -1 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b3b48501c790d0b10dadca08ce403a820eb16972/providers/tests/system/google/cloud/dataproc_metastore/example_dataproc_metastore_hive_partition_sensor.py#L105'>providers/tests/system/google/cloud/dataproc_metastore/example_dataproc_metastore_hive_partition_sensor.py:105:35:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L871'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:871:9:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/databases/filters_test.py#L125'>tests/unit_tests/databases/filters_test.py:125:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L477'>tests/unit_tests/jinja_context_test.py:477:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L485'>tests/unit_tests/jinja_context_test.py:485:12:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/superset/blob/bad48d072224a4cc47a263a71aef02ba8f70e1e5/tests/unit_tests/jinja_context_test.py#L493'>tests/unit_tests/jinja_context_test.py:493:12:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 6 | 5 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @DataEnggNerd on 2024-09-30 18:06_

@MichaReiser Can you review the changes please? 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:14 on 2024-10-01 03:22_

What's the motivation for including additional modifiers like `x` and `s`? We don't use any comments in the regex so `x` shouldn't be required. And the `\s` group should include the newline character so we might not require the `s` modifier as well. I might be wrong here so it would be helpful if you can provide some context here :)

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:92 on 2024-10-01 03:24_

I don't think we should avoid checking f-strings otherwise we're reducing the surface area for the rule. The fix should be to just remove the `escape_default` call but we still need to concatenated the f-strings.

The reason that string literals aren't being concatenated explicitly is because the `to_str` call above (`string.value.to_str()`) will concatenated them internally. 

Does this make sense?

---

_@dhruvmanila requested changes on 2024-10-01 03:25_

---

_Label `bug` added by @dhruvmanila on 2024-10-01 03:25_

---

_@DataEnggNerd reviewed on 2024-10-07 03:30_

---

_Review comment by @DataEnggNerd on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:14 on 2024-10-07 03:30_

I wanted to implement the exact RegEx available in bandit's codebase, so translated `re.DOTALL` to `?(s)`.
I couldn't recollect the reason why did I add `x` modifier though. Removed x and ran tests, which passes all tests successfully. 

Without `s` modifier as well, the tests are passing. So in curiosity, should I keep the pattern same from bandit? 

---

_@MichaReiser reviewed on 2024-10-07 08:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:14 on 2024-10-07 08:49_

I would try to keep the changes to our pattern minimal to avoid unwanted changes to the rule's semantic

---

_Comment by @DataEnggNerd on 2024-10-10 03:59_

⚠️ Do not consider previous commit for review. Something messed up and I have accepted wrong snap. 

---

_Comment by @MichaReiser on 2024-10-13 14:04_

@DataEnggNerd is the PR now ready for review again or are there still issues with the snapshots?

---

_Comment by @DataEnggNerd on 2024-10-14 14:57_

> @DataEnggNerd is the PR now ready for review again or are there still issues with the snapshots?

@MichaReiser I am facing trouble in terms of test cases. When I revert to last version in which @dhruvmanila has given reviews, only 37 out of 58 patterns are detected. Not able to understand why such changes are happening. 

Additionally, with the changes recommended, 52 test patterns are identified. 

---

_Comment by @MichaReiser on 2024-10-14 16:04_

@DataEnggNerd I suggest reverting the regex changes. Reverting them makes most diagnostic reappear. Could you let me know which diagnostics are missing when using the existing regex? It would help me helping you :)

---

_Comment by @DataEnggNerd on 2024-10-14 18:12_

@MichaReiser 
Below are the few of the patterns not being identified 
```python
query10 = f"DELETE FROM table WHERE var = {var}"
query18 = f"UPDATE {table} SET var = {var}"
query23 = f"select * from table where var = {var}"
query28 = f"delete from table where var = {var}"
query32 = f"insert into {table} values var = {var}"
query36 = f"update {table} set var = {var}"
query43 = cursor.execute(f"SELECT * FROM table WHERE var = {var}")
```

---

_Comment by @MichaReiser on 2024-10-15 06:16_

@DataEnggNerd would you mind pushing your latest changes? The PR still shows changes to the Regex and it comments out f-string handling. 

I did copy paste the examples into Regex101 and it at least matches the first example (I didnt' test th other examples)

---

_Comment by @DataEnggNerd on 2024-10-15 19:57_

@MichaReiser I have pushed the changes. 
This version of code is able to identify 48 patterns only. 
Unidentified patterns 
```py
def query40():
    return f"""
    SELECT *
    FROM table
    WHERE var = {var}
    """

query46 = "INSERT table VALUES (%s)" % (var,)

# # REPLACE (e.g. MySQL and derivatives, SQLite)
query47 = "REPLACE INTO table VALUES (%s)" % (var,)
query48 = "REPLACE table VALUES (%s)" % (var,)

def query52():
    return f"""
SELECT {var}
    FROM bar
    """

def query53():
    return f"""
    SELECT
        {var}
    FROM bar
    """

def query54():
    return f"""
    SELECT {var}
    FROM
        bar
    """

query56 = f"""SELECT *
FROM {var}.table
"""

query57 = f"""
SELECT *
FROM {var}.table
"""

query58 = f"SELECT\
 * FROM {var}.table"

```

---

_@dhruvmanila reviewed on 2024-10-16 04:50_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:23 on 2024-10-16 04:50_

The regex seems to be missing a few things. Sorry for a lot of back and forth but we should revert it back to the original regex and only update the relevant parts that are required to fix the linked issue.

1. The `select` should be `select\s+.*\s+from\s` (currently it doesn't account for multiple whitespaces)
2. The `replace` is missing in the updated regex, so it should be `(insert|replace)\s+.*\s+values\s`
3. The `update` should be `update\s+.*\s+set\s` so we use the similar pattern as others for whitespaces (`\s+.*\s+`) 

---

_Renamed from "[flake8-bandit] Detect patterns from multi line SQL statements (S608)" to "[`flake8-bandit`] Detect patterns from multi line SQL statements (`S608`)" by @dhruvmanila on 2024-10-16 04:50_

---

_@dhruvmanila reviewed on 2024-10-16 04:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:23 on 2024-10-16 04:56_

With the updated regex, it matches all the examples that you've specified in https://github.com/astral-sh/ruff/pull/13574#issuecomment-2414889325 except for the last one which contains a line continuation character. Refer https://regex101.com/r/u77aw8/2

---

_@DataEnggNerd reviewed on 2024-10-16 16:19_

---

_Review comment by @DataEnggNerd on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:23 on 2024-10-16 16:19_

Thanks a ton, it was a complete mess. 
Yes, the suggested changes resulted in 57/58 matches. Trying to figure out the reason for missing 
```py
query58 = f"SELECT\
 * FROM {var}.table"
```

---

_@MichaReiser reviewed on 2024-10-16 16:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:23 on 2024-10-16 16:42_

I would be fine landing this PR without line continuation support because I understand that line continuation weren't supported before. We can create a separate PR to add line continuation support. Wdyt?

---

_Review comment by @DataEnggNerd on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:23 on 2024-10-16 18:39_

Yes, it does make sense. 
Line continuation was a part of additional test cases only. 

---

_@DataEnggNerd reviewed on 2024-10-16 18:39_

---

_Comment by @DataEnggNerd on 2024-10-16 19:11_

@MichaReiser @dhruvmanila Sorry to bug you again. 
Tests are failing related to salsa crate 😓 . Not able to figure out how to handle this
```console
error[E0463]: can't find crate for `salsa`
 --> crates/ruff_db/src/files.rs:6:5
  |
6 | use salsa::{Durability, Setter};
  |     ^^^^^ can't find crate
```

---

_Closed by @AlexWaygood on 2024-10-17 00:03_

---

_Reopened by @AlexWaygood on 2024-10-17 00:03_

---

_Comment by @DataEnggNerd on 2024-10-17 02:43_

> @MichaReiser @dhruvmanila Sorry to bug you again. Tests are failing related to salsa crate 😓 . Not able to figure out how to handle this
> 
> ```
> error[E0463]: can't find crate for `salsa`
>  --> crates/ruff_db/src/files.rs:6:5
>   |
> 6 | use salsa::{Durability, Setter};
>   |     ^^^^^ can't find crate
> ```

This issue is resolved. Seems to be something irrelevant to the changes in this PR. 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S608.py`:4 on 2024-10-17 04:55_

Let's remove this variable definitions to keep the snapshot changes to a minimum so that it's easy to review the actual changes.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S608.py`:84 on 2024-10-17 04:56_

Same as above, let's remove this variable definition.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:14 on 2024-10-17 04:57_

Sorry, I didn't notice earlier but we should keep `\b` in the regex otherwise we'd also match the word that ends with "select", "delete", etc.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:91 on 2024-10-17 04:58_

Let's keep this comment as it shows an example of what we're trying to match here.

---

_@dhruvmanila reviewed on 2024-10-17 05:00_

Thanks for your patience. There are only a few minor changes required but otherwise this looks good to go.

---

_@MichaReiser approved on 2024-10-17 05:37_

Thanks for your persistent work on this. 
I went ahead and addressed @dhruvmanila's feedback. Let's merge this PR :)

---

_Merged by @MichaReiser on 2024-10-17 05:42_

---

_Closed by @MichaReiser on 2024-10-17 05:42_

---

_Comment by @DataEnggNerd on 2024-10-17 07:13_

@MichaReiser @dhruvmanila 
Thank for the detailed guidance throughout this issue. 
Looking forward to contribute more, with lesser noob questions 😅. 

---
