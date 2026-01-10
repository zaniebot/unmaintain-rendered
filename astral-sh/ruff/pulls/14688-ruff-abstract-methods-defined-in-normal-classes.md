```yaml
number: 14688
title: "[`ruff`] Abstract methods defined in normal classes (`RUF044`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
base: main
head: RUF044
created_at: 2024-11-30T07:29:40Z
updated_at: 2025-09-08T14:26:02Z
url: https://github.com/astral-sh/ruff/pull/14688
synced_at: 2026-01-10T17:46:21Z
```

# [`ruff`] Abstract methods defined in normal classes (`RUF044`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-30 07:29_

## Summary

Resolves #12861.

This change introduces `RUF044` along with a new shared logic for detecting whether a given class is abstract or not. The details on how that works can be found in the doc comments.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2024-11-30 09:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+102 -0 violations, +0 -0 fixes in 12 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/featurizers/tracker_featurizers.py#L253'>rasa/core/featurizers/tracker_featurizers.py:253:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/policy.py#L333'>rasa/core/policies/policy.py:333:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/policy.py#L354'>rasa/core/policies/policy.py:354:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+26 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/airflow-core/src/airflow/api_fastapi/auth/managers/models/base_user.py#L27'>airflow-core/src/airflow/api_fastapi/auth/managers/models/base_user.py:27:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/airflow-core/src/airflow/api_fastapi/auth/managers/models/base_user.py#L30'>airflow-core/src/airflow/api_fastapi/auth/managers/models/base_user.py:30:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/airflow-core/src/airflow/utils/log/logging_mixin.py#L137'>airflow-core/src/airflow/utils/log/logging_mixin.py:137:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/airflow-core/src/airflow/utils/log/logging_mixin.py#L141'>airflow-core/src/airflow/utils/log/logging_mixin.py:141:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/airflow-core/src/airflow/utils/log/logging_mixin.py#L146'>airflow-core/src/airflow/utils/log/logging_mixin.py:146:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L331'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:331:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L335'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:335:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/providers/amazon/src/airflow/providers/amazon/aws/sensors/bedrock.py#L88'>providers/amazon/src/airflow/providers/amazon/aws/sensors/bedrock.py:88:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/providers/amazon/src/airflow/providers/amazon/aws/sensors/comprehend.py#L80'>providers/amazon/src/airflow/providers/amazon/aws/sensors/comprehend.py:80:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/providers/amazon/src/airflow/providers/amazon/aws/sensors/eks.py#L110'>providers/amazon/src/airflow/providers/amazon/aws/sensors/eks.py:110:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/providers/amazon/src/airflow/providers/amazon/aws/sensors/eks.py#L113'>providers/amazon/src/airflow/providers/amazon/aws/sensors/eks.py:113:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/airflow/blob/96ebf2909f15c0bf45476c055c23d5e02ff162be/providers/amazon/src/airflow/providers/amazon/aws/triggers/base.py#L141'>providers/amazon/src/airflow/providers/amazon/aws/triggers/base.py:141:9:</a> RUF044 Abstract method defined in non-abstract class
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/8013b32f0e37dd037f4f66eb5f6916e69f9b24a6/superset/commands/database/uploaders/base.py#L81'>superset/commands/database/uploaders/base.py:81:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/apache/superset/blob/8013b32f0e37dd037f4f66eb5f6916e69f9b24a6/superset/commands/database/uploaders/base.py#L84'>superset/commands/database/uploaders/base.py:84:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L216'>src/bokeh/command/subcommands/file_output.py:216:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/backends/sql/__init__.py#L183'>ibis/backends/sql/__init__.py:183:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L123'>ibis/common/collections.py:123:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L31'>ibis/common/collections.py:31:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L39'>ibis/common/collections.py:39:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L47'>ibis/common/collections.py:47:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L58'>ibis/common/collections.py:58:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L66'>ibis/common/collections.py:66:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/collections.py#L79'>ibis/common/collections.py:79:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/deferred.py#L29'>ibis/common/deferred.py:29:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/deferred.py#L45'>ibis/common/deferred.py:45:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/graph.py#L258'>ibis/common/graph.py:258:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/graph.py#L263'>ibis/common/graph.py:263:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/patterns.py#L201'>ibis/common/patterns.py:201:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/ibis-project/ibis/blob/2665f53591846c1d828fc91529ee18314bc27bae/ibis/common/patterns.py#L223'>ibis/common/patterns.py:223:9:</a> RUF044 Abstract method defined in non-abstract class
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/indexing/base.py#L519'>libs/core/langchain_core/indexing/base.py:519:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/indexing/base.py#L570'>libs/core/langchain_core/indexing/base.py:570:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/indexing/base.py#L610'>libs/core/langchain_core/indexing/base.py:610:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/output_parsers/list.py#L51'>libs/core/langchain_core/output_parsers/list.py:51:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/runnables/configurable.py#L138'>libs/core/langchain_core/runnables/configurable.py:138:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/langchain-ai/langchain/blob/635ce60a2293b45e79ec57d0a86fed92bfc5112a/libs/core/langchain_core/tools/base.py#L621'>libs/core/langchain_core/tools/base.py:621:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/45c735570d246cb809a83108e184be49f72708d4/pymilvus/client/abstract.py#L499'>pymilvus/client/abstract.py:499:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/milvus-io/pymilvus/blob/45c735570d246cb809a83108e184be49f72708d4/pymilvus/client/asynch.py#L30'>pymilvus/client/asynch.py:30:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/milvus-io/pymilvus/blob/45c735570d246cb809a83108e184be49f72708d4/pymilvus/client/asynch.py#L41'>pymilvus/client/asynch.py:41:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/milvus-io/pymilvus/blob/45c735570d246cb809a83108e184be49f72708d4/pymilvus/client/asynch.py#L49'>pymilvus/client/asynch.py:49:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/milvus-io/pymilvus/blob/45c735570d246cb809a83108e184be49f72708d4/pymilvus/client/asynch.py#L86'>pymilvus/client/asynch.py:86:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e2bd8e60f000b46bb632d9ed78939264c55629ef/pandas/io/formats/info.py#L594'>pandas/io/formats/info.py:594:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/plugins/base_plugin.py#L17'>src/poetry/plugins/base_plugin.py:17:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/plugins/plugin.py#L23'>src/poetry/plugins/plugin.py:23:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/chain/ethereum/oracles/uniswap.py#L117'>rotkehlchen/chain/ethereum/oracles/uniswap.py:117:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/chain/ethereum/oracles/uniswap.py#L125'>rotkehlchen/chain/ethereum/oracles/uniswap.py:125:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/chain/ethereum/oracles/uniswap.py#L138'>rotkehlchen/chain/ethereum/oracles/uniswap.py:138:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/chain/evm/decoding/aave/common.py#L74'>rotkehlchen/chain/evm/decoding/aave/common.py:74:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/exchanges/exchange.py#L58'>rotkehlchen/exchanges/exchange.py:58:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/rotki/rotki/blob/d04dc85d961d274b99de7012253721d96276a1fb/rotkehlchen/utils/interfaces.py#L107'>rotkehlchen/utils/interfaces.py:107:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/259ef423ad31abe165f2aaa589e13bd6558a7e71/zproject/backends.py#L2634'>zproject/backends.py:2634:9:</a> RUF044 Abstract method defined in non-abstract class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/coordinates/representation/base.py#L282'>astropy/coordinates/representation/base.py:282:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/coordinates/representation/base.py#L299'>astropy/coordinates/representation/base.py:299:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/coordinates/representation/base.py#L517'>astropy/coordinates/representation/base.py:517:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/coordinates/representation/base.py#L539'>astropy/coordinates/representation/base.py:539:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/coordinates/transformations/affine.py#L210'>astropy/coordinates/transformations/affine.py:210:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/cosmology/_src/flrw/base.py#L377'>astropy/cosmology/_src/flrw/base.py:377:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/cosmology/_src/io/builtin/model.py#L74'>astropy/cosmology/_src/io/builtin/model.py:74:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/cosmology/_src/io/builtin/model.py#L81'>astropy/cosmology/_src/io/builtin/model.py:81:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/cosmology/_src/tests/flrw/test_base.py#L267'>astropy/cosmology/_src/tests/flrw/test_base.py:267:9:</a> RUF044 Abstract method defined in non-abstract class
+ <a href='https://github.com/astropy/astropy/blob/33adcd67e88e489c92fac0afb45d0a2b0035f3f9/astropy/cosmology/_src/tests/flrw/test_base.py#L72'>astropy/cosmology/_src/tests/flrw/test_base.py:72:9:</a> RUF044 Abstract method defined in non-abstract class
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF044 | 102 | 102 | 0 | 0 | 0 |

