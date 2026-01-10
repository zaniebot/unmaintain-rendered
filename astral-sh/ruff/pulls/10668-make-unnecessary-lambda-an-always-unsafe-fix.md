```yaml
number: 10668
title: "Make `unnecessary-lambda` an always-unsafe fix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/safe
created_at: 2024-03-30T00:56:22Z
updated_at: 2024-03-30T01:20:37Z
url: https://github.com/astral-sh/ruff/pull/10668
synced_at: 2026-01-10T22:47:02Z
```

# Make `unnecessary-lambda` an always-unsafe fix

---

_Pull request opened by @charliermarsh on 2024-03-30 00:56_

## Summary

See the linked issue and comment for more.

Closes https://github.com/astral-sh/ruff/issues/10663.


---

_Label `bug` added by @charliermarsh on 2024-03-30 00:56_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-03-30 00:56_

---

_Review request for @AlexWaygood removed by @charliermarsh on 2024-03-30 00:56_

---

_Merged by @charliermarsh on 2024-03-30 01:05_

---

_Closed by @charliermarsh on 2024-03-30 01:05_

---

_Branch deleted on 2024-03-30 01:05_

---

_@AlexWaygood approved on 2024-03-30 01:05_

I guess we could mark it as safe if we inspected the two functions and determined that they had exactly the same signatures (including the argument names). But that would be a more involved change — this looks good for now!

---

