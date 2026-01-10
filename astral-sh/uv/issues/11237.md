```yaml
number: 11237
title: "Two `run.rs` test regressions in 0.5.28, apparently due to installing `typing-extensions` (?)"
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2025-02-05T07:56:30Z
updated_at: 2025-02-06T12:41:48Z
url: https://github.com/astral-sh/uv/issues/11237
synced_at: 2026-01-10T03:50:31Z
```

# Two `run.rs` test regressions in 0.5.28, apparently due to installing `typing-extensions` (?)

---

_Issue opened by @mgorny on 2025-02-05 07:56_

### Summary

```
$ cargo test
[…]
failures:

---- run::run_repeated stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.13.1 interpreter at: /home/mgorny/.local/share/uv/tests/.tmpp1Kpy8/python/3.13/python3
Creating virtual environment at: .venv
Resolved 2 packages in 373ms
Prepared 1 package in 116ms
Installed 1 package in 14ms
 + iniconfig==2.0.0
Resolved 1 package in 307ms
Prepared 1 package in 113ms
Installed 1 package in 1ms
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 2 packages in 97ms
Audited 1 package in 0.04ms
Resolved 1 package in 12ms
Installed 1 package in 8ms
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: run_repeated-2
Source: crates/uv/tests/it/run.rs:3914
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    3     3 │ 
    4     4 │ ----- stderr -----
    5     5 │ Resolved 2 packages in [TIME]
    6     6 │ Audited 1 package in [TIME]
    7       │-Resolved 1 package in [TIME]
          7 │+Resolved 1 package in [TIME]
          8 │+Installed 1 package in [TIME]
          9 │+ + typing-extensions==4.10.0
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'run::run_repeated' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.42.1/src/runtime.rs:679:13:
snapshot assertion for 'run_repeated-2' failed in line 3914
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- run::run_without_overlay stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.13.1 interpreter at: /home/mgorny/.local/share/uv/tests/.tmpsb7AWS/python/3.13/python3
Creating virtual environment at: .venv
Resolved 2 packages in 444ms
Prepared 1 package in 132ms
Installed 1 package in 9ms
 + iniconfig==2.0.0
Resolved 1 package in 324ms
Prepared 1 package in 162ms
Installed 1 package in 26ms
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 1 package in 20ms
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import typing_extensions; import iniconfig
                              ^^^^^^^^^^^^^^^^
ModuleNotFoundError: No module named 'iniconfig'

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 2 packages in 305ms
Audited 1 package in 0.04ms
Resolved 1 package in 9ms
Installed 1 package in 9ms
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: run_without_overlay-3
Source: crates/uv/tests/it/run.rs:4001
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    3     3 │ 
    4     4 │ ----- stderr -----
    5     5 │ Resolved 2 packages in [TIME]
    6     6 │ Audited 1 package in [TIME]
    7       │-Resolved 1 package in [TIME]
          7 │+Resolved 1 package in [TIME]
          8 │+Installed 1 package in [TIME]
          9 │+ + typing-extensions==4.10.0
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'run::run_without_overlay' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.42.1/src/runtime.rs:679:13:
snapshot assertion for 'run_without_overlay-3' failed in line 4001


failures:
    run::run_repeated
    run::run_without_overlay

test result: FAILED. 1656 passed; 2 failed; 4 ignored; 0 measured; 0 filtered out; finished in 557.08s

error: test failed, to rerun pass `-p uv --test it`
```

### Platform

Gentoo Linux

### Version

0.5.28

### Python version

3.13.1

---

_Label `bug` added by @mgorny on 2025-02-05 07:56_

---

_Comment by @charliermarsh on 2025-02-05 13:51_

This looks like evidence of a real bug. You shouldn’t need to reinstall there.

---

_Comment by @charliermarsh on 2025-02-05 13:56_

Is there a Dockerfile I can use to reproduce this?

---

_Comment by @mgorny on 2025-02-05 14:22_

I've hit it from the git repo as my user. I'll try to write a Dockerfile and see if it reproduces in a minute.

---

_Comment by @charliermarsh on 2025-02-05 14:35_

Thank you.

---

_Label `testing` added by @zanieb on 2025-02-05 15:05_

---

_Comment by @mgorny on 2025-02-05 16:33_

Would a Dockerfile that triggers a few failures more work? I've already spent a little too much time on it, and I really have no clue what's up with these venv tests (I don't see it locally).

The two relevant cases are:

```
    run::run_repeated
    run::run_without_overlay
```

I see the dockerfile also triggers `build_backend::preserve_executable_bit` failing because I've skipped `git` feature to make it more contained. Feel free to ignore the rest. I can try improving it later if necessary.

```
FROM gentoo/python
RUN wget --progress=dot:mega -O - https://github.com/gentoo-mirror/gentoo/archive/master.tar.gz | tar -xz \
 && mv gentoo-master /var/db/repos/gentoo
RUN getuto
RUN printf "[binhost]\nsync-uri = https://distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64/\n" > /etc/portage/binrepos.conf/gentoobinhost.conf
RUN emerge -vng dev-vcs/git dev-lang/rust-bin dev-build/cmake dev-build/ninja --jobs
RUN git clone --depth=1 https://github.com/astral-sh/uv
RUN cd uv && cargo test --no-default-features --features=python,pypi
```

---

_Comment by @charliermarsh on 2025-02-05 16:33_

Yeah this is plenty. Thanks.

---

_Comment by @charliermarsh on 2025-02-05 17:58_

Just to confirm, do you disable the use of the `python-build-standalone` Pythons in the test suite?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-05 19:13_

---

_Closed by @charliermarsh on 2025-02-05 20:47_

---

_Closed by @charliermarsh on 2025-02-05 20:47_

---

_Comment by @mgorny on 2025-02-06 04:01_

> Just to confirm, do you disable the use of the `python-build-standalone` Pythons in the test suite?

Yes, we run on the system Pythons.

Thanks for the prompt fix! Unfortunately, I couldn't test it during the morning work, but I'll confirm the fix later.

---

_Comment by @mgorny on 2025-02-06 12:41_

Confirmed working. Thanks!

---