</p>
</details>




---

_@sbrugman reviewed on 2024-11-30 15:46_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:14 on 2024-11-30 15:46_

Could we expand a bit more here with an example so the reader understands what will happen when the class is not an abstract class? 

The abstract method decorators prevent users from instantiating abstract classes, or inheriting from abstract classes without implementing all abstract methods by throwing an exception:
> TypeError: Can't instantiate abstract class [class] with abstract method [method]

For classes that inherit from multiple classes, for example a single base class and a mixin, it's _not_ enough that `abc.ABC` is the meta class for only one of them to achieve the behaviour above., even though `abc.ABC` will be part of the MRO of the subclass.

Example code:

```python
from abc import ABC, abstractmethod


class Base(ABC):
    @abstractmethod
    def hello(self) -> None:
        ...

    def __repr__(self) -> str:
        return f"message='{self.msg}'"


class Mixin: # should be: `Mixin(ABC)`:
    @abstractmethod
    def world(self) -> None:
        self.msg += " goodbye"


class FooBar(Mixin, Base):
    def __init__(self):
        self.msg = ""

    def hello(self) -> None:
        self.msg += "hello"

    # without `Mixin(ABC)`, omitting this does not raise an exception
    # def world(self) -> None:
    #     self.msg += " world"


# `abc.ABC` is part of the MRO
print(FooBar.mro()) # [<class '__main__.FooBar'>, <class '__main__.Mixin'>, <class '__main__.Base'>, <class 'abc.ABC'>, <class 'object'>] 

fb = FooBar()
fb.hello()
fb.world()
print(str(fb))  # message='hello goodbye'
```


