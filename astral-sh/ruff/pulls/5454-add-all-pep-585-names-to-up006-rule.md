```yaml
number: 5454
title: Add all PEP-585 names to UP006 rule
type: pull_request
state: merged
author: wookie184
labels:
  - rule
assignees: []
merged: true
base: main
head: more-UP006-detection
created_at: 2023-06-30T13:15:14Z
updated_at: 2025-01-04T01:03:41Z
url: https://github.com/astral-sh/ruff/pull/5454
synced_at: 2026-01-12T15:55:18Z
```

# Add all PEP-585 names to UP006 rule

---

_@wookie184_

Closes #3936 

## Summary

Building on https://github.com/astral-sh/ruff/pull/4420, adds the remaining PEP-585 names.

## Test Plan

Added snapshot tests for a few cases.



---

_@wookie184 reviewed on 2023-06-30 13:17_

---

_Review comment by @wookie184 on `crates/ruff_python_semantic/src/model.rs`:605 on 2023-06-30 13:17_

To add some context for this change:

With this code (`test.py`)
```python
import typing
from collections.abc import Sequence

def f(x: typing.Sequence) -> None:
    ...
```

Running `cargo run --bin ruff -- --no-cache --target-version py311 --select UP test.py`

Without this change:
```
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Sequence` into scope due to name conflict
test.py:4:10: UP006 Use `collections.abc.Sequence` instead of `typing.Sequence` for type annotation
Found 1 error.
```

With this change:
```
test.py:4:10: UP006 [*] Use `collections.abc.Sequence` instead of `typing.Sequence` for type annotation
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

(and fix is valid)

---

_Comment by @charliermarsh on 2023-06-30 14:28_

Hmm, I believe these are already covered by the deprecated import rule that we have in pyupgrade. There’s clearly some overlap in responsibilities between the two.

---

_Comment by @wookie184 on 2023-06-30 15:33_

> Hmm, I believe these are already covered by the deprecated import rule that we have in pyupgrade. There’s clearly some overlap in responsibilities between the two.

Currently, `UP035` detects imports of many deprecated names, while `UP006` detects usage of just PEP 585 deprecated names. So e.g.
```python
from typing import List  # Flagged by UP035
x: List  # Flagged by UP006

import typing
x: typing.List  # Flagged by UP006
```

It definitely seems like something that could be merged into one rule, or we could have two (one for pep 585 and one for other deprecated names) which share their implementation.

-------

Also made me realise an issue with this PR currently:
```python
import typing
from typing import Sequence

x: Sequence
x: typing.Sequence
```
causes an error
```
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Sequence` into scope due to name conflict
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Sequence` into scope due to name conflict
test.py:2:1: UP035 [*] Import from `collections.abc` instead: `Sequence`
test.py:4:4: UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
test.py:5:4: UP006 Use `collections.abc.Sequence` instead of `typing.Sequence` for type annotation
Found 3 errors.
[*] 1 potentially fixable with the --fix option.
```
although weirdly when I actually run `--fix` it runs **2** fixes (including `UP006`) and does actually fix the code properly... I guess ruff reruns fixes until the output is stable?

---

_Comment by @charliermarsh on 2023-07-01 02:44_

I think this originally arose from pyupgrade, which has two separate "rules" for deprecated imports vs. PEP 585 rewrites (for builtins only). Most deprecated imports can be fixed by only modifying the import site, since they typically involve members that were moved from one module to another. Meanwhile, the PEP 585 built-in rewrites don't require changing imports at all, since they only change to `list`, `set`, etc., which are available as builtins. Later, when we added support for multi-location fixes, we added `defaultdict` and `deque` to the PEP 585 rule; and we added `typing.List` and friends to the deprecated import rule, even though it was already covered in a sense by the PEP 585 rule. Anyway, just explaining why this happened, not how we should deal with it.

