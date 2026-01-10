```yaml
number: 15541
title: "[`flake8-bandit`] Report all references to suspicious functions (`S3`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: S307
created_at: 2025-01-17T00:11:25Z
updated_at: 2025-01-20T12:58:21Z
url: https://github.com/astral-sh/ruff/pull/15541
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-bandit`] Report all references to suspicious functions (`S3`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-17 00:11_

## Summary

Resolves #15522.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-17 00:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/src/airflow/providers/ftp/hooks/ftp.py#L66'>providers/src/airflow/providers/ftp/hooks/ftp.py:66:24:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/src/airflow/providers/ftp/operators/ftp.py#L201'>providers/src/airflow/providers/ftp/operators/ftp.py:201:24:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/src/airflow/providers/ftp/sensors/ftp.py#L84'>providers/src/airflow/providers/ftp/sensors/ftp.py:84:20:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/tests/ftp/sensors/test_ftp.py#L54'>providers/tests/ftp/sensors/test_ftp.py:54:28:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/tests/ftp/sensors/test_ftp.py#L67'>providers/tests/ftp/sensors/test_ftp.py:67:28:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/providers/tests/ftp/sensors/test_ftp.py#L80'>providers/tests/ftp/sensors/test_ftp.py:80:28:</a> S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/tests/decorators/test_python.py#L74'>tests/decorators/test_python.py:74:26:</a> S307 Use of possibly insecure function; consider using `ast.literal_eval`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/migrations/versions/2023-05-01_12-03_9c2a5681ddfd_convert_key_value_entries_to_json.py#L46'>superset/migrations/versions/2023-05-01_12-03_9c2a5681ddfd_convert_key_value_entries_to_json.py:46:27:</a> S301 `pickle` and modules that wrap it can be unsafe when used to deserialize untrusted data, possible security issue
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S321 | 6 | 6 | 0 | 0 | 0 |
| S307 | 1 | 1 | 0 | 0 | 0 |
| S301 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @InSyncWithFoo on 2025-01-17 00:31_

---

_Renamed from "[`flake8-bandit`] Report all references (`S3`)" to "[`flake8-bandit`] Report all references to suspicious functions (`S3`)" by @InSyncWithFoo on 2025-01-17 00:31_

---

_Marked ready for review by @InSyncWithFoo on 2025-01-17 02:30_

---

_Comment by @InSyncWithFoo on 2025-01-17 02:42_

The ecosystem changes are mostly as expected, except for this:

[@apache/airflow](https://github.com/apache/airflow/blob/060eeb73d3636f7ff692acb6c61c7ff948c3574f/providers/src/airflow/providers/ftp/hooks/ftp.py#L282):

```py
class FTPSHook(FTPHook):
    def get_conn(self) -> ftplib.FTP:
#                         ^^^^^^^^^^
```

Should references in type hints be reported?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1013 on 2025-01-17 04:10_

What's the reason for updating this two match patterns?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:916 on 2025-01-17 04:11_

Just to confirm, this is because now we check all references which can come from type annotations as well.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:847 on 2025-01-17 04:27_

I'm not sure what this condition is avoiding, can you expand on this?

---

_@dhruvmanila reviewed on 2025-01-17 04:27_

---

_Comment by @dhruvmanila on 2025-01-17 04:30_

> Should references in type hints be reported?

I think we should ignore them which, I think, is what you've done as well in this PR.

---

_Label `rule` added by @dhruvmanila on 2025-01-17 04:31_

---

_Label `preview` added by @dhruvmanila on 2025-01-17 04:31_

---

_@InSyncWithFoo reviewed on 2025-01-17 13:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1013 on 2025-01-17 13:34_

I think these should not be reported:

```py
import ftplib

# References to the module
print(ftplib)
#     ^^^^^^
print(ftplib.foo)
#     ^^^^^^
```

The second pattern is also covered by [a `match` branch](https://github.com/astral-sh/ruff/pull/15541/files#diff-2eb1b56423c04f6d7842839aa6ea3d70f56306c9fb15c678f7abd3a8cd97719dR848-R850) in `suspicious_function_reference`. Previously this wasn't a problem because it's unlikely for users to call a module (`ftplib()`).

---

_@InSyncWithFoo reviewed on 2025-01-17 13:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:916 on 2025-01-17 13:34_

Yes, that's correct.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:847 on 2025-01-17 13:34_

This is to avoid duplicated diagnostics:

```python
# vvvvvvvvvvvvvvvvvvvvv Already reported as a call expression
  foo.bar(lorem, ipsum)
# ^^^^^^^ Should not be reported as a reference
```

---

_@InSyncWithFoo reviewed on 2025-01-17 13:34_

---

_@dhruvmanila reviewed on 2025-01-20 08:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1013 on 2025-01-20 08:24_

Thanks for providing those examples. I agree that it's unlikely that a module can be callable but it is possible. Regardless, this would mean that we won't be highlighting code like:
```py
import ftplib

getattr(ftplib, 'Error')
```

I don't expect to see this kind of usages but there's no cost to us in detecting them. So, I'd favor in reverting this change unless it's causing some other problem.

---

_@dhruvmanila approved on 2025-01-20 08:37_

---

_@dhruvmanila reviewed on 2025-01-20 08:37_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1013 on 2025-01-20 08:37_

I reverted this but feel free to comment :)

---

_Comment by @dhruvmanila on 2025-01-20 08:47_

Updating the docs with the new preview behavior and then will merge.

---

_Merged by @dhruvmanila on 2025-01-20 09:02_

---

_Closed by @dhruvmanila on 2025-01-20 09:02_

---

_Branch deleted on 2025-01-20 12:58_

---
