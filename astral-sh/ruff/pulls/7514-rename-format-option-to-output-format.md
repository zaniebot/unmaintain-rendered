```yaml
number: 7514
title: "Rename `format` option to `output-format`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: rename-format-to-output-format
created_at: 2023-09-19T10:03:20Z
updated_at: 2023-09-20T13:19:00Z
url: https://github.com/astral-sh/ruff/pull/7514
synced_at: 2026-01-12T02:39:10Z
```

# Rename `format` option to `output-format`

---

_Pull request opened by @MichaReiser on 2023-09-19 10:03_

## Summary

This PR renames the `check --format` option to `--output-format`, the `RUFF_FORMAT` environment variable to `RUFF_OUTPUT_FORMAT`, and the `format` configuration option to `output-format` to avoid ambiguity and make the `format` option available for the formatter.

The issue https://github.com/astral-sh/ruff/issues/7354 suggests building support in our configuration macro. I decided against it because it requires multiple manual edits anyway (CLI, add warning, when loading the configuration from options, another warning).

Closes https://github.com/astral-sh/ruff/issues/7354


## Test Plan

New option works 

```bash
❯ cargo run --bin ruff -- check  . --no-cache --output-format=grouped
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/ruff check . --no-cache --output-format=grouped`
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```

The `--format` option works but prints a warning

```bash
❯ cargo run --bin ruff -- check  . --no-cache --format=grouped
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check . --no-cache --format=grouped`
warning: The argument `--format=<FORMAT>` is deprecated. Use `--output-format=<FORMAT>` instead.
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```

Using both options prints an error

```bash
❯ cargo run --bin ruff -- check  . --no-cache --format=grouped --output-format=text
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check . --no-cache --format=grouped --output-format=text`
error: the argument '--format <FORMAT>' cannot be used with '--output-format <OUTPUT_FORMAT>'

Usage: ruff check --no-cache <FILES>...

For more information, try '--help'.
```

Using the old environment variable prints a warning but applies the format

```bash
❯ RUFF_FORMAT=grouped cargo run --bin ruff -- check  . --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check . --no-cache`
warning: The environment variable `RUFF_FORMAT` is deprecated. Use `RUFF_OUTPUT_FORMAT` instead.
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```

Using the new environment variable works

```bash
❯ RUFF_OUTPUT_FORMAT=grouped cargo run --bin ruff -- check  . --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check . --no-cache`
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```


Using both environment variables emits an error

```bash
❯ RUFF_FORMAT=text RUFF_OUTPUT_FORMAT=grouped cargo run --bin ruff -- check  . --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check . --no-cache`
error: the argument '--format <FORMAT>' cannot be used with '--output-format <OUTPUT_FORMAT>'

Usage: ruff check --no-cache <FILES>...

For more information, try '--help'.
```

Using the `format` option prints a warning but applies the format

```toml
[tool.ruff]
format = "grouped"
```

```bash
❯ cargo run --bin ruff -- check  . --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/ruff check . --no-cache`
warning: The option `format` has been deprecated to avoid ambiguity with Ruff's upcoming formatter. Use `format-output` instead.
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```

Using the new `output-format` option applies the format

```toml
[tool.ruff]
output-format = "grouped"
```

```bash
❯ cargo run --bin ruff -- check  . --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff check . --no-cache`
crates/ruff_cli/resources/test/fixtures/cache_remove_old_files/source.py:
  4:1 F822 Undefined name `b` in `__all__`

crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:
   2:8  F401 [*] `os` imported but unused
   5:47 F401 [*] `airflow.jobs.scheduler_job_runner.SchedulerJobRunner` imported but unused
   8:38 F401 [*] `airflow.compat.functools.cached_property` imported but unused
  11:54 F401 [*] `airflow.providers.google.cloud.hooks.gcs.GCSHook` imported but unused
  14:28 F401 [*] `sqlalchemy.orm.Query` imported but unused

Found 6 errors.
[*] 5 potentially fixable with the --fix option.
```

---

_Comment by @MichaReiser on 2023-09-19 10:03_

Current dependencies on/for this PR:
* main
  * **PR #7514** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7514" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7514?utm_source=stack-comment).

---

_@konstin approved on 2023-09-19 10:04_

extra snapshot, otherwise i really like this!

---

_Comment by @MichaReiser on 2023-09-19 10:07_

@charliermarsh should we mark this PR as breaking or only when we remove the option entirely?

---

_Label `breaking` added by @MichaReiser on 2023-09-19 10:08_

---

_Label `cli` added by @MichaReiser on 2023-09-19 10:08_

---

_Marked ready for review by @MichaReiser on 2023-09-19 10:13_

---

_Comment by @github-actions[bot] on 2023-09-19 10:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-19 12:18_

For how long do you anticipate leaving these deprecation warnings in place?

> @charliermarsh should we mark this PR as breaking or only when we remove the option entirely?

Only when removing the option but we should call it out explicitly at the top of the release notes.


---

_Label `breaking` removed by @MichaReiser on 2023-09-19 13:37_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-09-20 06:39_

---

_@charliermarsh approved on 2023-09-20 12:56_

---

_Merged by @MichaReiser on 2023-09-20 13:18_

---

_Closed by @MichaReiser on 2023-09-20 13:18_

---

_Branch deleted on 2023-09-20 13:19_

---
