```yaml
number: 13879
title: Expand environment variables in user-provided index URLs
type: pull_request
state: open
author: msabramo
labels:
  - needs-decision
assignees: []
base: main
head: env-vars-in-index-urls
created_at: 2025-06-06T04:05:54Z
updated_at: 2025-06-06T14:11:05Z
url: https://github.com/astral-sh/uv/pull/13879
synced_at: 2026-01-12T16:10:54Z
```

# Expand environment variables in user-provided index URLs

---

_@msabramo_

Fixes: GH-5734

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds support for environment variable expansion in `[[tool.uv.index]]` configuration URLs, enabling dynamic and secure index configuration. 

**What it does:**
- Expands environment variables in `url` and `publish-url` fields using `${VARIABLE}` and `${VARIABLE:-default}` syntax
- Supports tilde (`~`) expansion for home directory paths
- Enables secure credential injection without storing sensitive information in plaintext configuration files
- Allows for environment-specific configuration (dev/staging/prod) using the same `pyproject.toml`

**Implementation details:**
- Added `shellexpand` crate dependency for robust environment variable and tilde expansion
- Implemented custom `Deserialize` for the `Index` struct to perform expansion during TOML parsing
- Added comprehensive error handling with descriptive messages for missing variables or expansion failures
- Maintains backward compatibility with existing configurations

**Example usage:**
```toml
[[tool.uv.index]]
name = "artifactory"
url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/simple"
```

## Test Plan

**Unit tests:**
- `test_index_environment_variable_expansion`: Verifies `${VAR}` syntax expansion with set environment variables
- `test_index_without_environment_variables`: Ensures existing configurations without env vars continue to work
- `test_index_missing_environment_variable`: Tests error handling when required environment variables are missing
- `test_index_tilde_expansion`: Verifies `~` home directory expansion works correctly

**Integration testing:**
- Ran `/Users/abramowi/Code/OpenSource/uv/target/release/uv sync` in a project that had something like:
   ```toml
   [[tool.uv.index]]
   name = "artifactory"
   url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/simple"
   ```
- Ran a normal `uv sync` which failed:
```
$ uv sync -v | grep artifactory
...
  × Failed to download `vortex-sdk==0.0.35`
  ├─▶ Failed to fetch:
  │   `https://artifactory.company.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/vortex-sdk/0.0.35/vortex_sdk-0.0.35-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url
      (https://artifactory.company.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/vortex-sdk/0.0.35/vortex_sdk-0.0.35-py3-none-any.whl)
```
- Built uv from this branch with `cargo build --release` 
- Ran `$ /Users/abramowi/Code/OpenSource/uv/target/release/uv sync -v | grep artifactory` and got:
```
DEBUG No cache entry for: https://artifactory.company.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/vortex-sdk/0.0.35/vortex_sdk-0.0.35-py3-none-any.whl
Prepared 1 package in 469ms
Installed 1 package in 3ms
 + vortex-sdk==0.0.35
```

**Compatibility testing:**
- All existing `cargo test -p uv-distribution-types` tests pass
   ```
   abramowi at marcs-mbp-3 in ~/Code/OpenSource/uv (env-vars-in-index-urls●)
   $ cargo test -p uv-distribution-types
   ...
   test result: ok. 19 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
   ```
- No breaking changes to existing index configurations
- Error messages are clear and actionable when environment variables are missing

**Documentation:**
- Updated `docs/concepts/indexes.md` with environment variable expansion section and examples
- Enhanced `docs/guides/integration/alternative-indexes.md` with env var examples for Azure, GCP, and AWS
- Updated `docs/reference/settings.md` with comprehensive examples and feature description

---

_Converted to draft by @msabramo on 2025-06-06 05:11_

---

_Marked ready for review by @msabramo on 2025-06-06 05:12_

---

_Converted to draft by @msabramo on 2025-06-06 05:16_

---

_Comment by @msabramo on 2025-06-06 12:32_

Windows test failed with what looks to be an intermittent failure downloading Python:
```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: python_install_preview-10
    Source: D:\uv:463
    ───────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ───────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬──────────────────────────────────────────────────────────────────
        0       │-success: true
        1       │-exit_code: 0
              0 │+success: false
              1 │+exit_code: 1
        2     2 │ ----- stdout -----
        3     3 │ 
        4     4 │ ----- stderr -----
        5       │-Installed 2 versions in [TIME]
        6       │- + cpython-3.12.6-[PLATFORM]
        7       │- + cpython-3.12.8-[PLATFORM] (python3.12)
              5 │+Installed Python 3.12.6 in [TIME]
              6 │+ + cpython-3.12.6-[PLATFORM] (python3.12)
              7 │+error: Failed to install cpython-3.12.8-[PLATFORM]
              8 │+  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
              9 │+  Caused by: HTTP status server error (500 Internal Server Error) for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
    ────────────┴──────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test python_install::python_install_preview ... FAILED
```

---

_Marked ready for review by @msabramo on 2025-06-06 12:56_

---

_Comment by @msabramo on 2025-06-06 12:58_

> Windows test failed with what looks to be an intermittent failure downloading Python:
> 
> ```
>     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
>     Snapshot: python_install_preview-10
>     Source: D:\uv:463
>     ───────────────────────────────────────────────────────────────────────────────
>     Expression: snapshot
>     ───────────────────────────────────────────────────────────────────────────────
>     -old snapshot
>     +new results
>     ────────────┬──────────────────────────────────────────────────────────────────
>         0       │-success: true
>         1       │-exit_code: 0
>               0 │+success: false
>               1 │+exit_code: 1
>         2     2 │ ----- stdout -----
>         3     3 │ 
>         4     4 │ ----- stderr -----
>         5       │-Installed 2 versions in [TIME]
>         6       │- + cpython-3.12.6-[PLATFORM]
>         7       │- + cpython-3.12.8-[PLATFORM] (python3.12)
>               5 │+Installed Python 3.12.6 in [TIME]
>               6 │+ + cpython-3.12.6-[PLATFORM] (python3.12)
>               7 │+error: Failed to install cpython-3.12.8-[PLATFORM]
>               8 │+  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
>               9 │+  Caused by: HTTP status server error (500 Internal Server Error) for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
>     ────────────┴──────────────────────────────────────────────────────────────────
>     Stopped on the first failure. Run `cargo insta test` to run all snapshots.
>     test python_install::python_install_preview ... FAILED
> ```

Adding an empty commit to force a rerun fixed this temporary issue, though maybe it might be good to add retry logic for this?

Anyway, all tests passing now! 🎉 

![Screenshot 2025-06-06 at 5 57 43 AM](https://github.com/user-attachments/assets/a1c89563-e9d6-43e8-80ee-e8437b57ae77)

---

_Label `needs-decision` added by @zanieb on 2025-06-06 13:46_

---

_Comment by @zanieb on 2025-06-06 13:47_

Thanks for the pull request.

As a brief note, we do not have consensus on a design for this and are very unlikely to accept a pull request without reaching that consensus first. We may be able to use this proposal to help drive consensus forward, but I want to set the right expectations here.

---

_Comment by @msabramo on 2025-06-06 14:11_

> Thanks for the pull request.
> 
> As a brief note, we do not have consensus on a design for this and are very unlikely to accept a pull request without reaching that consensus first. We may be able to use this proposal to help drive consensus forward, but I want to set the right expectations here.

@zanieb 

Yeah understood, I know this is kind of controversial. I was just composing a comment on the issue with a proposal about how to solve some of the concerns. That proposal is not implemented in this PR, but if by discussion we come to some solution that people are happy with, then I could add it to this PR.

---
