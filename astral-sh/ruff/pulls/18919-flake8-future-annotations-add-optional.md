```yaml
number: 18919
title: "[`flake8-future-annotations`] Add optional integration with `TC` rules (`FA100`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
  - configuration
assignees: []
draft: true
base: main
head: brent/tc-fa100
created_at: 2025-06-24T16:33:04Z
updated_at: 2025-07-24T13:32:44Z
url: https://github.com/astral-sh/ruff/pull/18919
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-future-annotations`] Add optional integration with `TC` rules (`FA100`)

---

_Pull request opened by @ntBre on 2025-06-24 16:33_

## Summary

This PR covers the next step in https://github.com/astral-sh/ruff/issues/18502 after adding an autofix in https://github.com/astral-sh/ruff/pull/18903: checking for opportunities to emit FA100 that would cause TC001, TC002, and TC003 to activate. The main changes here are:
- A new `lint.flake8-future-annotations.aggressive` config option to enable this behavior (not sold on the name, so this is basically a placeholder)
- A new return type from the helper method `is_typing_reference` to help distinguish the cases where applying FA100 would change the applicability of the TC rules
- A slightly modified `future_rewritable_type_annotation` implementation to accept anything `Ranged` and customize the error message
  - This changed some snapshots (and the ecosystem hits), but I think using the actual text from the file instead of the resolved name makes sense.

## Test Plan

New tests with both FA100 and the TC rules active.

I opted only to emit FA100 in these cases, but we should double check that applying its autofix then also triggers the TC rules after #18903 lands, maybe with a CLI test.

I also did some manual testing for duplicates with the setup below. Everything seems okay when selecting combinations of the FA and TC rules, even when backporting the fix from #18903, but maybe someone else has a better idea for a test here.

<details><summary>Manual testing setup</summary>

```py
# try.py
import collections

import pandas

from . import local_module


# FA102
def func(obj: dict[str, int | None]) -> None: ...

# FA102
def func(obj: dict[str, int | None]) -> None: ...

# TC003
def func(c: collections.Counter) -> int: ...

# TC002
def func(df: pandas.DataFrame) -> int: ...

# TC001
def func(sized: local_module.Container) -> int: ...
```

```shell
# run.sh
set -x

cp try.py input.py

~/astral/ruff/target/debug/ruff check input.py --no-cache --select FA100 --select TC \
        --config lint.flake8-future-annotations.aggressive=true \
        --config lint.flake8-type-checking.strict=true --fix --unsafe-fixes

diff try.py input.py
```

Output:

```diff
Found 6 errors (6 fixed, 0 remaining).
1c1
< import collections
---
> from __future__ import annotations
3d2
< import pandas
5c4,9
< from . import local_module
---
> from typing import TYPE_CHECKING
> 
> if TYPE_CHECKING:
>     from . import local_module
>     import pandas
>     import collections
```

</details>


---

_Comment by @github-actions[bot] on 2025-06-24 16:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L58'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:58:26:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L58'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:58:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L59'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:59:26:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L59'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:59:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L69'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:69:10:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L69'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:69:10:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L134'>providers/standard/tests/unit/standard/decorators/test_python.py:134:21:</a> FA100 Add `from __future__ import annotations` to simplify `Union`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L134'>providers/standard/tests/unit/standard/decorators/test_python.py:134:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L964'>providers/standard/tests/unit/standard/decorators/test_python.py:964:23:</a> FA100 Add `from __future__ import annotations` to simplify `Union`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L964'>providers/standard/tests/unit/standard/decorators/test_python.py:964:23:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L58'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:58:26:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L58'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:58:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L59'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:59:26:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L59'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:59:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L69'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:69:10:</a> FA100 Add `from __future__ import annotations` to simplify `Optional`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L69'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:69:10:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L134'>providers/standard/tests/unit/standard/decorators/test_python.py:134:21:</a> FA100 Add `from __future__ import annotations` to simplify `Union`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L134'>providers/standard/tests/unit/standard/decorators/test_python.py:134:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
+ <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L964'>providers/standard/tests/unit/standard/decorators/test_python.py:964:23:</a> FA100 Add `from __future__ import annotations` to simplify `Union`
- <a href='https://github.com/apache/airflow/blob/f0c329641c49823f90e3b3cc51936b690c81917f/providers/standard/tests/unit/standard/decorators/test_python.py#L964'>providers/standard/tests/unit/standard/decorators/test_python.py:964:23:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>




---

_Label `configuration` added by @ntBre on 2025-06-24 16:46_

---

_Label `rule` added by @ntBre on 2025-06-24 16:46_

---

_Marked ready for review by @ntBre on 2025-06-24 17:57_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-24 17:57_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-24 17:57_

---

_Comment by @MichaReiser on 2025-06-25 08:59_

