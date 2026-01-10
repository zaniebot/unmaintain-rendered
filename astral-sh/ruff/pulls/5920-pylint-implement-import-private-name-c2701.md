```yaml
number: 5920
title: "[`pylint`] Implement `import-private-name` (`C2701`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: import-private-name
created_at: 2023-07-20T13:49:11Z
updated_at: 2024-01-16T21:02:31Z
url: https://github.com/astral-sh/ruff/pull/5920
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Implement `import-private-name` (`C2701`)

---

_Pull request opened by @tjkuson on 2023-07-20 13:49_

## Summary

Implements  [`import-private-name` (`C2701`)](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/import-private-name.html) as  `import-private-name` (`PLC2701`). Includes documentation.

Related to #970.

Closes https://github.com/astral-sh/ruff/issues/9138.

### PEP 420 namespace package limitation

`checker.module_path` doesn't seem to support automatic detection of namespace packages (PEP 420). This leads to 'false' positives (Pylint allows both).

Currently, for this to work like Pylint, users would have to [manually input known namespace packages](https://beta.ruff.rs/docs/settings/#namespace-packages).

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-20 14:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.23ms     4.9 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1598.8±5.51µs    10.4 MB/sec    1.01  1618.3±12.99µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    171.0±1.75µs    17.3 MB/sec    1.00    171.1±0.30µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.04ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.6±0.03ms     3.9 MB/sec    1.00     10.5±0.04ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.00ms     5.8 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    318.7±0.98µs     9.3 MB/sec    1.00    316.4±1.32µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.01ms     5.3 MB/sec    1.00      4.8±0.04ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.02      5.4±0.01ms     7.5 MB/sec    1.00      5.3±0.02ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1115.0±3.37µs    14.9 MB/sec    1.00   1101.9±3.31µs    15.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    114.1±0.33µs    25.9 MB/sec    1.00    114.0±0.19µs    25.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.01ms    10.6 MB/sec    1.00      2.4±0.00ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.08ms     4.2 MB/sec    1.00      9.6±0.08ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1883.8±33.01µs     8.8 MB/sec    1.00  1879.0±27.07µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00   213.3±11.27µs    13.8 MB/sec    1.00    214.1±8.20µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.05ms     6.3 MB/sec    1.01      4.1±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.02     14.4±0.53ms     2.8 MB/sec    1.00     14.1±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.8±0.23ms     4.3 MB/sec    1.00      3.7±0.03ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   467.7±24.58µs     6.3 MB/sec    1.00    444.9±5.85µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±0.28ms     3.9 MB/sec    1.00      6.3±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.44ms     5.6 MB/sec    1.14      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1440.0±65.45µs    11.6 MB/sec    1.15  1651.3±12.67µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.1±3.10µs    18.5 MB/sec    1.13    180.5±2.43µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.09ms     8.3 MB/sec    1.16      3.6±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tjkuson on 2023-07-20 14:44_

Most of the triggers from the ecosystem check seem to be in test suites, which is fine. The others seem to be false positives from not checking if a module is importing something from itself.

---

_Comment by @tjkuson on 2023-07-20 22:58_

Looking through the ecosystem check, the flagged violations are all

- true positives,
- issues with PEP 420 (which can be fixed using the [`namespace-packages` setting](https://beta.ruff.rs/docs/settings/#namespace-packages),
- or test suites importing private names for testing reasons (which makes sense).

---

_Renamed from "[`pylint`] Impliment `import-private-name` (`C2701`)" to "[`pylint`] Implement `import-private-name` (`C2701`)" by @charliermarsh on 2023-07-24 19:35_

---

_Comment by @tjkuson on 2023-07-27 10:47_

Related: #6114.

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:54 on 2023-08-01 11:10_

```suggestion
                format!("Private name import `{name}` from external module `{module}`")
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:56 on 2023-08-01 11:10_

```suggestion
            None => format!("Private name import `{name}`"),
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:74 on 2023-08-01 11:12_

nit: You can use let-else to avoid the indentation
```suggestion
    let Some(module) = module else {
        return;
    }
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:75 on 2023-08-01 11:14_

Would it make sense to allow names starting with `__` in general?

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:79 on 2023-08-01 11:25_

Does this work when the namespace packages are defined in the settings?

---

