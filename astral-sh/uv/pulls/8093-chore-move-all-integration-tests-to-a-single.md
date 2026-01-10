```yaml
number: 8093
title: "chore: Move all integration tests to a single binary"
type: pull_request
state: merged
author: fasterthanlime
labels:
  - testing
assignees: []
merged: true
base: main
head: single-it-binary
created_at: 2024-10-10T14:18:05Z
updated_at: 2024-10-11T14:41:39Z
url: https://github.com/astral-sh/uv/pull/8093
synced_at: 2026-01-10T12:54:02Z
```

# chore: Move all integration tests to a single binary

---

_Pull request opened by @fasterthanlime on 2024-10-10 14:18_

As per https://matklad.github.io/2021/02/27/delete-cargo-integration-tests.html

Before that, there were 91 separate integration tests binary.

(As discussed on Discord — I've done the `uv` crate, there's still a few more commits coming before this is mergeable, and I want to see how it performs in CI and locally).

---

_Comment by @fasterthanlime on 2024-10-10 15:24_

This is almost ready, I'm taking a look at `scripts/sync_scenarios.sh`

---

_Comment by @fasterthanlime on 2024-10-11 12:10_

This is ready for merge cc @charliermarsh — the only remaining CI failure is an ioctl failure at the "Add password to keyring" step of the uv publish integration test, which.. I may have broken but I don't see how!

<details>
<summary>Click for a nice trace</summary>

<pre><code>
Installed 1 executable: keyring
/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/getpass.py:91: GetPassWarning: Can not control echo on the terminal.
  passwd = fallback_getpass(prompt, stream)
Warning: Password input may be echoed.
Password for '__token__' in 'https://test.pypi.org/legacy/?astral-test-keyring': Traceback (most recent call last):
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/getpass.py", line 69, in unix_getpass
    old = termios.tcgetattr(fd)     # a copy to save
          ^^^^^^^^^^^^^^^^^^^^^
termios.error: (25, 'Inappropriate ioctl for device')

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/runner/.local/bin/keyring", line 8, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/runner/.local/share/uv/tools/keyring/lib/python3.12/site-packages/keyring/cli.py", line 212, in main
    return cli.run(argv)
           ^^^^^^^^^^^^^
  File "/home/runner/.local/share/uv/tools/keyring/lib/python3.12/site-packages/keyring/cli.py", line 111, in run
    return method()
           ^^^^^^^^
  File "/home/runner/.local/share/uv/tools/keyring/lib/python3.12/site-packages/keyring/cli.py", line 147, in do_set
    password = self.input_password(
               ^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.local/share/uv/tools/keyring/lib/python3.12/site-packages/keyring/cli.py", line 184, in input_password
    return self.pass_from_pipe() or getpass.getpass(prompt)
                                    ^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/getpass.py", line 91, in unix_getpass
    passwd = fallback_getpass(prompt, stream)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/getpass.py", line 126, in fallback_getpass
    return _raw_input(prompt, stream)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/getpass.py", line 148, in _raw_input
    raise EOFError
EOFError
</code></pre>
</details>

---

_Comment by @charliermarsh on 2024-10-11 12:49_

@fasterthanlime -- Sweet! Do you have a sense for the impact of the change (a rough benchmark or similar)?

---

_Comment by @fasterthanlime on 2024-10-11 14:29_

> Sweet! Do you have a sense for the impact of the change (a rough benchmark or similar)?

Like all the stuff I've done so far on uv, the answer is "it's complicated". But with @konstin we came up with a test I can at least report on.

On the current main branch, editing the `decode_token` function in `crates/uv/tests/common/mod.rs` to add/remove a println and doing `time cargo nextest run branching_urls_of_different_sources_disjoint` shows this:

```shell
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 10.46s
    Starting 1 test across 91 binaries (1746 skipped; run ID: c607bd8c-63eb-4294-8604-e9b6c46922b6, nextest profile: default)
        PASS [   7.071s] uv::branching_urls branching_urls_of_different_sources_disjoint
------------
     Summary [   7.072s] 1 test run: 1 passed, 1746 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  47.57s user 19.49s system 185% cpu 36.068 total

❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 13.72s
    Starting 1 test across 91 binaries (1746 skipped; run ID: 64d9152c-d8fd-4bcd-a080-e72e8ce3bfa0, nextest profile: default)
        PASS [   5.362s] uv::branching_urls branching_urls_of_different_sources_disjoint
------------
     Summary [   5.365s] 1 test run: 1 passed, 1746 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  49.72s user 21.38s system 188% cpu 37.615 total

❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 10.49s
    Starting 1 test across 91 binaries (1746 skipped; run ID: 44f2360a-acae-4515-b069-b7f054a6d569, nextest profile: default)
        PASS [   4.384s] uv::branching_urls branching_urls_of_different_sources_disjoint
------------
     Summary [   4.386s] 1 test run: 1 passed, 1746 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  46.96s user 19.27s system 193% cpu 34.147 total
```

there's a _very long pause_ before "Starting 1 test across 91 binaries" (that I think is caused by macOS gatekeeper) — the average iteration time here is 35 seconds (see time's output at the very end)

---

By comparison, here's the same thing after this PR:

