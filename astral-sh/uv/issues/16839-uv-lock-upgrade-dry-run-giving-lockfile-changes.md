---
number: 16839
title: "`uv lock --upgrade --dry-run` giving \"Lockfile changes detected\" despite no changes"
type: issue
state: open
author: shenanigansd
labels:
  - bug
assignees: []
created_at: 2025-11-24T20:54:07Z
updated_at: 2025-12-19T15:46:54Z
url: https://github.com/astral-sh/uv/issues/16839
synced_at: 2026-01-10T01:26:10Z
---

# `uv lock --upgrade --dry-run` giving "Lockfile changes detected" despite no changes

---

_Issue opened by @shenanigansd on 2025-11-24 20:54_

### Summary

Running `uv lock --upgrade --dry-run` is showing "**Lockfile changes detected**", but running without the `--dry-run` flag makes no changes to the lockfile.

```
@shenanigansd ➜ /workspaces/core (main) $ uv lock --upgrade --dry-run
Resolved 156 packages in 7.28s
Lockfile changes detected
@shenanigansd ➜ /workspaces/core (main) $ uv lock --upgrade 
Resolved 156 packages in 295ms
@shenanigansd ➜ /workspaces/core (main) $ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
@shenanigansd ➜ /workspaces/core (main) $ 
```

Dry run logs with `-vvv`: https://gist.github.com/shenanigansd/786852dc4cbd8f549ad7f8387edab568

### Platform

Linux 6.8.0-1030-azure x86_64 GNU/Linux

### Version

uv 0.9.11

### Python version

Python 3.14.0

---

_Label `bug` added by @shenanigansd on 2025-11-24 20:54_

---

_Assigned to @EliteTK by @EliteTK on 2025-12-18 16:31_

---

_Comment by @EliteTK on 2025-12-19 15:46_

So when we read the original lockfile, we "complexify" the marker trees to include the python version. But the resolution from the resolver (`ResolverOutput::fork_markers`) doesn't include the python version in its marker trees, so when we [check to see if the new `Lock` is different from the previous](https://github.com/astral-sh/uv/blob/1d9672c11c8ec008b27fe158897173c8f1f0cac3/crates/uv/src/commands/project/lock.rs#L984), we find that it is, but only because of that discrepancy.

You can reproduce this with:

```
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["markupsafe<2 ; sys_platform != 'win32'", "markupsafe==2.0.0 ; sys_platform == 'win32'"]
```

Which is directly taken from the `lock::lock_upgrade_log_multi_version` integration test.

For an example of the difference between what we read (or well, what we "complexified" into after reading), and what we produce, see this gist:

https://gist.github.com/EliteTK/6e9824f46fb49fe8f803fb3670b0994c/revisions

---

As to solving this, we could stop complexifying the existing lockfile. Since we produce one without that data. But I haven't checked if this would break things.

Alternatively, the resolver could output complex markers, which probably shouldn't break anything as it's strictly superfluous information.

---