The current setup honestly seems okay to me (the PEP 585 rule deals with members that need to be renamed; the deprecated imports rule only fixes _imports_), but I'd probably also be okay with removing the PEP 585 imports from "deprecated imports" and moving them into this rule. Though it might be sort of complicated to pull off given how different the implementations are, and some of the quirks around how they'd need to be gated (e.g., you can rewrite `List` to `list` if `from __future__ import annotations` is present, but you can't rewrite to a `collections.abc` import), at which point, I may question the value.

> although weirdly when I actually run --fix it runs 2 fixes (including UP006) and does actually fix the code properly... I guess ruff reruns fixes until the output is stable?

Yeah, that's right -- we iteratively fix up to a certain number of iterations.



---

_Comment by @wookie184 on 2023-07-07 18:50_

Thanks for explaining, I agree that it seems quite complicated to combine the rules, and removing the duplication of names would probably also require a decent amount of work. Maybe something that could be done at some point but I'm not sure it's necessary for this PR.

I fixed the issue with the name conflict when both types of import are already there (the autofix for this case still works, it just needs the `UP035` autofix first).

Does this look alright?

---

_@wookie184 reviewed on 2023-08-06 10:02_

---

_Review comment by @wookie184 on `crates/ruff_python_semantic/src/model.rs`:605 on 2023-08-06 10:02_

This seems to now have been fixed by this PR already: https://github.com/astral-sh/ruff/pull/6102

---

_Comment by @flying-sheep on 2023-08-25 09:47_

Ah, this is great! It would be great to have this automated!

---

_Comment by @zanieb on 2023-10-19 16:56_

@charliermarsh I think this is probably reasonable as discussed?

---

_Comment by @charliermarsh on 2023-10-21 23:10_

I feel like this could be better suited to inclusion in UP035 -- since the deprecated imports flagged in there should be a superset of the PEP 585 changes.

---

_Comment by @charliermarsh on 2023-10-21 23:10_

As an example: if someone does `import pipes; pipes.quote`, we probably want to rewrite that to `import shlex; shlex.quote`. But that's unrelated to PEP 585.

---

_Comment by @wookie184 on 2023-10-23 10:54_

> I feel like this could be better suited to inclusion in UP035 -- since the deprecated imports flagged in there should be a superset of the PEP 585 changes.

I can't think of a case that you'd want to have a rule to detect/fix
```python
from typing import List
x: List
```
but not
```
import typing
x: typing.List
```
I would expect them to be treated as equivalent by a rule attempting to update to PEP 585 annotations.

> As an example: if someone does import pipes; pipes.quote, we probably want to rewrite that to import shlex; shlex.quote. But that's unrelated to PEP 585.

I think that just suggests that would make sense to implement for `UP006` (ideally sharing implementation with `UP035`). IMO, the difference between `UP006` and `UP035` should just be what names are affected, not in what cases the names are changed, since that's not a helpful distinction for the user.


---

_Review requested from @charliermarsh by @charliermarsh on 2023-12-01 01:56_

---

_Label `rule` added by @charliermarsh on 2023-12-01 01:56_

---

_Comment by @github-actions[bot] on 2023-12-01 02:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/e29740f4022cb0bc29140a05f6f2cfb900b8b581/src/pdm/cli/commands/cache.py#L37'>src/pdm/cli/commands/cache.py:37:47:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
+ <a href='https://github.com/pdm-project/pdm/blob/e29740f4022cb0bc29140a05f6f2cfb900b8b581/src/pdm/cli/commands/cache.py#L97'>src/pdm/cli/commands/cache.py:97:20:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
+ <a href='https://github.com/pdm-project/pdm/blob/e29740f4022cb0bc29140a05f6f2cfb900b8b581/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:36:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
+ <a href='https://github.com/pdm-project/pdm/blob/e29740f4022cb0bc29140a05f6f2cfb900b8b581/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:67:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2433 -0 violations, +0 -0 fixes in 22 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+409 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L104'>aiven/client/argx.py:104:38:</a> UP006 Use `collections.abc.Iterable` instead of `Iterable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L155'>aiven/client/argx.py:155:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L171'>aiven/client/argx.py:171:45:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L174'>aiven/client/argx.py:174:49:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L241'>aiven/client/argx.py:241:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:34:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:44:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L290'>aiven/client/argx.py:290:32:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L300'>aiven/client/argx.py:300:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L303'>aiven/client/argx.py:303:34:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
... 399 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+481 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api/auth/backend/deny_all.py#L34'>airflow/api/auth/backend/deny_all.py:34:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/parameters.py#L87'>airflow/api_connexion/parameters.py:87:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:52:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:78:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L114'>airflow/api_connexion/security.py:114:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L161'>airflow/api_connexion/security.py:161:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L180'>airflow/api_connexion/security.py:180:53:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L199'>airflow/api_connexion/security.py:199:57:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L218'>airflow/api_connexion/security.py:218:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/api_connexion/security.py#L237'>airflow/api_connexion/security.py:237:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 471 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+167 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/scripts/check-env.py#L37'>scripts/check-env.py:37:40:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/advanced_data_type/types.py#L58'>superset/advanced_data_type/types.py:58:21:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/advanced_data_type/types.py#L59'>superset/advanced_data_type/types.py:59:23:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/cli/test_db.py#L81'>superset/cli/test_db.py:81:12:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/cli/test_db.py#L88'>superset/cli/test_db.py:88:38:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/commands/chart/export.py#L80'>superset/commands/chart/export.py:80:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/commands/dashboard/export.py#L168'>superset/commands/dashboard/export.py:168:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/commands/database/export.py#L108'>superset/commands/database/export.py:108:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/commands/dataset/export.py#L87'>superset/commands/dataset/export.py:87:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/640dac1eff50229b5b945791d1e99488c707be91/superset/commands/dataset/importers/v0.py#L129'>superset/commands/dataset/importers/v0.py:129:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 157 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+250 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:52:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L35'>release/action.py:35:47:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L144'>release/build.py:144:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L37'>release/credentials.py:37:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L40'>release/credentials.py:40:38:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L24'>release/pipeline.py:24:12:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L34'>release/pipeline.py:34:31:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L103'>release/ui.py:103:33:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L24'>release/ui.py:24:17:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 240 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/33d6784e64b42e0dc47e15282979925a9bc60333/ibis/common/tests/test_patterns.py#L1122'>ibis/common/tests/test_patterns.py:1122:13:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/33d6784e64b42e0dc47e15282979925a9bc60333/ibis/common/tests/test_patterns.py#L1125'>ibis/common/tests/test_patterns.py:1125:10:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/33d6784e64b42e0dc47e15282979925a9bc60333/ibis/common/tests/test_patterns.py#L731'>ibis/common/tests/test_patterns.py:731:31:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+299 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/beta_decorator.py#L128'>libs/core/langchain_core/_api/beta_decorator.py:128:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/beta_decorator.py#L186'>libs/core/langchain_core/_api/beta_decorator.py:186:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/beta_decorator.py#L201'>libs/core/langchain_core/_api/beta_decorator.py:201:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/beta_decorator.py#L30'>libs/core/langchain_core/_api/beta_decorator.py:30:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/beta_decorator.py#L39'>libs/core/langchain_core/_api/beta_decorator.py:39:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/deprecation.py#L202'>libs/core/langchain_core/_api/deprecation.py:202:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/deprecation.py#L232'>libs/core/langchain_core/_api/deprecation.py:232:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/deprecation.py#L252'>libs/core/langchain_core/_api/deprecation.py:252:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/deprecation.py#L268'>libs/core/langchain_core/_api/deprecation.py:268:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/edbe7d5f5e0dcc771c1f53a49bb784a3960ce448/libs/core/langchain_core/_api/deprecation.py#L300'>libs/core/langchain_core/_api/deprecation.py:300:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 289 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+47 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L342'>lnbits/app.py:342:46:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L352'>lnbits/app.py:352:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L332'>lnbits/core/models.py:332:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L333'>lnbits/core/models.py:333:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/auth_api.py#L340'>lnbits/core/views/auth_api.py:340:49:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/lnurl.py#L24'>lnbits/lnurl.py:24:36:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L143'>lnbits/tasks.py:143:11:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L143'>lnbits/tasks.py:143:31:</a> UP006 Use `collections.abc.Coroutine` instead of `Coroutine` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L144'>lnbits/tasks.py:144:19:</a> UP006 Use `collections.abc.Coroutine` instead of `Coroutine` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L144'>lnbits/tasks.py:144:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/4a6680c97968e9d6173f9525a19ce4ff4273b1ce/_version_helper.py#L32'>_version_helper.py:32:17:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+101 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/dev/clint/src/clint/linter.py#L110'>dev/clint/src/clint/linter.py:110:42:</a> UP006 Use `collections.abc.Iterator` instead of `Iterator` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/dev/set_matrix.py#L317'>dev/set_matrix.py:317:56:</a> UP006 Use `collections.abc.Iterator` instead of `Iterator` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/bedrock/_autolog.py#L46'>mlflow/bedrock/_autolog.py:46:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/bedrock/_autolog.py#L46'>mlflow/bedrock/_autolog.py:46:58:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/dataset_registry.py#L122'>mlflow/data/dataset_registry.py:122:48:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/dataset_registry.py#L18'>mlflow/data/dataset_registry.py:18:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/dataset_registry.py#L63'>mlflow/data/dataset_registry.py:63:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/dataset_registry.py#L93'>mlflow/data/dataset_registry.py:93:42:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/huggingface_dataset.py#L180'>mlflow/data/huggingface_dataset.py:180:37:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/mlflow/mlflow/blob/4244b05d5a6f99af59ed709efe7c34e79dec0155/mlflow/data/huggingface_dataset.py#L180'>mlflow/data/huggingface_dataset.py:180:52:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
... 91 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L120'>src/build/_builder.py:120:14:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L120'>src/build/_builder.py:120:68:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L39'>src/build/_builder.py:39:28:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L60'>src/build/_builder.py:60:44:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L74'>src/build/_builder.py:74:47:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L74'>src/build/_builder.py:74:69:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP006 | 2429 | 2429 | 0 | 0 | 0 |
| FA100 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-03-12 17:07_

@AlexWaygood @charliermarsh could you two work together to determine a path forward on this one?

---

_Assigned to @charliermarsh by @zanieb on 2024-03-12 17:07_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-03-12 18:45_

---

_Comment by @charliermarsh on 2024-07-18 22:42_

Ok, I think it actually does make sense to add these here -- but they probably need to go under preview. You're right that it's silly that the first case triggers, but the second does not:

```python
from typing import AbstractSet

def f(x: AbstractSet[str]) -> None:
    pass

import typing

def f(x: typing.AbstractSet[str]) -> None:
    pass
```

---

_Comment by @charliermarsh on 2024-07-18 22:43_

If you want to rebase and gate the new variants behind preview, I am happy to merge the change.

---

_@MichaReiser requested changes on 2024-08-01 21:42_

As of Charlie's comment

> If you want to rebase and gate the new variants behind preview, I am happy to merge the change.

---

_Review requested from @MichaReiser by @dylwil3 on 2024-12-23 22:53_

---

_@dhruvmanila reviewed on 2024-12-24 05:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_stdlib/src/typing.rs`:327 on 2024-12-24 05:21_

nit: we can split this into two matches to short-circuit when the call path is not from `typing` / `typing_extensions` module like:
```rs
let ["typing" | "typing_extensions", member @ ..] = call_path else {
    return None;
};

match member {
    // Builtins
    ["Tuple"] => Some(("", "tuple")),
    ["List"] => Some(("", "list")),
	// ...
}
```

---

_@dhruvmanila approved on 2024-12-24 05:29_

Looks good

---

_Review comment by @wookie184 on `crates/ruff_python_stdlib/src/typing.rs`:327 on 2024-12-28 19:52_

That's roughly what I did originally [`a818d91` (#5454)](https://github.com/astral-sh/ruff/pull/5454/commits/a818d91fcc6718ec8d4d6c6f5c8c9be136100669#diff-c4f7c61e24d0aea6b6ead5c0c47c17c3a6a126e63ed1bbf3c51b3cc31dee06cdR326-R328)

Not sure if the change back was intentional, or just something lost in the course of the PR...

---

_@wookie184 reviewed on 2024-12-28 19:53_

---

_@MichaReiser approved on 2024-12-30 11:21_

---

_Merged by @MichaReiser on 2024-12-30 11:21_

---

_Closed by @MichaReiser on 2024-12-30 11:21_

---

_Comment by @wookie184 on 2024-12-30 11:24_

Thanks everyone for finishing this off and getting it merged in!

---

_Comment by @oprypin on 2025-01-04 01:03_

I have raised a few issues with this change:

* https://github.com/astral-sh/ruff/issues/15246

  Even if the detection+fix ability is better now that these are implemented in `UP006`, something needs to be done about the fact that the newly added warnings are all duplicates of warnings and fixes that already exist in `UP035`.

  If this implementation becomes a little more polished, it probably should completely replace the implementation that's currently in `UP035`, there's really no reason to still keep that inferior implementation other than its battle-tested status. Although maybe for historic consistency, even if the *implementation* is unified towards `UP006`, the set of items that it triggers for should remain split between `UP006` (covering only builtins such as `list`) and `UP035` (covering others) like it's always been.

* https://github.com/astral-sh/ruff/issues/15245

  The fixer has bugs where it sometimes fails to kick in, and in corner cases kicks in with strange results.
  
For now I'd like to see this pull request reverted and relanded later after a more detailed discussion, because it has ongoing bugs and possibly introduces undesirable/unexpected changes to what the rules are supposed to cover.

---
