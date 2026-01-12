```yaml
number: 4831
title: Add rule to disallow implicit optional with autofix
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: feat/implicit-optional
created_at: 2023-06-03T11:27:16Z
updated_at: 2023-06-13T02:44:41Z
url: https://github.com/astral-sh/ruff/pull/4831
synced_at: 2026-01-12T03:43:29Z
```

# Add rule to disallow implicit optional with autofix

---

_Pull request opened by @dhruvmanila on 2023-06-03 11:27_

## Summary

Add rule to disallow implicit optional with autofix.

Currently, I've added it under `RUF` category.

### Limitation

Type aliases could result in false positive:

```python
from typing import Optional

StrOptional = Optional[str]


def foo(arg: StrOptional = None):
	pass
```

## Test Plan

`cargo test`

resolves: #1983 


---

_Comment by @github-actions[bot] on 2023-06-03 11:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+65, -0, 0 error(s))

<details><summary>airflow (+26, -0)</summary>
<p>

```diff
+ airflow/api_connexion/endpoints/connection_endpoint.py:118:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/api_connexion/endpoints/dag_endpoint.py:110:44: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/api_connexion/endpoints/pool_endpoint.py:86:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/api_connexion/endpoints/role_and_permission_endpoint.py:113:48: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/api_connexion/endpoints/user_endpoint.py:128:47: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/api_connexion/endpoints/variable_endpoint.py:102:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/kubernetes/pod_launcher_deprecated.py:73:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:101:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:127:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:156:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:167:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:177:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:212:50: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:227:50: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/metrics/otel_logger.py:65:47: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/models/dagbag.py:175:40: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/models/renderedtifields.py:178:30: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/models/variable.py:159:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/models/variable.py:188:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/models/variable.py:210:35: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/amazon/aws/links/emr.py:51:59: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/google/cloud/hooks/pubsub.py:176:33: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/google/cloud/operators/pubsub.py:130:33: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/google/common/utils/id_token_credentials.py:189:43: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/jenkins/operators/jenkins_job_trigger.py:102:21: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ airflow/providers/jenkins/operators/jenkins_job_trigger.py:116:58: RUF013 [*] PEP 484 prohibits implicit `Optional`
```

</p>
</details>
<details><summary>bokeh (+6, -0)</summary>
<p>

```diff
+ src/bokeh/core/property/any.py:83:33: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ src/bokeh/core/property/nullable.py:54:73: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ src/bokeh/core/property/primitive.py:62:33: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ src/bokeh/embed/standalone.py:158:52: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ src/bokeh/embed/standalone.py:300:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ src/bokeh/embed/standalone.py:371:62: RUF013 [*] PEP 484 prohibits implicit `Optional`
```

</p>
</details>
<details><summary>disnake (+30, -0)</summary>
<p>

```diff
+ disnake/app_commands.py:227:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/app_commands.py:354:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/app_commands.py:850:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/ctx_menus_core.py:177:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/ctx_menus_core.py:233:11: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/ctx_menus_core.py:312:11: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/ctx_menus_core.py:75:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/interaction_bot_base.py:487:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/interaction_bot_base.py:488:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/interaction_bot_base.py:581:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/interaction_bot_base.py:658:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/params.py:1090:11: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/params.py:1091:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/params.py:495:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/params.py:496:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:144:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:191:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:192:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:271:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:272:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:431:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:432:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:519:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:520:22: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:584:15: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:747:11: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/ext/commands/slash_core.py:748:18: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/i18n.py:118:17: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/interactions/base.py:1856:85: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ disnake/utils.py:474:65: RUF013 [*] PEP 484 prohibits implicit `Optional`
```

</p>
</details>
<details><summary>zulip (+3, -0)</summary>
<p>