_@konstin approved on 2023-08-01 11:30_

---

_@tjkuson reviewed on 2023-08-01 11:50_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:79 on 2023-08-01 11:50_

I think so (I tried it on one repository). I want to try checking it on more repositories before marking it ready for review (assuming requiring that namespace packages are marked in the setting isn't a deal-breaker for this PR).

---

_Review comment by @tjkuson on `crates/ruff/src/rules/pylint/rules/import_private_name.rs`:75 on 2023-08-01 12:09_

This is what Pylint does, so I think you are right.

---

_@tjkuson reviewed on 2023-08-01 12:09_

---

_Marked ready for review by @tjkuson on 2023-08-02 10:55_

---

_Review requested from @charliermarsh by @konstin on 2023-08-02 12:16_

---

_Comment by @AiyionPrime on 2023-10-17 09:09_

@tjkuson Is there anything I can do to help moving this forward?
I'd really like to have this checker in ruff.

Or is all that's left to to a rebase and the review of @charliermarsh?

---

_Comment by @tjkuson on 2023-10-17 09:23_

> @tjkuson Is there anything I can do to help moving this forward? I'd really like to have this checker in ruff.
> 
> Or is all that's left to to a rebase and the review of @charliermarsh?

Wow, I forgot about this PR! If I recall correctly, this PR was working but required users to specify `namespace` packages (otherwise, it would false positive). I'm not a maintainer, but perhaps the team are waiting for #6114 to be solved before accepting this merge?

@charliermarsh I don't mind rebasing and tweaking the PR if you are open to it being merged (I have probably improved with Rust over these months).

---

_Comment by @AiyionPrime on 2023-10-17 10:37_

If wanted I'd be open to test it prior merging.
Obviously I'm not a maintainer either, but to my understanding C2701 is working, but has a known limitation for projects with namespace packages, which are not all projects.

Might a note in the docs regarding this behavior be sufficient?
It feels like #6114 would be a nice addition but is a quite orthogonal feature?
Or are there likely changes necessary in this PR once it is merged?

Anyway, thanks for the fast response, let me know when I can help :)

---

_Comment by @charliermarsh on 2023-12-12 20:57_

I think we probably _do_ need to support the type annotation thing, since we changed `flake8-self` to support this: https://github.com/astral-sh/ruff/pull/9036

---

_Comment by @charliermarsh on 2023-12-12 21:00_

How does this compare to the flake8-self rules?

---

_Comment by @tjkuson on 2023-12-12 21:08_

> I think we probably _do_ need to support the type annotation thing, since we changed `flake8-self` to support this: #9036

Sure! It should be a simple change if you are still open to the rule being merged.

> How does this compare to the flake8-self rules?

`private-member-access` doesn't check imports. For example,

```python
from foo import _bar

bar = _bar(...)
```

won't trigger the rule, but would trigger `private-import-name`.



---

_Comment by @charliermarsh on 2023-12-12 21:18_

Thanks @tjkuson. I assume they would both trigger on:

```python
from foo import _bar

bar = _bar._baz(...)
```

And that only flake8-self would trigger on:

```python
from foo import bar

bar = bar._baz(...)
```

Is that right?

---

_Comment by @tjkuson on 2023-12-12 21:21_

Yes! Are you thinking of merging them as one rule?

---

_Comment by @charliermarsh on 2023-12-12 21:22_

@tjkuson - Eventually maybe... For now, I'm okay with them being separate as long as they don't have overlap.

---

_Converted to draft by @tjkuson on 2023-12-21 17:33_

---

_Comment by @tjkuson on 2023-12-21 17:41_

Rebased to `main`, everything seems to be working. Checking if an import is used as a type annotation will take longer as I need to make it a deferred rule…

---