---

_@InSyncWithFoo reviewed on 2024-12-01 19:09_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:14 on 2024-12-01 19:09_

Thanks for the suggestion! Explanation and example added.

---

_Label `rule` added by @dhruvmanila on 2024-12-02 05:15_

---

_Label `preview` added by @dhruvmanila on 2024-12-02 05:15_

---

_@dhruvmanila reviewed on 2024-12-02 05:20_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-02 05:20_

Should we also check the base classes that are in the same file? I'm fine with moving forward but this seems like something that's doable. I'm specifically referring to point (1) that's mentioned https://github.com/astral-sh/ruff/issues/12861#issuecomment-2288460681:

> We need to recognise as valid any class that has metaclass=X, or any class that inherits from a class that has metaclass=X, where X is either abc.ABCMeta or a subclass of abc.ABCMeta.

If any of the base class is _not_ in the same module, then we can short-circuit and avoid raising the diagnostic to address point (2).

---

_@dhruvmanila reviewed on 2024-12-02 05:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/visibility.rs`:87 on 2024-12-02 05:27_

What's the reason for returning the `Decorator` here as I see that it's not being used? We can maybe just use `Option<AbstractDecoratorKind>` and add documentation like "Returns the abstract decorator kind for the function definition if it's an abstract method...".

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/visibility.rs`:84 on 2024-12-02 05:28_

nit: `abstract_decorator_kind` if you choose to update the return type

---

_@dhruvmanila reviewed on 2024-12-02 05:29_

---

_@InSyncWithFoo reviewed on 2024-12-02 08:55_

---

_Review comment by @InSyncWithFoo on `crates/ruff_python_semantic/src/analyze/visibility.rs`:87 on 2024-12-02 08:55_

Removed. I originally intended to use the precise expression in the fix title, but eventually decided to use the qualified name instead for simplicity.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-02 10:33_

