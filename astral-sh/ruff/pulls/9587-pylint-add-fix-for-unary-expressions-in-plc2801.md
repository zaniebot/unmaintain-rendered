```yaml
number: 9587
title: "[`pylint`] - add fix for unary expressions in `PLC2801`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: tweak-PLC2801
created_at: 2024-01-20T01:59:10Z
updated_at: 2024-03-04T10:25:17Z
url: https://github.com/astral-sh/ruff/pull/9587
synced_at: 2026-01-12T15:55:29Z
```

# [`pylint`] - add fix for unary expressions in `PLC2801`

---

_@diceroll123_

## Summary

Closes #9572

Don't go easy on this review!

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-20 02:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+48 -0 violations, +0 -48 fixes in 5 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/amazon/aws/hooks/test_base_aws.py#L556'>tests/providers/amazon/aws/hooks/test_base_aws.py:556:17:</a> PLC2801 Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/amazon/aws/hooks/test_base_aws.py#L556'>tests/providers/amazon/aws/hooks/test_base_aws.py:556:17:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
+ <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/google/common/hooks/test_discovery_api.py#L134'>tests/providers/google/common/hooks/test_discovery_api.py:134:17:</a> PLC2801 Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/google/common/hooks/test_discovery_api.py#L134'>tests/providers/google/common/hooks/test_discovery_api.py:134:17:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
+ <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/google/common/hooks/test_discovery_api.py#L140'>tests/providers/google/common/hooks/test_discovery_api.py:140:17:</a> PLC2801 Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/apache/airflow/blob/f7f005f180221a1ba5dc17ef4ff49ee21a46b540/tests/providers/google/common/hooks/test_discovery_api.py#L140'>tests/providers/google/common/hooks/test_discovery_api.py:140:17:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +0 -24 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/commands/local/lib/test_debug_context.py#L32'>tests/unit/commands/local/lib/test_debug_context.py:32:25:</a> PLC2801 Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/commands/local/lib/test_debug_context.py#L32'>tests/unit/commands/local/lib/test_debug_context.py:32:25:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/commands/local/lib/test_debug_context.py#L45'>tests/unit/commands/local/lib/test_debug_context.py:45:26:</a> PLC2801 Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/commands/local/lib/test_debug_context.py#L45'>tests/unit/commands/local/lib/test_debug_context.py:45:26:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L1995'>tests/unit/local/apigw/test_local_apigw_service.py:1995:29:</a> PLC2801 Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L1995'>tests/unit/local/apigw/test_local_apigw_service.py:1995:29:</a> PLC2801 [*] Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L1995'>tests/unit/local/apigw/test_local_apigw_service.py:1995:48:</a> PLC2801 Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L1995'>tests/unit/local/apigw/test_local_apigw_service.py:1995:48:</a> PLC2801 [*] Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L2000'>tests/unit/local/apigw/test_local_apigw_service.py:2000:29:</a> PLC2801 Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L2000'>tests/unit/local/apigw/test_local_apigw_service.py:2000:29:</a> PLC2801 [*] Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L2000'>tests/unit/local/apigw/test_local_apigw_service.py:2000:48:</a> PLC2801 Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/613b12df945d0dde53975e92d2976394a21c48b9/tests/unit/local/apigw/test_local_apigw_service.py#L2000'>tests/unit/local/apigw/test_local_apigw_service.py:2000:48:</a> PLC2801 [*] Unnecessary dunder call to `__hash__`. Use `hash()` builtin.
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -0 violations, +0 -14 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_meta.py#L677'>securedrop/pretty_bad_protocol/_meta.py:677:54:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
- <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_meta.py#L677'>securedrop/pretty_bad_protocol/_meta.py:677:54:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_meta.py#L688'>securedrop/pretty_bad_protocol/_meta.py:688:59:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
- <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_meta.py#L688'>securedrop/pretty_bad_protocol/_meta.py:688:59:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_util.py#L364'>securedrop/pretty_bad_protocol/_util.py:364:11:</a> PLC2801 Unnecessary dunder call to `__str__`. Use `str()` builtin.
- <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/pretty_bad_protocol/_util.py#L364'>securedrop/pretty_bad_protocol/_util.py:364:11:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/tests/test_db.py#L76'>securedrop/tests/test_db.py:76:9:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
- <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/tests/test_db.py#L76'>securedrop/tests/test_db.py:76:9:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/tests/test_db.py#L83'>securedrop/tests/test_db.py:83:9:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
- <a href='https://github.com/freedomofpress/securedrop/blob/1486da6ffbbe80ee8ea0d7eb69ce7f085a5cbb8f/securedrop/tests/test_db.py#L83'>securedrop/tests/test_db.py:83:9:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/dd6ad25509825cf7fa70b29d36c737c89797e50d/pymilvus/client/prepare.py#L1196'>pymilvus/client/prepare.py:1196:24:</a> PLC2801 Unnecessary dunder call to `__str__`. Use `str()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/dd6ad25509825cf7fa70b29d36c737c89797e50d/pymilvus/client/prepare.py#L1196'>pymilvus/client/prepare.py:1196:24:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/dd6ad25509825cf7fa70b29d36c737c89797e50d/pymilvus/orm/iterator.py#L247'>pymilvus/orm/iterator.py:247:19:</a> PLC2801 Unnecessary dunder call to `__len__`. Use `len()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/dd6ad25509825cf7fa70b29d36c737c89797e50d/pymilvus/orm/iterator.py#L247'>pymilvus/orm/iterator.py:247:19:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+48 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/arrays/sparse/test_arithmetics.py#L407'>pandas/tests/arrays/sparse/test_arithmetics.py:407:14:</a> PLC2801 Unnecessary dunder call to `__add__`. Use `+` operator.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/arrays/string_/test_string.py#L195'>pandas/tests/arrays/string_/test_string.py:195:12:</a> PLC2801 Unnecessary dunder call to `__add__`. Use `+` operator.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/arrays/string_/test_string.py#L211'>pandas/tests/arrays/string_/test_string.py:211:12:</a> PLC2801 Unnecessary dunder call to `__add__`. Use `+` operator.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/base/test_conversion.py#L135'>pandas/tests/base/test_conversion.py:135:28:</a> PLC2801 Unnecessary dunder call to `__iter__`. Use `iter()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/base/test_conversion.py#L53'>pandas/tests/base/test_conversion.py:53:28:</a> PLC2801 Unnecessary dunder call to `__iter__`. Use `iter()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/base/test_conversion.py#L85'>pandas/tests/base/test_conversion.py:85:28:</a> PLC2801 Unnecessary dunder call to `__iter__`. Use `iter()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/dtypes/test_dtypes.py#L230'>pandas/tests/dtypes/test_dtypes.py:230:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/groupby/test_groupby.py#L2156'>pandas/tests/groupby/test_groupby.py:2156:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/groupby/test_groupby.py#L2159'>pandas/tests/groupby/test_groupby.py:2159:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/groupby/test_grouping.py#L1116'>pandas/tests/groupby/test_grouping.py:1116:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L105'>pandas/tests/indexes/multi/test_formats.py:105:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L111'>pandas/tests/indexes/multi/test_formats.py:111:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L120'>pandas/tests/indexes/multi/test_formats.py:120:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L153'>pandas/tests/indexes/multi/test_formats.py:153:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L158'>pandas/tests/indexes/multi/test_formats.py:158:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L173'>pandas/tests/indexes/multi/test_formats.py:173:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L51'>pandas/tests/indexes/multi/test_formats.py:51:22:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L63'>pandas/tests/indexes/multi/test_formats.py:63:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L69'>pandas/tests/indexes/multi/test_formats.py:69:18:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L81'>pandas/tests/indexes/multi/test_formats.py:81:22:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/multi/test_formats.py#L93'>pandas/tests/indexes/multi/test_formats.py:93:22:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/test_base.py#L778'>pandas/tests/indexes/test_base.py:778:33:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/indexes/test_common.py#L202'>pandas/tests/indexes/test_common.py:202:29:</a> PLC2801 Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/scalar/timedelta/test_arithmetic.py#L1156'>pandas/tests/scalar/timedelta/test_arithmetic.py:1156:12:</a> PLC2801 Unnecessary dunder call to `__add__`. Use `+` operator.
+ <a href='https://github.com/pandas-dev/pandas/blob/d2bc8460f64701ea7824021da5af98238de8168b/pandas/tests/scalar/timedelta/test_arithmetic.py#L1157'>pandas/tests/scalar/timedelta/test_arithmetic.py:1157:12:</a> PLC2801 Unnecessary dunder call to `__sub__`. Use `-` operator.
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2801 | 96 | 48 | 0 | 0 | 48 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-26 04:02_

