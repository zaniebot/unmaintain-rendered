```yaml
number: 13385
title: "`UV_FROZEN=0 uv lock --check` fails"
type: issue
state: closed
author: NellyWhads
labels:
  - bug
assignees: []
created_at: 2025-05-11T03:14:48Z
updated_at: 2026-01-06T20:43:06Z
url: https://github.com/astral-sh/uv/issues/13385
synced_at: 2026-01-10T03:11:34Z
```

# `UV_FROZEN=0 uv lock --check` fails

---

_Issue opened by @NellyWhads on 2025-05-11 03:14_

### Summary

The environment variable resolution, for good reason, considers `--frozen` (`UV_FROZEN=1`) and `--check` (`UV_LOCKED=1`) [to be conflicting.](https://github.com/astral-sh/uv/blob/e70cf25ea72c94502d08637474e9e4ad576bf342/crates/uv-cli/src/lib.rs#L3278)

However, the `false`/`0` values should not cause conflicts.

For example, if in a managed repository, `UV_FROZEN=1` is set up in the development environment to prevent accidental automatic re-locked updates, a linter should be able to use the command as titled.

A current work-around is to instead use `env -u UV_FROZEN uv lock --check`, however, the conflicting `false`/`0` behavior is unintuitive.

### Platform

MacOS 14, Ubuntu 22.04

### Version

tested with >= 0.6.13

### Python version

3.8 through 3.12

---

_Label `bug` added by @NellyWhads on 2025-05-11 03:14_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-05-12 10:27_

---

_Comment by @heiner on 2025-05-12 22:42_

I can confirm this is still an issue

```
$ UV_FROZEN=1 uv lock --locked
error: the argument '--check' cannot be used with '--check-exists'

Usage: uv lock --check

For more information, try '--help'.
$ UV_FROZEN=0 uv lock --locked
error: the argument '--check' cannot be used with '--check-exists'

Usage: uv lock --check

For more information, try '--help'.
$ uv --version
uv 0.7.3
```

---

_Comment by @zanieb on 2025-05-12 23:00_

This has the same cause as https://github.com/astral-sh/uv/issues/13316

---

_Comment by @maxmynter on 2025-05-13 04:19_

I did a little digging into this. The exclusivity is enforced by `Clap`.
https://github.com/astral-sh/uv/blob/3b8139526a8dc1f3bfd003b9ef215bd148025ce6/crates/uv-cli/src/lib.rs#L3378-L3386

From what I found in the [Clap docs](https://docs.rs/clap/latest/clap/struct.Arg.html#method.conflicts_with), it's not possible to declare conflicts on runtime values without custom validation logic.
Alternatively, one could handle the values earlier in the lifecycle before they are passed in `LockSettings::resolve`.

If either is desired, let me know, I'm happy to suggest a PR. 

---

_Comment by @konstin on 2025-05-13 07:13_

Upstream bug: https://github.com/clap-rs/clap/issues/5591

---

_Unassigned @jtfmumm by @konstin on 2025-09-12 13:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 21:00_

---

_Closed by @charliermarsh on 2026-01-06 20:43_

---