Let's save that for a follow-up PR.

---

_@InSyncWithFoo reviewed on 2024-12-02 10:33_

---

_@MichaReiser reviewed on 2024-12-03 10:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-03 10:08_

I'm not against doing this as a future extension for as long as it doesn't result in the rule having too many false negatives. I'm already somewhat concerned that the rule has too many false negatives because of Ruff's lack for multifile analysis. 

---

_@InSyncWithFoo reviewed on 2024-12-03 19:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-03 19:34_

@MichaReiser I'm not sure if I follow. Should I extend the rule to also check classes with bases, or can this PR be merged on its own merit?

---

_@MichaReiser reviewed on 2024-12-04 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-04 07:59_

What I'm saying is that it depends. I'm worried that the rule has too many false positives to be useful because it only catches a small subset of violations. Supporting classes with base classes in the same file could help to reduce the number of false positives. 

I'm not sure how much effort it is but it would be nice to see how many additional violation the rule catches in the ecosystem check if it had support for base classes in the same file. Do you think it would be possible to *hack* something together just to get that data? It doesn't have to be a 100% correct.

Looking at it from a different angle, an alternative is to keep it as is and document it as a known limitation that the rule only applies to classes without base classes. The benefit of this is that it's a very simple limitation that's easy understood



---

_@InSyncWithFoo reviewed on 2024-12-15 21:39_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:116 on 2024-12-15 21:39_

