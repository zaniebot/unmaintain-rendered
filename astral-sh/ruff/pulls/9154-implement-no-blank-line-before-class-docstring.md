```yaml
number: 9154
title: "Implement `no_blank_line_before_class_docstring` preview style"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/no-blank-line-docstring
created_at: 2023-12-16T01:03:07Z
updated_at: 2023-12-19T06:43:21Z
url: https://github.com/astral-sh/ruff/pull/9154
synced_at: 2026-01-10T23:31:11Z
```

# Implement `no_blank_line_before_class_docstring` preview style

---

_Pull request opened by @dhruvmanila on 2023-12-16 01:03_

## Summary

This PR implements the `no_blank_line_before_class_docstring` preview style.

## Test Plan

Update existing snapshots.

### Formatter ecosystem

`main`

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               213 |
| poetry         |           0.99905 |               321 |                15 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99958 |              1459 |                36 |

`dhruv/no-blank-line-docstring`

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               213 |
| poetry         |           0.99905 |               321 |                15 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99958 |              1459 |                36 |

fixes: #8888 


---

_Label `formatter` added by @dhruvmanila on 2023-12-16 01:03_

---

_Label `preview` added by @dhruvmanila on 2023-12-16 01:03_

---

_Comment by @github-actions[bot] on 2023-12-16 01:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 0 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/10bf8d88360442939c6aa2d5491d74fe2dc5a087/Packs/Automox/Integrations/Automox/Automox.py#L42'>Packs/Automox/Integrations/Automox/Automox.py~L42</a>
```diff
 
 
 class Client(BaseClient):
-
     """Client class to interact with the service API
 
     This Client implements API calls, and does not contain any XSOAR logic.
```
<a href='https://github.com/demisto/content/blob/10bf8d88360442939c6aa2d5491d74fe2dc5a087/Packs/MS-ISAC/Integrations/MSISAC/MSISAC.py#L13'>Packs/MS-ISAC/Integrations/MSISAC/MSISAC.py~L13</a>
```diff
 
 
 class Client(BaseClient):
-
     """
     This client class for MS-ISAC definies two API endpoints
     Query events in a set amount of days /albert/{days}
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -13 lines across 8 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L336'>securedrop/models.py~L336</a>
```diff
 
 
 class InvalidUsernameException(Exception):
-
     """Raised when a user logs in with an invalid username"""
 
 
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L355'>securedrop/models.py~L355</a>
```diff
 
 
 class LoginThrottledException(Exception):
-
     """Raised when a user attempts to log in
     too many times in a given time period"""
 
 
 class WrongPasswordException(Exception):
-
     """Raised when a user logs in with an incorrect password"""
 
 
 class PasswordError(Exception):
-
     """Generic error for passwords that are invalid."""
 
 
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L387'>securedrop/models.py~L387</a>
```diff
 
 
 class NonDicewarePassword(PasswordError):
-
     """Raised when attempting to validate a password that is not diceware-like"""
 
 
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L840'>securedrop/models.py~L840</a>
```diff
 
 
 class JournalistLoginAttempt(db.Model):
-
     """This model keeps track of journalist's login attempts so we can
     rate limit them in order to prevent attackers from brute forcing
     passwords or two-factor tokens."""
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_15ac9509fc68.py#L18'>securedrop/tests/migrations/migration_15ac9509fc68.py~L18</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration has no upgrade because there are no tables in the
     database prior to running, so there is no data to load or test.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_3d91d6948753.py#L12'>securedrop/tests/migrations/migration_3d91d6948753.py~L12</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the UUID column now exists, and that
     the data migration completed successfully.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_60f41bb14d98.py#L21'>securedrop/tests/migrations/migration_60f41bb14d98.py~L21</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the session_nonce column now exists, and
     that the data migration completed successfully.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_6db892e17271.py#L77'>securedrop/tests/migrations/migration_6db892e17271.py~L77</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the deleted_by_source column now exists,
     and that the data migration completed successfully.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_e0a525cbab83.py#L77'>securedrop/tests/migrations/migration_e0a525cbab83.py~L77</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the deleted_by_source column now exists,
     and that the data migration completed successfully.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_f2833ac34bb6.py#L20'>securedrop/tests/migrations/migration_f2833ac34bb6.py~L20</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the UUID column now exists, and that
     the data migration completed successfully.
     """
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/tests/migrations/migration_fccf57ceef02.py#L32'>securedrop/tests/migrations/migration_fccf57ceef02.py~L32</a>
```diff
 
 
 class UpgradeTester:
-
     """This migration verifies that the UUID column now exists, and that
     the data migration completed successfully.
     """
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/366c7211ebb66f1a23f37abae19f410b37dd0dae/mlflow/langchain/retriever_chain.py#L17'>mlflow/langchain/retriever_chain.py~L17</a>
```diff
 
 @experimental
 class _RetrieverChain(Chain):
-
     """
     Chain that wraps a retriever for use with MLflow.
 
```
<a href='https://github.com/mlflow/mlflow/blob/366c7211ebb66f1a23f37abae19f410b37dd0dae/tests/sagemaker/mock/__init__.py#L707'>tests/sagemaker/mock/__init__.py~L707</a>
```diff
 
 
 class TransformJob(TimestampedResource):
-
     """
     Object representing a SageMaker transform job. The SageMakerBackend will create
     and manage transform jobs.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/pip/blob/50e3c500bd2b27dd72317cfd5ff5bb498540e416/src/pip/_internal/index/collector.py#L395'>src/pip/_internal/index/collector.py~L395</a>
```diff
 
 
 class LinkCollector:
-
     """
     Responsible for collecting Link objects from all configured locations,
     making network requests as needed.
```
<a href='https://github.com/pypa/pip/blob/50e3c500bd2b27dd72317cfd5ff5bb498540e416/src/pip/_internal/utils/logging.py#L212'>src/pip/_internal/utils/logging.py~L212</a>
```diff
 
 
 class ExcludeLoggerFilter(Filter):
-
     """
     A logging Filter that excludes records from a logger (or its children).
     """
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+0 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/_distutils/version.py#L111'>setuptools/_distutils/version.py~L111</a>
```diff
 
 
 class StrictVersion(Version):
-
     """Version numbering for anal retentives and software idealists.
     Implements the standard interface for version number classes as
     described above.  A version number consists of two or three
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/_distutils/version.py#L286'>setuptools/_distutils/version.py~L286</a>
```diff
 
 
 class LooseVersion(Version):
-
     """Version numbering for anarchists and software realists.
     Implements the standard interface for version number classes as
     described above.  A version number consists of a series of numbers,
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+0 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/mypy/blob/1dd8e7fe654991b01bd80ef7f1f675d9e3910c3a/mypy/main.py#L342'>mypy/main.py~L342</a>
```diff
 
 
 class CapturableArgumentParser(argparse.ArgumentParser):
-
     """Override ArgumentParser methods that use sys.stdout/sys.stderr directly.
 
     This is needed because hijacking sys.std* is not thread-safe,
```
<a href='https://github.com/python/mypy/blob/1dd8e7fe654991b01bd80ef7f1f675d9e3910c3a/mypy/main.py#L396'>mypy/main.py~L396</a>
```diff
 
 
 class CapturableVersionAction(argparse.Action):
-
     """Supplement CapturableArgumentParser to handle --version.
 
     This is nearly identical to argparse._VersionAction except,
```

</p>
</details>




---

_Comment by @T-256 on 2023-12-16 02:00_

It seems ecosystem reported numbers are wrong.
Also, links pointed lines aren't exact.

---

_Review requested from @konstin by @MichaReiser on 2023-12-16 02:26_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-16 02:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__class_definition.py.snap`:535 on 2023-12-16 02:28_

Can we add a few tests where we have comments before the docstring (including new or empty lines)

---

_@MichaReiser reviewed on 2023-12-16 02:30_

Could you update your pr summary and include the 'old' compatibility numbers. I don't expect them to change but just to be safe

---

_Comment by @charliermarsh on 2023-12-16 03:07_

The ecosystem line numbers are not guaranteed to be exact (hence the tilde). It’s a diff of two changes against HEAD, so mapping that diff back to a line number in HEAD is not trivial. (Not sure about the “number of lines changed” though.)

---

_@konstin approved on 2023-12-18 08:21_

---

_@dhruvmanila reviewed on 2023-12-19 06:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__class_definition.py.snap`:535 on 2023-12-19 06:12_

I've tested it out locally with different combinations of comments, newline and docstrings. It seems that the Black preview style is a bit inconsistent with the stable style.

<details><summary>Original source code:</summary>
<p>

```python
class OneBlankLineDocstring:

    """This is a docstring."""


class DocstringWithComment0:
    # This is a comment
    """This is a docstring."""


class DocstringWithComment1:
    # This is a comment

    """This is a docstring."""


class DocstringWithComment2:

    # This is a comment
    """This is a docstring."""


class DocstringWithComment3:

    # This is a comment

    """This is a docstring."""


class DocstringWithComment4:


    # This is a comment


    """This is a docstring."""
```

</p>
</details> 

<details><summary>Black stable formatting:</summary>
<p>

```python
class NormalDocstring:

    """This is a docstring."""


class DocstringWithComment0:
    # This is a comment
    """This is a docstring."""


class DocstringWithComment1:
    # This is a comment

    """This is a docstring."""


class DocstringWithComment2:
    # This is a comment
    """This is a docstring."""


class DocstringWithComment3:
    # This is a comment

    """This is a docstring."""


class DocstringWithComment4:
    # This is a comment

    """This is a docstring."""
```

</p>
</details> 

<details><summary>Black preview formatting:</summary>
<p>

```python
class OneBlankLineDocstring:
    """This is a docstring."""


class DocstringWithComment0:
    # This is a comment
    """This is a docstring."""


class DocstringWithComment1:
    # This is a comment

    """This is a docstring."""


class DocstringWithComment2:

    # This is a comment
    """This is a docstring."""


class DocstringWithComment3:

    # This is a comment

    """This is a docstring."""


class DocstringWithComment4:

    # This is a comment

    """This is a docstring."""
```

</p>
</details>

What I'm noticing is that the rules change if there's a comment in between the class header and docstring in a way that for `n1` and `n2` number of blank lines before a comment and docstring respectively the following holds true:
1. If `n1` / `n2` is 0, keep it 0
2. If `n1` / `n2` is 1, keep it 1
3. If `n1` / `n2` > 1, then reduce it to 1

It's probably another preview style which is keeping the newline before the comment (`allow_empty_first_line_before_block_or_comment`).

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-12-19 06:12_

---

_@MichaReiser approved on 2023-12-19 06:23_

---

_Merged by @dhruvmanila on 2023-12-19 06:43_

---

_Closed by @dhruvmanila on 2023-12-19 06:43_

---

_Branch deleted on 2023-12-19 06:43_

---
