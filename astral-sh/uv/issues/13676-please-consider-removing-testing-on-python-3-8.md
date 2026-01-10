```yaml
number: 13676
title: Please consider removing testing on Python 3.8
type: issue
state: closed
author: mgorny
labels:
  - testing
assignees: []
created_at: 2025-05-27T12:08:04Z
updated_at: 2025-06-02T09:10:53Z
url: https://github.com/astral-sh/uv/issues/13676
synced_at: 2026-01-10T03:41:47Z
```

# Please consider removing testing on Python 3.8

---

_Issue opened by @mgorny on 2025-05-27 12:08_

I'm sorry for making so many requests.

Currently, uv's test suite (with `python` feature) requires a bunch of different Python versions. This includes Python 3.8 that's been EOL for a while now.

So far I've managed to backport security fixes from newer CPython versions to 3.8, but the most recent fix is too complex, so we'd really like to finally remove the vulnerable Python 3.8 interpreter. However, we can't do that without losing ~200 tests from uv which I believe to be quite important.

Could you please consider removing the requirement for Python 3.8 for testing?

---

_Comment by @zanieb on 2025-05-27 13:43_

I don't think we can / should stop testing on 3.8. We already get a lot of complaints about breakage of 3.7 compatibility which we don't even officially support.

That said, I think it's reasonable to move most tests off of 3.8 unless they actually need it, and we could add a `python-eol` feature flag for those that do cover 3.8.

---

_Label `testing` added by @zanieb on 2025-05-27 13:43_

---

_Comment by @zanieb on 2025-05-27 13:56_

(Looking at this, and there are a lot of tests that don't require 3.8... I'll put up an initial patch)

---

_Comment by @mgorny on 2025-05-28 06:13_

Thanks a lot!

---

_Closed by @zanieb on 2025-05-28 13:58_

---

_Comment by @mgorny on 2025-05-31 05:01_

I'm afraid that's still not solved:

With `cargo test --no-default-features --features git,pypi,python` I'm getting:

```
failures:

---- init::init_existing_environment stdout ----

thread 'init::init_existing_environment' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- init::init_existing_environment_parent stdout ----

thread 'init::init_existing_environment_parent' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- init::init_requires_python_specifiers stdout ----

thread 'init::init_requires_python_specifiers' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- lock::lock_requires_python stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
  × No solution found when resolving dependencies for split (python_full_version >= '3.7' and python_full_version < '3.7.9'):
  ╰─▶ Because the requested Python version (>=3.7) does not satisfy Python>=3.7.9 and pygls>=1.1.0,<=1.2.1 depends on Python>=3.7.9,<4, we can conclude that pygls>=1.1.0,<=1.2.1 cannot be used.
      And because only pygls<=1.3.0 is available, we can conclude that all of:
          pygls>=1.1.0,<1.3.0
          pygls>1.3.0
       cannot be used. (1)

      Because the requested Python version (>=3.7) does not satisfy Python>=3.8 and pygls==1.3.0 depends on Python>=3.8, we can conclude that pygls==1.3.0 cannot be used.
      And because we know from (1) that all of:
          pygls>=1.1.0,<1.3.0
          pygls>1.3.0
       cannot be used, we can conclude that pygls>=1.1.0 cannot be used.
      And because your project depends on pygls>=1.1.0, we can conclude that your project's requirements are unsatisfiable.

      hint: Pre-releases are available for `pygls` in the requested range (e.g., 2.0.0a3), but pre-releases weren't enabled (try: `--prerelease=allow`)

      hint: The `requires-python` value (>=3.7) includes Python versions that are not supported by your dependencies (e.g., pygls>=1.1.0,<=1.2.1 only supports >=3.7.9, <4). Consider using a more restrictive `requires-python` value (like >=3.7.9, <4).

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 17 packages in 664ms

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 13 packages in 117ms

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 5 packages in 109ms

────────────────────────────────────────────────────────────────────────────────


thread 'lock::lock_requires_python' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- python_list::python_list_downloads stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list_downloads
Source: crates/uv/tests/it/python_list.rs:364
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-cpython-3.10.17-[PLATFORM]    <download available>
    4       │-pypy-3.10.16-[PLATFORM]       <download available>
    5       │-graalpy-3.10.0-[PLATFORM]     <download available>
    6     3 │ 
    7     4 │ ----- stderr -----
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_list::python_list_downloads' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'python_list_downloads' failed in line 364

---- run::run_invalid_project_table stdout ----

thread 'run::run_invalid_project_table' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- run::run_isolated_python_version stdout ----

thread 'run::run_isolated_python_version' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- sync::sync_locked_script stdout ----

thread 'sync::sync_locked_script' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test

---- sync::sync_script stdout ----

thread 'sync::sync_script' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test


failures:
    init::init_existing_environment
    init::init_existing_environment_parent
    init::init_requires_python_specifiers
    lock::lock_requires_python
    python_list::python_list_downloads
    run::run_invalid_project_table
    run::run_isolated_python_version
    sync::sync_locked_script
    sync::sync_script

test result: FAILED. 1928 passed; 9 failed; 3 ignored; 0 measured; 0 filtered out; finished in 512.81s
```

