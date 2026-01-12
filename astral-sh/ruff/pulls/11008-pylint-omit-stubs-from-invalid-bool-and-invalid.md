```yaml
number: 11008
title: "[`pylint`] Omit stubs from `invalid-bool` and `invalid-str-return-type`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/is-stub
created_at: 2024-04-18T01:41:33Z
updated_at: 2024-04-18T02:03:22Z
url: https://github.com/astral-sh/ruff/pull/11008
synced_at: 2026-01-12T15:55:34Z
```

# [`pylint`] Omit stubs from `invalid-bool` and `invalid-str-return-type`

---

_@charliermarsh_

## Summary

Reflecting some improvements that were made in https://github.com/astral-sh/ruff/pull/10959.


---

_Label `rule` added by @charliermarsh on 2024-04-18 01:41_

---

_Label `preview` added by @charliermarsh on 2024-04-18 01:41_

---

_Merged by @charliermarsh on 2024-04-18 01:57_

---

_Closed by @charliermarsh on 2024-04-18 01:57_

---

_Branch deleted on 2024-04-18 01:57_

---

_Comment by @github-actions[bot] on 2024-04-18 02:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3f1ffd0068b80a9f9c2eac604b12f71bc6ab9c02/zerver/lib/validator.py#L684'>zerver/lib/validator.py:684:9:</a> PLE0307 `__str__` does not return `str`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9a60b9fe8eebc9ca123a2181e4f3431021ed3167/indico/util/passwords.py#L69'>indico/util/passwords.py:69:9:</a> PLE0307 `__str__` does not return `str`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE0307 | 2 | 2 | 0 | 0 | 0 |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 6 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a4ea72dfa86cff184f9ae7d27b747a0d21217c58/src/plasmapy/particles/particle_class.py#L174'>src/plasmapy/particles/particle_class.py:174:9:</a> PLE0304 `__bool__` does not return `bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2778ed536a1530ad07b4a49fa96e3124685e9193/tests/models/test_mappedoperator.py#L76'>tests/models/test_mappedoperator.py:76:13:</a> PLE0304 `__bool__` does not return `bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/98ef69ce096b72c1deaa3abe217b458ea17139c8/ibis/common/deferred.py#L101'>ibis/common/deferred.py:101:9:</a> PLE0304 `__bool__` does not return `bool`
+ <a href='https://github.com/ibis-project/ibis/blob/98ef69ce096b72c1deaa3abe217b458ea17139c8/ibis/expr/types/core.py#L166'>ibis/expr/types/core.py:166:9:</a> PLE0304 `__bool__` does not return `bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3f1ffd0068b80a9f9c2eac604b12f71bc6ab9c02/zerver/lib/validator.py#L684'>zerver/lib/validator.py:684:9:</a> PLE0307 `__str__` does not return `str`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9a60b9fe8eebc9ca123a2181e4f3431021ed3167/indico/util/passwords.py#L69'>indico/util/passwords.py:69:9:</a> PLE0307 `__str__` does not return `str`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE0304 | 4 | 4 | 0 | 0 | 0 |
| PLE0307 | 2 | 2 | 0 | 0 | 0 |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---