---

_Comment by @diceroll123 on 2024-01-26 04:09_

I see now that the `set_fix` in both sides of the unary if statement can be moved out, but I won't touch it until after you've reviewed @charliermarsh 

---

_Comment by @charliermarsh on 2024-01-26 04:12_

@diceroll123 - Feel free to change, I won't get to this until tomorrow.

---

_Comment by @codspeed-hq[bot] on 2024-01-26 04:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:tweak-PLC2801)

### Merging #9587 will **not alter performance**

<sub>Comparing <code>diceroll123:tweak-PLC2801</code> (1a14221) with <code>main</code> (c007b17)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Label `autofix` added by @MichaReiser on 2024-01-26 07:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:161 on 2024-01-26 09:06_

The section here could use a comment explaining on why we're doing what we're doing below because it's not immediately obvious what it is about. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:173 on 2024-01-26 09:07_

What happens if we have `a.__sub__(((((b)))))`?


You can use `call.arguments.start` (or `end`) to get the parentheses offsets of the call expression and retain those.  

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:168 on 2024-01-26 09:09_

I wonder if the fix produces invalid code if you have

```python
4.__sub__(
    3 
    + 
    4
)
```

What i understand is that we fix this to

```python
a -
    3
    +
    4
```

which Python doesn't like very much

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:169 on 2024-01-26 09:11_