I can try to make a PR for the "more obvious" cases later today.

---

_Comment by @zanieb on 2025-05-31 14:57_

I didn't actually confirm this succeeded, I meant to do that after #13688 — thanks for checking back in!



---

_Reopened by @zanieb on 2025-05-31 14:57_

---

_Comment by @mgorny on 2025-05-31 15:22_

Oh, sorry — I already managed to get distracted and forget about this. I'll take a look at it now.

---

_Comment by @mgorny on 2025-05-31 15:51_

#13755 fixes almost all failures. One remains:

```
failures:

---- lock::lock_requires_python stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
  × No solution found when resolving dependencies for split (python_full_version >= '3.7' and python_full_version < '3.7.9'):
  ╰─▶ Because the requested Python version (>=3.7) does not satisfy Python>=3.7.9 and pygls>=1.1.0,<=1.2.1 depends on Python>=3.7.9,<4, we can conclude that pygls>=1.1.0,<=1.2.1 cannot be used.
      And because only pygls<=1.3.0 is available, we can conclude that all of:
          pygls>=1.1.0,<1.3.0
          pygls>1.3.0
       cannot be used. (1)

      Because the requested Python version (>=3.7) does not satisfy Python>=3.8 and pygls==1.3.0 depends on Python>=3.8, we can conclude that pygls==1.3.0 cannot be used.
      And because we know from (1) that all of:
          pygls>=1.1.0,<1.3.0
          pygls>1.3.0
       cannot be used, we can conclude that pygls>=1.1.0 cannot be used.
      And because your project depends on pygls>=1.1.0, we can conclude that your project's requirements are unsatisfiable.

      hint: Pre-releases are available for `pygls` in the requested range (e.g., 2.0.0a3), but pre-releases weren't enabled (try: `--prerelease=allow`)

      hint: The `requires-python` value (>=3.7) includes Python versions that are not supported by your dependencies (e.g., pygls>=1.1.0,<=1.2.1 only supports >=3.7.9, <4). Consider using a more restrictive `requires-python` value (like >=3.7.9, <4).

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 17 packages in 525ms

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 13 packages in 79ms

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 5 packages in 62ms

────────────────────────────────────────────────────────────────────────────────


thread 'lock::lock_requires_python' panicked at crates/uv/tests/it/common/mod.rs:1429:17:
Could not find Python 3.8 for test
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    lock::lock_requires_python

test result: FAILED. 1936 passed; 1 failed; 3 ignored; 0 measured; 0 filtered out; finished in 261.46s
```

Should I just mark it as requiring "eol" or can you think of another solution?

---

_Comment by @konstin on 2025-06-02 07:21_

`lock_requires_python` can use a higher interpreter version too

---

_Comment by @mgorny on 2025-06-02 07:55_

Oh, indeed. I thought it will require more complex changes but apparently not.

---

_Closed by @konstin on 2025-06-02 09:10_

---