```shell
uv on  single-it-binary via 🐳 orbstack is 📦 v0.4.20 via 🐍 v3.9.6 via 🦀 v1.81.0 took 5s
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.19s
    Starting 1 test across 50 binaries (1751 skipped; run ID: aa1c9fb5-3846-4979-b0fe-ec7ec5113df4, nextest profile: default)
        PASS [   3.917s] uv::it branching_urls::branching_urls_of_different_sources_disjoint
------------
     Summary [   3.917s] 1 test run: 1 passed, 1751 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  1.62s user 0.78s system 53% cpu 4.504 total

uv on  single-it-binary via 🐳 orbstack is 📦 v0.4.20 via 🐍 v3.9.6 via 🦀 v1.81.0 took 4s
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 9.19s
    Starting 1 test across 50 binaries (1751 skipped; run ID: 212762f0-1ba4-41c0-a003-5507e3d4172f, nextest profile: default)
        PASS [   4.731s] uv::it branching_urls::branching_urls_of_different_sources_disjoint
------------
     Summary [   4.733s] 1 test run: 1 passed, 1751 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  15.09s user 2.14s system 115% cpu 14.900 total

uv on  single-it-binary [!] via 🐳 orbstack is 📦 v0.4.20 via 🐍 v3.9.6 via 🦀 v1.81.0 took 14s
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 3.95s
    Starting 1 test across 50 binaries (1751 skipped; run ID: 906fa26d-7107-4d4b-9766-0212fd1fcd01, nextest profile: default)
        PASS [   4.161s] uv::it branching_urls::branching_urls_of_different_sources_disjoint
------------
     Summary [   4.162s] 1 test run: 1 passed, 1751 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  4.08s user 2.05s system 67% cpu 9.087 total

uv on  single-it-binary [!] via 🐳 orbstack is 📦 v0.4.20 via 🐍 v3.9.6 via 🦀 v1.81.0 took 9s
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 4.15s
    Starting 1 test across 50 binaries (1751 skipped; run ID: 8eea830b-ebee-46ef-ac0e-12ed9b6e186d, nextest profile: default)
        PASS [   4.396s] uv::it branching_urls::branching_urls_of_different_sources_disjoint
------------
     Summary [   4.397s] 1 test run: 1 passed, 1751 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  4.10s user 2.01s system 64% cpu 9.521 total

uv on  single-it-binary [!] via 🐳 orbstack is 📦 v0.4.20 via 🐍 v3.9.6 via 🦀 v1.81.0 took 9s
❯ time cargo nextest run branching_urls_of_different_sources_disjoint
   Compiling uv v0.4.20 (/Users/amos/bearcove/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 3.55s
    Starting 1 test across 50 binaries (1751 skipped; run ID: 6d83d39b-b7a7-4a8f-b475-57fa54a3803b, nextest profile: default)
        PASS [   4.117s] uv::it branching_urls::branching_urls_of_different_sources_disjoint
------------
     Summary [   4.117s] 1 test run: 1 passed, 1751 skipped
cargo nextest run branching_urls_of_different_sources_disjoint  4.06s user 1.84s system 68% cpu 8.659 total
```

The first one is a bit of an outlier but I included it anyway 🤷 

Notice that the build time is consistently ~4s, there's no pause between "building the tests" and "running them" (I expect this to also be a big factor on Windows) — the total duration ranges from 9-14s (the wi-fi is not super reliable here).

Of course that's just _one_ workflow, I imagine you don't change the common code a lot, but it avoids it being compiled ~40 times every time it changes. 

---

Another advantage of this change is that running groups of tests is much easier.

Before: `branching_urls` is not a package name, or the name of a test, so the default filtering in nextest doesn't find it — only tests _with_ branching_urls in the title:

```shell
❯ cargo nextest list branching_urls
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.15s
uv::branching_urls:
    branching_urls_disjoint
    branching_urls_of_different_sources_conflict
    branching_urls_of_different_sources_disjoint
    branching_urls_overlapping
```

To find those you need to know cargo nextest's filtering syntax:

```shell
❯ cargo nextest list -E 'binary(branching_urls)'
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.15s
uv::branching_urls:
    branching_between_registry_and_direct_url
    branching_urls_disjoint
    branching_urls_of_different_sources_conflict
    branching_urls_of_different_sources_disjoint
    branching_urls_overlapping
    dont_pre_visit_url_packages
    root_package_splits_but_transitive_conflict
    root_package_splits_other_dependencies_too
    root_package_splits_transitive_too
```

Or:

```shell
❯ cargo nextest list -E 'binary_id(uv::branching_urls)'
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.15s
uv::branching_urls:
    branching_between_registry_and_direct_url
    branching_urls_disjoint
    branching_urls_of_different_sources_conflict
    branching_urls_of_different_sources_disjoint
    branching_urls_overlapping
    dont_pre_visit_url_packages
    root_package_splits_but_transitive_conflict
    root_package_splits_other_dependencies_too
    root_package_splits_transitive_too
```

After:

```shell
❯ cargo nextest list branching_urls
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.17s
uv::it:
    branching_urls::branching_between_registry_and_direct_url
    branching_urls::branching_urls_disjoint
    branching_urls::branching_urls_of_different_sources_conflict
    branching_urls::branching_urls_of_different_sources_disjoint
    branching_urls::branching_urls_overlapping
    branching_urls::dont_pre_visit_url_packages
    branching_urls::root_package_splits_but_transitive_conflict
    branching_urls::root_package_splits_other_dependencies_too
    branching_urls::root_package_splits_transitive_too
```

---

_Comment by @charliermarsh on 2024-10-11 14:36_

That's great -- thanks for writing it up!

---

_@charliermarsh approved on 2024-10-11 14:37_

---

_Merged by @charliermarsh on 2024-10-11 14:41_

---

_Closed by @charliermarsh on 2024-10-11 14:41_

---

_Label `testing` added by @charliermarsh on 2024-10-11 14:41_

---