Can we start after the `attribute` to reduce the text that need to be searched?

---

_@MichaReiser reviewed on 2024-01-26 09:14_

Would it be possible to generalize the fix to correctly handle operator precedence in general. There are some more cases mentioned on the linked Issue that this PR doesn't address.

---

_@diceroll123 reviewed on 2024-01-27 00:40_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:169 on 2024-01-27 00:40_

Can't start after the value in this case.

We're looking for this,

```
(-a).__sub__(1)
###^
```

and the difference between

`(-a).__sub__(1)` and `-a.__sub__(1)`

It could possibly be optimized another way though, perhaps by checking the tokens between `value` and the first `Dot` 🤔 

---

_Comment by @diceroll123 on 2024-01-27 05:06_

Made some tweaks!

First and foremost: the fix is marked as unsafe. This could be conditional with more type logic in a future iteration, for sure.

The arguments within the dunder method call will now stay in parentheses if it's anything more complex than a literal/name/attribute. I'm sure that logic may be fine to expand into other expression types.

So, this example:

```python
print(a.__sub__(
    3
    +
    4
))
```

now turns into 
```python
print(a - (3
    +
    4
))
```

---

And since you explicitly asked,

```python
print(a.__sub__(((((2 - 1))))))
```

is now reduced to

```python
print(a - (2 - 1))  # PLC2801
```

Semantically it's the same, regardless of extra parens. _`UP034` and/or the formatter could take care of those extra parens if we did want to keep them though..._

---

Looking forward to opinions on improving it further! 😄 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:130 on 2024-02-22 17:05_

I think we can exclude some more expressions here

* Calls
* Unary expressions 
* Lambda
* If expressions (I think)? 
* Dict / Set / List / Tuple and all comprehension variants (everything with parentheses)
* Generators
* Subscripts
* Starred (I think)
* Slices?

---

_@MichaReiser reviewed on 2024-02-22 17:05_

---

_@MichaReiser approved on 2024-02-22 17:06_

Sorry for the late review.  The `uv` release took a lot of attention recently. 

This is an excellent improvement. I think there are a few more situations where adding the parentheses isn't strictly necessary. You might want to move the logic into its own function to avoid coding it twice. 



---

_@diceroll123 reviewed on 2024-03-02 05:09_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:130 on 2024-03-02 05:09_

Reworked it, added all of the above except Unary (because that's what got us this PR in the first place hah)

---

_Review requested from @MichaReiser by @diceroll123 on 2024-03-02 05:48_

---

_@MichaReiser approved on 2024-03-04 10:25_

Thank you!

---

_Merged by @MichaReiser on 2024-03-04 10:25_

---

_Closed by @MichaReiser on 2024-03-04 10:25_

---
