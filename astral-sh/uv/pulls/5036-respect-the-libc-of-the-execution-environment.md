```yaml
number: 5036
title: "Respect the libc of the execution environment with `uv python list`"
type: pull_request
state: merged
author: Di-Is
labels: []
assignees: []
merged: true
base: main
head: fix-python-list
created_at: 2024-07-13T14:31:25Z
updated_at: 2024-07-19T06:29:50Z
url: https://github.com/astral-sh/uv/pull/5036
synced_at: 2026-01-10T13:42:52Z
```

# Respect the libc of the execution environment with `uv python list`

---

_Pull request opened by @Di-Is on 2024-07-13 14:31_

Fix #4988

## Summary

Running `uv python list` on glibc-based Linux will list musl pythons.

```bash
$ uv version
uv 0.2.24
$ uv python list
warning: `uv python list` is experimental and may change without warning.
cpython-3.12.3-linux-x86_64-musl     <download available>
cpython-3.12.3-linux-x86_64-gnu      /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu      /bin/python3
cpython-3.11.9-linux-x86_64-musl     <download available>
cpython-3.10.14-linux-x86_64-musl    <download available>
cpython-3.9.19-linux-x86_64-musl     <download available>
cpython-3.8.19-linux-x86_64-musl     <download available>
cpython-3.7.9-linux-x86_64-musl      <download available>
```

Change it to show Python matching the environment's libc as follows.

```bash
$ uv python list
warning: `uv python list` is experimental and may change without warning.
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu     /bin/python3
cpython-3.12.3-linux-x86_64-gnu     <download available>
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     <download available>
cpython-3.7.9-linux-x86_64-gnu      <download available>
```

Also, if --all-platforms is specified, change to list Python for all architectures and libc.

```bash
$ uv python list --all-platforms
warning: `uv python list` is experimental and may change without warning.
cpython-3.12.3-windows-x86_64-none       <download available>
cpython-3.12.3-windows-x86-none          <download available>
cpython-3.12.3-macos-x86_64-none         <download available>
cpython-3.12.3-macos-aarch64-none        <download available>
cpython-3.12.3-linux-x86_64-musl         <download available>
cpython-3.12.3-linux-x86_64-gnu          /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu          /bin/python3
cpython-3.12.3-linux-x86_64-gnu          <download available>
cpython-3.12.3-linux-s390x-gnu           <download available>
cpython-3.12.3-linux-powerpc64le-gnu     <download available>
cpython-3.12.3-linux-armv7-gnueabihf     <download available>
cpython-3.12.3-linux-armv7-gnueabi       <download available>
cpython-3.12.3-linux-aarch64-gnu         <download available>
...
```

## Test Plan

The following commands were executed on the command line to confirm the results in Ubuntu 24.04.
- `cargo run python list`
- `cargo run python list --all-platforms`

---

_Renamed from "Fix python list" to "Respect the libc of the execution environment with `uv python list`." by @Di-Is on 2024-07-13 14:35_

---

_Comment by @zanieb on 2024-07-13 15:00_

Thank you! Note we don't have proper musl detection yet (see https://github.com/astral-sh/uv/pull/4160 and https://github.com/astral-sh/uv/issues/4242)

---

_Assigned to @zanieb by @charliermarsh on 2024-07-13 16:38_

---

_Renamed from "Respect the libc of the execution environment with `uv python list`." to "Respect the libc of the execution environment with `uv python list`" by @Di-Is on 2024-07-14 10:44_

---

_Comment by @Di-Is on 2024-07-14 11:46_

Hi @zanieb!

I see, the function to detect the type of libc has not been implemented yet.
Since various conflicts are likely to occur, we will not make any changes to this MR for now.

I hope that the problem reported in #4988 will be fixed somehow.

---

_@zanieb approved on 2024-07-14 16:14_

I think this is an improvement regardless of our detection abilities, thank you!

---

_Merged by @zanieb on 2024-07-14 16:14_

---

_Closed by @zanieb on 2024-07-14 16:14_

---

_Branch deleted on 2024-07-19 06:29_

---