_Comment by @github-actions[bot] on 2024-03-30 01:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+72 -6 violations, +0 -172 fixes in 8 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/client.py#L1280'>disnake/client.py:1280:52:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/client.py#L1280'>disnake/client.py:1280:52:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/client.py#L1281'>disnake/client.py:1281:53:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/client.py#L1281'>disnake/client.py:1281:53:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/ext/commands/flag_converter.py#L330'>disnake/ext/commands/flag_converter.py:330:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/DisnakeDev/disnake/blob/2919c6177aee6bb33ce61260e7213fe0ec6769e9/disnake/ext/commands/flag_converter.py#L330'>disnake/ext/commands/flag_converter.py:330:33:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -16 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L169'>airflow/providers/weaviate/hooks/weaviate.py:169:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L169'>airflow/providers/weaviate/hooks/weaviate.py:169:32:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L202'>airflow/providers/weaviate/hooks/weaviate.py:202:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L202'>airflow/providers/weaviate/hooks/weaviate.py:202:32:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L233'>airflow/providers/weaviate/hooks/weaviate.py:233:44:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L233'>airflow/providers/weaviate/hooks/weaviate.py:233:44:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L466'>airflow/providers/weaviate/hooks/weaviate.py:466:44:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L466'>airflow/providers/weaviate/hooks/weaviate.py:466:44:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L689'>airflow/providers/weaviate/hooks/weaviate.py:689:40:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/apache/airflow/blob/50a4c951fcc36f90521a7fdcf9d443da5169db1c/airflow/providers/weaviate/hooks/weaviate.py#L689'>airflow/providers/weaviate/hooks/weaviate.py:689:40:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +0 -14 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L114'>tests/unit/commands/local/lib/test_stack_provider.py:114:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L114'>tests/unit/commands/local/lib/test_stack_provider.py:114:51:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L152'>tests/unit/commands/local/lib/test_stack_provider.py:152:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L152'>tests/unit/commands/local/lib/test_stack_provider.py:152:51:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L190'>tests/unit/commands/local/lib/test_stack_provider.py:190:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L190'>tests/unit/commands/local/lib/test_stack_provider.py:190:51:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L229'>tests/unit/commands/local/lib/test_stack_provider.py:229:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L229'>tests/unit/commands/local/lib/test_stack_provider.py:229:51:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L270'>tests/unit/commands/local/lib/test_stack_provider.py:270:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/aws/aws-sam-cli/blob/0446045c4f11ba82a165022c67c7488982094217/tests/unit/commands/local/lib/test_stack_provider.py#L270'>tests/unit/commands/local/lib/test_stack_provider.py:270:51:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -86 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L201'>src/bokeh/core/property/dataspec.py:201:71:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L201'>src/bokeh/core/property/dataspec.py:201:71:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L219'>src/bokeh/io/webdriver.py:219:17:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L219'>src/bokeh/io/webdriver.py:219:17:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L85'>src/bokeh/models/annotations/geometry.py:85:31:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L85'>src/bokeh/models/annotations/geometry.py:85:31:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L89'>src/bokeh/models/annotations/geometry.py:89:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L89'>src/bokeh/models/annotations/geometry.py:89:32:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L93'>src/bokeh/models/annotations/geometry.py:93:30:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L93'>src/bokeh/models/annotations/geometry.py:93:30:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L97'>src/bokeh/models/annotations/geometry.py:97:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L97'>src/bokeh/models/annotations/geometry.py:97:33:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L203'>src/bokeh/models/sources.py:203:37:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L203'>src/bokeh/models/sources.py:203:37:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L205'>src/bokeh/models/sources.py:205:48:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L205'>src/bokeh/models/sources.py:205:48:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1367'>src/bokeh/models/tools.py:1367:101:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
... 69 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/da8ac79a2017001e017b2a742fcb8c81b2f3e7b0/securedrop/pretty_bad_protocol/gnupg.py#L657'>securedrop/pretty_bad_protocol/gnupg.py:657:26:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/freedomofpress/securedrop/blob/da8ac79a2017001e017b2a742fcb8c81b2f3e7b0/securedrop/pretty_bad_protocol/gnupg.py#L657'>securedrop/pretty_bad_protocol/gnupg.py:657:26:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_blink_functions.py#L191'>tests/test_blink_functions.py:191:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_blink_functions.py#L191'>tests/test_blink_functions.py:191:13:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_sync_module.py#L615'>tests/test_sync_module.py:615:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_sync_module.py#L615'>tests/test_sync_module.py:615:13:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L135'>tests/test_util.py:135:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L135'>tests/test_util.py:135:13:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L149'>tests/test_util.py:149:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L149'>tests/test_util.py:149:13:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L166'>tests/test_util.py:166:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/test_util.py#L166'>tests/test_util.py:166:13:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +0 -38 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/impala/tests/test_value_exprs.py#L217'>ibis/backends/impala/tests/test_value_exprs.py:217:9:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/impala/tests/test_value_exprs.py#L217'>ibis/backends/impala/tests/test_value_exprs.py:217:9:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L293'>ibis/backends/pandas/kernels.py:293:21:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L293'>ibis/backends/pandas/kernels.py:293:21:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L296'>ibis/backends/pandas/kernels.py:296:20:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L296'>ibis/backends/pandas/kernels.py:296:20:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L298'>ibis/backends/pandas/kernels.py:298:21:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L298'>ibis/backends/pandas/kernels.py:298:21:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L469'>ibis/backends/pandas/kernels.py:469:22:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/ibis-project/ibis/blob/7ecfaaf8ae1e7c2299c76ade6e44dedb5c4397e2/ibis/backends/pandas/kernels.py#L469'>ibis/backends/pandas/kernels.py:469:22:</a> PLW0108 [*] Lambda may be unnecessary; consider inlining inner function
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+72 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/asv_bench/benchmarks/groupby.py#L954'>asv_bench/benchmarks/groupby.py:954:49:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/asv_bench/benchmarks/io/csv.py#L340'>asv_bench/benchmarks/io/csv.py:340:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/_testing/__init__.py#L343'>pandas/_testing/__init__.py:343:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/_testing/__init__.py#L347'>pandas/_testing/__init__.py:347:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/_testing/__init__.py#L355'>pandas/_testing/__init__.py:355:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/_testing/__init__.py#L359'>pandas/_testing/__init__.py:359:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/core/_numba/extensions.py#L284'>pandas/core/_numba/extensions.py:284:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/core/arrays/arrow/array.py#L167'>pandas/core/arrays/arrow/array.py:167:21:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
... 65 additional changes omitted for rule PLW0108
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/apply/test_series_apply.py#L378'>pandas/tests/apply/test_series_apply.py:378:40:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/groupby/aggregate/test_other.py#L544'>pandas/tests/groupby/aggregate/test_other.py:544:9:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/groupby/aggregate/test_other.py#L545'>pandas/tests/groupby/aggregate/test_other.py:545:9:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/groupby/aggregate/test_other.py#L566'>pandas/tests/groupby/aggregate/test_other.py:566:9:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/groupby/aggregate/test_other.py#L567'>pandas/tests/groupby/aggregate/test_other.py:567:9:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
- <a href='https://github.com/pandas-dev/pandas/blob/4241ba5e1eb5b12adaff4e98df9ddad65ff94ddb/pandas/tests/series/methods/test_map.py#L141'>pandas/tests/series/methods/test_map.py:141:40:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
... 64 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0108 | 244 | 72 | 0 | 0 | 172 |
| PT014 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-03-30 01:20_

Yeah I thought the same thing. Kind of a bummer but oh well.

---