> I think this should be behind a configuration option, as the TC rules are very opinionated: not all users will want FA100 to also apply to these situations. Alternatively, we could add a new FA* rule that sits alongside FA100 and flags these situations -- https://github.com/astral-sh/ruff/issues/13273#issuecomment-2336550747 argues in favour of doing that instead.

Can you tell me more why you opted for a configuration option over a new rule?

Edit: Maybe something to answer on the issue. I think there are at least 3 possible designs and I find it hard to think through if the approach chosen here is the most ideal without looking at the problem holsiticly: Some rules change their behavior based on a future import.

---

_@Daverball reviewed on 2025-06-25 09:42_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:76 on 2025-06-25 09:42_

You might want to check whether there is a future import, if there is, the answer here is definitely `No` and not `Maybe`. The same goes for if the target Version is 3.14 or above.

I also don't quite recall how Ruff treats PEP 695 type parameter bounds or PEP 696 type parameter defaults. They probably could relatively safely be treated as typing only references, but I'm not sure that's currently how they are treated. A similar, although weaker, argument could be made for `type` statements. See #16981 for further discussions.

Treating them as `Maybe` seems correct on the surface, but `Maybe` here seems to mean that the decisions will (or at least likely will) change based on a future annotation or the target python version, which is not the case for type statements or type parameter bounds/defaults. Maybe a different name would be more appropriate? Do we want a fourth value?

Also #14787 is relevant to some degree here as well. I'm also a little worried about the interaction with the `quote_annotations` setting, since quoting annotations is antithetical to adding a future import.

---

_@ntBre reviewed on 2025-06-25 12:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:76 on 2025-06-25 12:48_

Thanks for taking a look!

> You might want to check whether there is a future import, if there is, the answer here is definitely `No` and not `Maybe`. The same goes for if the target Version is 3.14 or above.

I think if there's already a future import or the version is 3.14+ we will have already returned `Yes` a few lines above, right? That's what I was thinking while writing this at least. This logic does feel tricky, so I could certainly be wrong here.

> I also don't quite recall how Ruff treats PEP 695 type parameter bounds or PEP 696 type parameter defaults. They probably could relatively safely be treated as typing only references, but I'm not sure that's currently how they are treated. A similar, although weaker, argument could be made for `type` statements. See #16981 for further discussions.
> 
> Treating them as `Maybe` seems correct on the surface, but `Maybe` here seems to mean that the decisions will (or at least likely will) change based on a future annotation or the target python version, which is not the case for type statements or type parameter bounds/defaults. Maybe a different name would be more appropriate? Do we want a fourth value?

Hmm, I think I see what you mean. This might just not be the best place to perform this check. The integration with some other [rules](https://github.com/astral-sh/ruff/blob/1f241b4cabe5b84d4c99b827bb088815d8d072d1/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L34) looked more repetitive, but if we just re-checked the condition that was here originally (or modified slightly to fit our needs), we could avoid trying to overload the yes/no/maybe meaning.

> Also #14787 is relevant to some degree here as well. I'm also a little worried about the interaction with the `quote_annotations` setting, since quoting annotations is antithetical to adding a future import.

Related to my first point, I _think_ we will have already returned `Yes` if we're in a string annotation, but, again, I could be wrong. It sounds like I should add more test cases anyway to be sure.



---

_@Daverball reviewed on 2025-06-25 13:47_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:76 on 2025-06-25 13:47_

> I think if there's already a future import or the version is 3.14+ we will have already returned Yes a few lines above, right? That's what I was thinking while writing this at least. This logic does feel tricky, so I could certainly be wrong here.

You're probably correct for annotations, for some reason I thought `in_typing_only_annotation()` was less reliable than it actually is. But that's probably only because I personally would like it to apply to type statements and type parameters as well, but currently it does not.

---

_@Daverball reviewed on 2025-06-25 13:54_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:76 on 2025-06-25 13:54_

But either way, this function gets called for a lot more type definitions than annotations, so `Maybe` is too generous for generic subscripts, type aliases, casts, type parameters bounds/defaults. So you should definitely make sure the reference stems from an annotation (i.e. `in_runtime_evaluated_annotation()`  for `Maybe` rather than `!in_typing_only_annotation()`, it's worth noting here that runtime required annotations also can't be re-written with modern typing syntax).

---

_Comment by @ntBre on 2025-06-25 16:13_

(no changes, just rebasing onto the autofix)

---

_Comment by @ntBre on 2025-06-25 22:33_

I'll put this back into draft while we discuss the design on the issue.

---

_Converted to draft by @ntBre on 2025-06-25 22:34_

---

_Closed by @ntBre on 2025-07-24 13:32_

---

_Branch deleted on 2025-07-24 13:32_

---
