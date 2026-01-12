```yaml
number: 2794
title: "failing test: f2361_sort_nested_files"
type: issue
state: closed
author: oconnor663
labels:
  - rollup
assignees: []
created_at: 2024-04-30T23:53:02Z
updated_at: 2025-10-11T02:07:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2794
synced_at: 2026-01-12T16:13:24Z
```

# failing test: f2361_sort_nested_files

---

_@oconnor663_

### What version of ripgrep are you using?

master (bb8601b2bafb5e68181cbbb84e6ffa4f7a72bf16)

### What operating system are you using ripgrep on?

CentOS Stream 9, with an XFS root filesystem

### Describe your bug.

This doesn't repro every time, maybe 1/5 or 1/10.

```
$ git clone https://github.com/BurntSushi/ripgrep
...
$ cd ripgrep
$ cargo test f2361_sort_nested_files
...
test feature::f2361_sort_nested_files ... FAILED

failures:

---- feature::f2361_sort_nested_files stdout ----
thread 'feature::f2361_sort_nested_files' panicked at tests/feature.rs:952:5:

printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
foo
dir/bar

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
dir/bar
foo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    feature::f2361_sort_nested_files
```

---

_Comment by @sukhmel on 2024-08-12 13:14_

Same here when building on MacOS with Nix, ripgrep version 14.1.0

> error: test failed, to rerun pass `--test integration`
> error: builder for '/nix/store/k6djbylmb9fdz4ss0dxszyx8qr9bhmcd-ripgrep-14.1.0.drv' failed with exit code 101
>
> thread 'feature::f2361_sort_nested_files' panicked at tests/feature.rs:960:5

output is otherwise same. The second attempt was successful, so maybe a flaky test?

---

_Label `rollup` added by @BurntSushi on 2025-10-11 01:37_

---

_Closed by @BurntSushi on 2025-10-11 02:07_

---