_Comment by @github-actions[bot] on 2023-12-21 17:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+473 -1002 violations, +0 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:27:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:39:</a> PLC2701 Private name import `_overload_with_permissions` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/core.py#L31'>disnake/ext/commands/core.py:31:5:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/core.py#L32'>disnake/ext/commands/core.py:32:5:</a> PLC2701 Private name import `_overload_with_permissions` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/flags.py#L8'>disnake/ext/commands/flags.py:8:27:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/docs/extensions/attributetable.py#L13'>docs/extensions/attributetable.py:13:27:</a> PLC2701 Private name import `_` from external module `sphinx.locale`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+98 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/api/auth/backend/kerberos_auth.py#L55'>airflow/api/auth/backend/kerberos_auth.py:55:51:</a> PLC2701 Private name import `_request_ctx_stack` from external module `flask`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/io/path.py#L29'>airflow/io/path.py:29:52:</a> PLC2701 Private name import `_CloudAccessor` from external module `upath.implementations.cloud`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:56:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:79:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:49:</a> PLC2701 Private name import `_blobs_page_start` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:68:</a> PLC2701 Private name import `_item_to_blob` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/hooks/base_google.py#L42'>airflow/providers/google/common/hooks/base_google.py:42:35:</a> PLC2701 Private name import `_http_client` from external module `google.auth.transport`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L149'>airflow/providers/google/common/utils/id_token_credentials.py:149:29:</a> PLC2701 Private name import `_cloud_sdk` from external module `google.auth`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L175'>airflow/providers/google/common/utils/id_token_credentials.py:175:48:</a> PLC2701 Private name import `_metadata` from external module `google.auth.compute_engine`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L178'>airflow/providers/google/common/utils/id_token_credentials.py:178:39:</a> PLC2701 Private name import `_http_client` from external module `google.auth.transport`
... 88 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+180 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/schema/make_schema.py#L12'>schema/make_schema.py:12:32:</a> PLC2701 Private name import `_SAM_CLI_COMMAND_PACKAGES` from external module `samcli.cli.command`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/package/test_package_command_image.py#L13'>tests/integration/package/test_package_command_image.py:13:45:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/sync/test_sync_adl.py#L5'>tests/integration/sync/test_sync_adl.py:5:67:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:68:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:84:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_option_nargs.py#L4'>tests/unit/commands/_utils/custom_options/test_option_nargs.py:4:64:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py#L4'>tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py:4:71:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_cdk_support_decorators.py#L4'>tests/unit/commands/_utils/test_cdk_support_decorators.py:4:59:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_click_mutex.py#L3'>tests/unit/commands/_utils/test_click_mutex.py:3:48:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_command_exception_handler.py#L7'>tests/unit/commands/_utils/test_command_exception_handler.py:7:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
... 170 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+94 -1002 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L39'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:39:27:</a> PLC2701 Private name import `_` from external module `sphinx.locale`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/command/subcommands/test_json__subcommands.py#L29'>tests/unit/bokeh/command/subcommands/test_json__subcommands.py:29:31:</a> PLC2701 Private name import `_util_subcommands`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_aliases.py#L29'>tests/unit/bokeh/core/property/test_aliases.py:29:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_aliases.py#L29'>tests/unit/bokeh/core/property/test_aliases.py:29:43:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_any.py#L22'>tests/unit/bokeh/core/property/test_any.py:22:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_any.py#L22'>tests/unit/bokeh/core/property/test_any.py:22:43:</a> PLC2701 Private name import `_util_property`
... 89 additional changes omitted for rule PLC2701
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L10'>tests/unit/bokeh/plotting/test__decorators.py:10:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L11'>tests/unit/bokeh/plotting/test__decorators.py:11:35:</a> E261 [*] Insert at least two spaces before an inline comment
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:14:</a> E203 [*] Whitespace before ';'
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:15:</a> E702 Multiple statements on one line (semicolon)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:17:</a> B018 Found useless expression. Either assign it to a variable or remove it.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:1:</a> I001 Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L15'>tests/unit/bokeh/plotting/test__decorators.py:15:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L17'>tests/unit/bokeh/plotting/test__decorators.py:17:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> INP001 File `tests/unit/bokeh/plotting/test__decorators.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L20'>tests/unit/bokeh/plotting/test__decorators.py:20:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L23'>tests/unit/bokeh/plotting/test__decorators.py:23:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L24'>tests/unit/bokeh/plotting/test__decorators.py:24:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L25'>tests/unit/bokeh/plotting/test__decorators.py:25:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L28'>tests/unit/bokeh/plotting/test__decorators.py:28:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L28'>tests/unit/bokeh/plotting/test__decorators.py:28:41:</a> E261 [*] Insert at least two spaces before an inline comment
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L30'>tests/unit/bokeh/plotting/test__decorators.py:30:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L32'>tests/unit/bokeh/plotting/test__decorators.py:32:1:</a> E265 [*] Block comment should start with `# `
... 99 additional changes omitted for rule E265
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> FIX002 Line contains TODO, consider resolving the issue
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L46'>tests/unit/bokeh/plotting/test__decorators.py:46:25:</a> C408 Unnecessary `dict` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L56'>tests/unit/bokeh/plotting/test__decorators.py:56:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L56'>tests/unit/bokeh/plotting/test__decorators.py:56:26:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:39:</a> ANN001 Missing type annotation for function argument `arg`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:44:</a> ANN001 Missing type annotation for function argument `values`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:5:</a> D103 Missing docstring in public function
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L59'>tests/unit/bokeh/plotting/test__decorators.py:59:25:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L60'>tests/unit/bokeh/plotting/test__decorators.py:60:17:</a> ANN202 Missing return type annotation for private function `foo`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L60'>tests/unit/bokeh/plotting/test__decorators.py:60:23:</a> ANN003 Missing type annotation for `**kw`
... 1059 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/secure_tempfile.py#L3'>securedrop/secure_tempfile.py:3:22:</a> PLC2701 Private name import `_TemporaryFileWrapper` from external module `tempfile`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/conftest.py#L24'>securedrop/tests/conftest.py:24:25:</a> PLC2701 Private name import `_SourceScryptManager` from external module `source_user`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_config.py#L3'>securedrop/tests/test_config.py:3:22:</a> PLC2701 Private name import `_parse_config_from_file` from external module `sdconfig`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_source_user.py#L11'>securedrop/tests/test_source_user.py:11:5:</a> PLC2701 Private name import `_DesignationGenerator` from external module `source_user`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_source_user.py#L12'>securedrop/tests/test_source_user.py:12:5:</a> PLC2701 Private name import `_SourceScryptManager` from external module `source_user`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/backends/app/backend_info_app.py#L14'>docs/backends/app/backend_info_app.py:14:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py#L6'>docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py:6:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/datafusion_ibis.py#L4'>docs/posts/pydata-performance-part2/datafusion_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/duckdb_ibis.py#L4'>docs/posts/pydata-performance-part2/duckdb_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/polars_ibis.py#L4'>docs/posts/pydata-performance-part2/polars_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step0.py#L4'>docs/posts/pydata-performance/step0.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step1.py#L4'>docs/posts/pydata-performance/step1.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step2.py#L4'>docs/posts/pydata-performance/step2.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step3.py#L4'>docs/posts/pydata-performance/step3.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/ibis/backends/conftest.py#L11'>ibis/backends/conftest.py:11:8:</a> PLC2701 Private name import `_pytest`
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (45 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2701 | 473 | 473 | 0 | 0 | 0 |
| S101 | 230 | 0 | 230 | 0 | 0 |
| Q000 | 150 | 0 | 150 | 0 | 0 |
| E265 | 104 | 0 | 104 | 0 | 0 |
| D102 | 63 | 0 | 63 | 0 | 0 |
| ANN101 | 60 | 0 | 60 | 0 | 0 |
| PLR6301 | 60 | 0 | 60 | 0 | 0 |
| PLR2004 | 47 | 0 | 47 | 0 | 0 |
| SLF001 | 38 | 0 | 38 | 0 | 0 |
| E402 | 23 | 0 | 23 | 0 | 0 |
| C408 | 22 | 0 | 22 | 0 | 0 |
| E275 | 18 | 0 | 18 | 0 | 0 |
| PT011 | 17 | 0 | 17 | 0 | 0 |
| E231 | 16 | 0 | 16 | 0 | 0 |
| E261 | 15 | 0 | 15 | 0 | 0 |
| ANN001 | 15 | 0 | 15 | 0 | 0 |
| ERA001 | 14 | 0 | 14 | 0 | 0 |
| D101 | 12 | 0 | 12 | 0 | 0 |
| N801 | 12 | 0 | 12 | 0 | 0 |
| E203 | 8 | 0 | 8 | 0 | 0 |
| E702 | 7 | 0 | 7 | 0 | 0 |
| B018 | 7 | 0 | 7 | 0 | 0 |
| I001 | 7 | 0 | 7 | 0 | 0 |
| D100 | 7 | 0 | 7 | 0 | 0 |
| INP001 | 7 | 0 | 7 | 0 | 0 |
| D103 | 6 | 0 | 6 | 0 | 0 |
| A002 | 6 | 0 | 6 | 0 | 0 |
| E202 | 4 | 0 | 4 | 0 | 0 |
| E201 | 4 | 0 | 4 | 0 | 0 |
| N802 | 3 | 0 | 3 | 0 | 0 |
| FIX002 | 2 | 0 | 2 | 0 | 0 |
| TD002 | 2 | 0 | 2 | 0 | 0 |
| TD003 | 2 | 0 | 2 | 0 | 0 |
| PT018 | 2 | 0 | 2 | 0 | 0 |
| DTZ001 | 2 | 0 | 2 | 0 | 0 |
| PT006 | 1 | 0 | 1 | 0 | 0 |
| ANN202 | 1 | 0 | 1 | 0 | 0 |
| ANN003 | 1 | 0 | 1 | 0 | 0 |
| ARG001 | 1 | 0 | 1 | 0 | 0 |
| PT012 | 1 | 0 | 1 | 0 | 0 |
| ANN201 | 1 | 0 | 1 | 0 | 0 |
| PLC0415 | 1 | 0 | 1 | 0 | 0 |
| E225 | 1 | 0 | 1 | 0 | 0 |
| E241 | 1 | 0 | 1 | 0 | 0 |
| PLR0402 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @tjkuson on 2023-12-21 19:36_

---

_Comment by @tjkuson on 2023-12-21 19:37_

@charliermarsh The PR has been rebased and now forgives private name imports that are used for type annotations!

---

_Comment by @codspeed-hq[bot] on 2023-12-29 10:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:import-private-name)