(Can't believe I missed this for two weeks.) The new logic adds quite some complexity, yet it only catches [one extra violation](https://github.com/milvus-io/pymilvus/blob/712e9b6bc407f6c1c434ee2208e77ace25169302/pymilvus/client/asynch.py#L85). I'm in favor of keeping it this way, regardless.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:148 on 2024-12-16 12:08_

Do I understand this code correctly that it returns `false` for base-classes imported from other files?

---

_@MichaReiser reviewed on 2024-12-16 12:08_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:148 on 2024-12-16 12:10_

@MichaReiser No, it returns `true`, because `false` means "this base class is definitely not abstract".

---

_@InSyncWithFoo reviewed on 2024-12-16 12:10_

---

_@MichaReiser reviewed on 2024-12-18 08:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/visibility.rs`:37 on 2024-12-18 08:08_

Nit: I suggest implementing `from(name: &QualifiedName)` because the method returns an `Option` anyway and the decorator is only "unique" if it's also the right module

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/visibility.rs`:46 on 2024-12-18 08:08_

```suggestion
            Self::AbstractMethod => "abc.abstractmethod",
            Self::AbstractClassMethod => "abc.abstractclassmethod",
            Self::AbstractStaticMethod => "abc.abstractstaticmethod",
            Self::AbstractProperty => "abc.abstractproperty",
```

---

_@MichaReiser reviewed on 2024-12-18 08:08_

---

_@MichaReiser reviewed on 2024-12-18 08:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:156 on 2024-12-18 08:29_

Could you tell me more about why the code here isn't using `is_metaclass`?

---

_Comment by @InSyncWithFoo on 2024-12-18 16:57_

@MichaReiser The function <em>cannot</em> be called `is_metaclass_abcmeta` because we do not have access to enough information to determine if that is the case. We can only tell if a class "might be" or "is definitely not" `abc.ABCMeta`. Same for `is_abstract_class`. I'm sure I have explained this somewhat in the doc comments. See also [`typing::might_be_generic`](https://github.com/astral-sh/ruff/blob/f0012df68653835752c32d47ebba06320c7e48cf/crates/ruff_python_semantic/src/analyze/class.rs#L175-L184).

---

_Comment by @MichaReiser on 2024-12-18 17:26_

@InSyncWithFoo what's an example where `is_metaclass_abcmeta` or `is_abstract_class` returns `true` where the class isn't a `abcmeta` or an abstract class? 

if there are indeed cases where some of those methods return false when the class isn't abstract, then it's important that we document these as part of the rule.

---

_Comment by @InSyncWithFoo on 2024-12-18 18:48_

A few examples:

```python
from abc import ABCMeta
from somewhere import SomeMetaclass  # Could be subclass of ABCMeta, for all we know.

# `B` might be abstract (methods are not checked).
class A(ABCMeta): ...
class B(metaclass=A): ...

# Is `C` abstract or not?
class C(metaclass=SomeMetaclass): ...

# `D` and `E` definitely aren't abstract.
class D: ...
class E(D): ...
```

---

_Comment by @MichaReiser on 2024-12-19 12:30_

Thanks for the example. I feel like we can keep using `is_metacalss_abcmeta` and add a documentation that clarifies that:

* It only returns `true` for classes that are guaranteed to be meta classes
* It may return `false` for classes that are meta classes but Ruff can't detect



---

_@InSyncWithFoo reviewed on 2024-12-19 17:45_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/abstract_method_in_normal_class.rs`:156 on 2024-12-19 17:45_

`is_metaclass` checks if the given class inherits from `type`, `ABCMeta`, or `EnumMeta`/`EnumType`, while this checks if the metaclass does <em>not</em> inherit from `ABCMeta`.

---

_Comment by @InSyncWithFoo on 2024-12-19 17:52_

Here's a table:

| Classes                           | Is abstract? | `might_be_abstract()` |
|-----------------------------------|--------------|-----------------------|
| `class A(ABC)`                    | Yes          | `true`                |
| `class B(A)`, `class A(ABC)`      | Yes          | `true`                |
| `class A(metaclass=ABCMeta)`      | Yes          | `true`                |
| `class A(Unknown)`                | Maybe        | `true`                |
| `class A(metaclass=Unknown)`      | Maybe        | `true`                |
| `class A`                         | No           | `false`               |
| `class B(A)`, `class A`           | No           | `false`               |
| `class B(metaclass=A)`, `class A` | No           | `false`               |

`might_be_abstract()` returns `false` when it is possible to determine that the given class is <em>not</em> abstract. It returns `true` otherwise (when the class is known to be abstract, or when there are not enough information). Same for `metaclass_might_be_abcmeta()`.

---

_Comment by @MichaReiser on 2024-12-20 08:07_

Thanks for the table. I see. I'm then leaning towards simply calling it `is_not_abstract`. 

I also noticed that we have `B024` which checks the opposite of this rule. We should make sure that the two rules are consistent in how they detect abstract classes. 

https://github.com/astral-sh/ruff/blob/14ba469fc099e0678859bba3ae6f67477d8934ec/crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs#L116-L129

---

_Comment by @InSyncWithFoo on 2024-12-20 23:06_

@MichaReiser `is_not_abstract` has the "double negative" problem:

```rust
// This doesn't read well in English: "If *not* `class` is *not* abstract, bail."
if !is_not_abstract(class, checker.semantic()) {
    return;
}

// This reads better: "If `class` might be abstract, bail."
if might_be_abstract(class, checker.semantic()) {
    return;
}
```

As for `B024` (and [`B027`](https://docs.astral.sh/ruff/rules/empty-method-without-abstract-decorator/)), I can submit another PR, but I don't think that alone should block this PR, especially when the existing logic was taken dirrectly from [upstream](https://github.com/PyCQA/flake8-bugbear/blob/3a140377c8f1f585013a1566f2c8bb3ead9c329c/bugbear.py#L1007).


---

_Comment by @MichaReiser on 2024-12-21 15:18_

> @MichaReiser is_not_abstract has the "double negative" problem:

That's fair. My only concern is that `might` is a bit hand-wavey. It's unclear if it classifies classes as abstract that aren't and/or classes as not abstract that are. The name suggests to me that it could have false positives as well as false negatives. The `is_not_abstract` is more explicit about it. The name suggests that it never returns `true` for a class that might be abstract. We can also try to address this in another way. For example by returning an enum with variant names, although I'm not sure how to name the variants to make it clear.

>As for B024 (and [B027](https://docs.astral.sh/ruff/rules/empty-method-without-abstract-decorator/)), I can submit another PR, but I don't think that alone should block this PR, especially when the existing logic was taken dirrectly from [upstream](https://github.com/PyCQA/flake8-bugbear/blob/3a140377c8f1f585013a1566f2c8bb3ead9c329c/bugbear.py#L1007).

I'm not sure if we want to update `B024`, instead I think it would be better to align this rule with what's done in B024 and, if we can, avoid any unnecessary code duplication. 



---

_Comment by @InSyncWithFoo on 2024-12-21 22:21_

@MichaReiser

* `B024` does not check for subclasses of `ABCMeta`; `RUF044` currently does.
* `B024` only reports when the class is definitely known to be abstract; `RUF044` currently only reports when the class is known to <em>not</em> be abstract.
* `B024` wants to avoid classes that do not inherit directly from `ABC` or has `metaclass=ABCMeta`, as such a class might not be abstract.
* `RUF044` wants to avoid classes that have an `ABC` base or an `ABCMeta` metaclass, as such a class might be abstract.

Visualization:

```text
         `is_abc_class() == true`
                    |             `might_be_abstract() == true`
                    /-------------------------------------------------------\
Abstractness:   Abstract        Likely abstract        Unknown      Likely not abstract    Not abstract
                    |------------------|------------------|------------------|------------------|
Requirements:  B024, B027                                                                     RUF044
```

<strong>Are you sure</strong> that you want `RUF044` to use existing logic from `B024` when the requirements are so significantly different?

---

_Comment by @MichaReiser on 2024-12-30 16:20_

To me the two rules must be consistent in their behavior. That means they should both respect `ABCMeta` or not. For now, I suggest to ignore `ABCMeta` and to create a separate PR that adds support for `ABCMeta` to both rules. Unless there are specific reasons why `B024` doesn't support `ABCMeta` that I'm overlooking. 

Whether both rules use the exact same code is less important but it would be nice if they can share most of the: Does this class implement `ABCMeta` logic. 



---

_Comment by @InSyncWithFoo on 2024-12-30 16:48_

That `B024` does not recognize <em>subclasses</em> of `ABCMeta` is a different matter, to be addressed in another PR. I can submit that PR tomorrow if you want (reusing all the code from this PR), but I don't see why I have to worsen `RUF044`'s logic to match that, especially when it will be readded again anyway.

`B024` wants only <em>direct</em> subclasses of `ABC` or classes that have (a subclass of) `ABCMeta` as the metaclass. `RUF044` wants classes with no `ABC` or `ABCMeta` <em>anywhere</em> in their and their metaclasses' MROs.

---

_@MichaReiser requested changes on 2024-12-30 17:17_

My main concern is that this PR adds more code to identify abstract classes that are hidden away in the rule implementation and isn't shared with the logic that other rules use. I'm okay looking over the `ABCMeta` difference if we plan on addressing that later, as long as my main concern is addressed

---

_Renamed from "[`ruff`] Abstract method defined in normal class (`RUF044`)" to "[`ruff`] Abstract methods defined in normal class (`RUF044`)" by @InSyncWithFoo on 2025-02-15 00:01_

---

_Renamed from "[`ruff`] Abstract methods defined in normal class (`RUF044`)" to "[`ruff`] Abstract methods defined in normal classes (`RUF044`)" by @InSyncWithFoo on 2025-02-15 00:01_

---

_Comment by @InSyncWithFoo on 2025-02-15 00:27_

This is ready for a re-review now.

---

_Comment by @liammcdevitt73 on 2025-05-23 16:05_

Did this ever get implemented "Abstract methods defined in normal classes (RUF044)" or are there plans to? https://docs.astral.sh/ruff/rules/#ruff-specific-rules-ruf

---

_Comment by @amaslenn on 2025-09-05 09:46_

@InSyncWithFoo what is the state of this feature? I find it useful and ready to help if needed.

---

_Comment by @InSyncWithFoo on 2025-09-05 20:10_

@amaslenn This PR is outdated due to not having a reviewer. I'm willing to update it as long as there's one.

---

_Comment by @amaslenn on 2025-09-08 14:26_

> @amaslenn This PR is outdated due to not having a reviewer. I'm willing to update it as long as there's one.

@MichaReiser could you please review this PR?

---