```diff
+ zerver/lib/db.py:34:43: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ zerver/views/auth.py:800:39: RUF013 [*] PEP 484 prohibits implicit `Optional`
+ zerver/views/auth.py:927:42: RUF013 [*] PEP 484 prohibits implicit `Optional`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF013 | 65 | 65 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.3±0.48ms     4.4 MB/sec    1.00      9.0±0.47ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.04  1908.3±91.01µs     8.7 MB/sec    1.00  1840.3±67.06µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00   180.1±14.16µs    16.4 MB/sec    1.01   181.5±12.38µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.29ms     7.0 MB/sec    1.00      3.6±0.12ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±0.60ms     2.0 MB/sec    1.05     20.8±0.59ms  1998.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.8±0.19ms     3.4 MB/sec    1.00      4.7±0.46ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   616.5±60.22µs     4.8 MB/sec    1.00   610.5±57.02µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.25ms     3.0 MB/sec    1.01      8.4±0.29ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03      9.8±0.31ms     4.1 MB/sec    1.00      9.6±0.30ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.10ms     8.1 MB/sec    1.00  1999.0±62.32µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.02   241.0±14.46µs    12.2 MB/sec    1.00   235.2±12.31µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.17ms     5.7 MB/sec    1.00      4.4±0.16ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.24ms     5.8 MB/sec    1.18      8.2±0.19ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1425.5±56.74µs    11.7 MB/sec    1.15  1646.4±70.00µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    136.1±9.63µs    21.7 MB/sec    1.11    151.6±7.88µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.07ms     9.0 MB/sec    1.19      3.4±0.12ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.03     14.8±0.24ms     2.8 MB/sec    1.00     14.4±0.23ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.08ms     4.4 MB/sec    1.00      3.7±0.10ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   452.8±15.62µs     6.5 MB/sec    1.00   446.1±14.62µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.18ms     4.0 MB/sec    1.00      6.3±0.14ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.05      7.6±0.20ms     5.3 MB/sec    1.00      7.3±0.14ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1589.8±48.56µs    10.5 MB/sec    1.00  1550.5±43.58µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    177.7±6.58µs    16.6 MB/sec    1.00    175.0±9.30µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.08ms     7.5 MB/sec    1.00      3.3±0.11ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-06-03 13:20_

Skimming through the ecosystem checks, questions and limitations:

* Type aliases could result in false positive:
	```python
	from typing import Optional

	Some: Optional[int]


	def foo(arg: Some = None):
		pass
	```

* Should we ignore for `typing.Any` ?
	```python
	from typing import Any


	def foo(arg: Any = None):
		pass
	```

---

_Comment by @zanieb on 2023-06-03 16:21_

Aliases seem like an annoying false positive :/

> Should we ignore for `typing.Any` 

`Any` includes `None` so I would think so.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:175 on 2023-06-05 06:23_

Nit: We could add this as a method to `TypingTarget`. It could shorten the name and you have fewer (explicit) arguments.

Nit: Rename: `is_implicit_optional()` or `is_optional` since it includes `None` and `Optional`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:178 on 2023-06-05 06:25_

Nit: As a non-pythonist. I always find `elts` so hard to understand. Is it an abbreviation for `elements`? Maybe write out the name in full

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:265 on 2023-06-05 06:27_

Can we set the right autofix kind here (suggested?)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:306 on 2023-06-05 06:28_

Nit: Maybe assign the right hand side to a variable to improve readability
```suggestion
	let arguments_with_defaults = arguments
      .kwonlyargs
      .iter()
      .rev()
      .zip(arguments.kw_defaults.iter().rev())
      .chain(
          arguments
              .args
              .iter()
              .rev()
              .chain(arguments.posonlyargs.iter().rev())
              .zip(arguments.defaults.iter().rev()),
	);	
    for (arg, default) in arguments_with_defaults {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:326 on 2023-06-05 06:29_

Nit: Implement `From` on `ConversionType` instead?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:151 on 2023-06-05 06:30_

Do we have a way to resolve the `Optional` typing symbol using the semantic model to avoid the aliasing problem?

---

_@MichaReiser reviewed on 2023-06-05 06:30_

---

_@dhruvmanila reviewed on 2023-06-05 12:51_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:178 on 2023-06-05 12:51_

The main reason I was using `elts` is because the field name on the node itself is `elts` but I do think writing it out in full is better :)

---

_@dhruvmanila reviewed on 2023-06-05 13:00_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:175 on 2023-06-05 13:00_

Yeah, makes sense. I am leaning towards `is_implicit_optional` as the latter could give hint of whether the check if for an `Optional` type.

---

_@dhruvmanila reviewed on 2023-06-05 13:23_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:177 on 2023-06-05 13:23_

Do we prefer to use `Self` or explicit type name `TypingTarget`?

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:151 on 2023-06-05 13:51_

No, not as of now but I think we should look into aliases as without it we're facing other issues as well (https://github.com/charliermarsh/ruff/issues/4843#issuecomment-1575250620)

---

_@dhruvmanila reviewed on 2023-06-05 13:51_

---

_@dhruvmanila reviewed on 2023-06-05 13:52_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:265 on 2023-06-05 13:52_

Yes, I would recommend `Suggested` as we'll produce false positive for type aliases. Once that's fixed, this can be made `Automatic`.

---

_Comment by @dhruvmanila on 2023-06-05 14:43_

> `Any` includes `None` so I would think so.

Thanks, that makes sense and what `mypy` does as well :)

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-06-05 14:52_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-06-06 15:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:177 on 2023-06-09 09:12_

I only use `Self` as the return type of `::new()`

---

_@MichaReiser reviewed on 2023-06-09 09:12_

---

_@dhruvmanila reviewed on 2023-06-09 13:55_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:177 on 2023-06-09 13:55_

That makes sense. Without editor support, it might be hard to figure out what `Self` is.

---

_Comment by @charliermarsh on 2023-06-12 17:22_

(Taking a look...)

---

_Comment by @charliermarsh on 2023-06-12 18:03_

I don't think it's _too_ bad to support aliases, at least within files, I can look into it separately.

---

_Merged by @charliermarsh on 2023-06-12 18:12_

---

_Closed by @charliermarsh on 2023-06-12 18:12_

---

_Branch deleted on 2023-06-13 02:44_

---