### Merging #5920 will **improve performances by ×4.9**

<sub>Comparing <code>tjkuson:import-private-name</code> (c98ef43) with <code>main</code> (c6d8076)</sub>



### Summary

`⚡ 15` improvements
`✅ 15` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:import-private-name` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.2 ms | 3 ms | +38.1% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 92.7 ms | 19 ms | ×4.9 |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 16.8 ms | 12.3 ms | +36.93% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 77 ms | 48.5 ms | +58.64% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 36.1 ms | 23.3 ms | +54.91% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 1,799.7 µs | 635.5 µs | ×2.8 |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 39.3 ms | 26.5 ms | +48.23% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 85.3 ms | 55.9 ms | +52.41% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 4.5 ms | 3.4 ms | +34.15% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 16.9 ms | 4 ms | ×4.2 |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 6 ms | 1.5 ms | ×4.1 |
| ⚡ | `linter/all-rules[large/dataset.py]` | 176 ms | 102.5 ms | +71.67% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 36.2 ms | 8.4 ms | ×4.3 |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 17.9 ms | 13.4 ms | +33.11% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 198.4 ms | 122 ms | +62.57% |


---

_Comment by @charliermarsh on 2024-01-16 04:34_

Sorry, this is blocked on me, I've just been slow to get to it.

---

_Label `rule` added by @charliermarsh on 2024-01-16 04:34_

---

_Label `preview` added by @charliermarsh on 2024-01-16 04:34_

---

_@charliermarsh approved on 2024-01-16 05:11_

---

_Comment by @charliermarsh on 2024-01-16 05:11_

Great work @tjkuson.

---

_Merged by @charliermarsh on 2024-01-16 05:17_

---

_Closed by @charliermarsh on 2024-01-16 05:17_

---

_Comment by @MichaReiser on 2024-01-16 08:24_

I'm going to revert this change. It's panicking in the pydantic benchmark run (see https://github.com/astral-sh/ruff/actions/runs/7537305800/job/20516059364?pr=5920). 

---

_@MichaReiser reviewed on 2024-01-16 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`:137 on 2024-01-16 08:26_

This doesn't seem to be safe. How is it guaranteed that `index` belongs to `module_name` and not the member name?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`:137 on 2024-01-16 13:47_

Just my fault from a last-minute refactor prior to merging.

---

_@charliermarsh reviewed on 2024-01-16 13:47_

---

_@tjkuson reviewed on 2024-01-16 18:08_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`:137 on 2024-01-16 18:08_

#9553 slices over `call_path` instead.

---

_Branch deleted on 2024-01-16 21:02_

---
