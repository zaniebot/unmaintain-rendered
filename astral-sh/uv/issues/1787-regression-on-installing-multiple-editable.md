---
number: 1787
title: Regression on installing multiple editable dependencies
type: issue
state: closed
author: hsheth2
labels:
  - bug
assignees: []
created_at: 2024-02-20T23:30:35Z
updated_at: 2024-02-22T05:00:04Z
url: https://github.com/astral-sh/uv/issues/1787
synced_at: 2026-01-10T01:23:08Z
---

# Regression on installing multiple editable dependencies

---

_Issue opened by @hsheth2 on 2024-02-20 23:30_

Installing multiple editable versions causes a panic. This used to work with uv 0.1.5, and works with `pip`.

```shell
$ uv --version
uv 0.1.6
$ uname -mo
Darwin x86_64
$ RUST_BACKTRACE=1 uv pip install -e '/Users/hsheth/projects/datahub/metadata-ingestion/' -e '/Users/hsheth/projects/datahub/metadata-ingestion-modules/airflow-plugin/[plugin-v2]'
   Built file:///Users/hsheth/projects/datahub/metadata-ingestion
   Built file:///Users/hsheth/projects/datahub/metadata-ingestion-modules/airflow-plugin                                                                                              
Built 2 editables in 4.54s
⠹ pycparser==2.21                                                                                                                                                                     
thread 'main' panicked at crates/uv-resolver/src/resolution.rs:125:37:
Every package should be pinned: PackageName("acryl-datahub-airflow-plugin")
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

See https://github.com/datahub-project/datahub/pull/9885 for a live example, or you can also repro by cloning the https://github.com/datahub-project/datahub repo and installing these two paths.



---

_Referenced in [datahub-project/datahub#9885](../../datahub-project/datahub/pulls/9885.md) on 2024-02-20 23:30_

---

_Label `bug` added by @charliermarsh on 2024-02-20 23:35_

---

_Comment by @charliermarsh on 2024-02-20 23:36_

I believe I know the problem — thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 23:38_

---

_Comment by @charliermarsh on 2024-02-20 23:45_

Is there any relationship between the two? Do they depend on one another? Are there any extras involved?

---

_Comment by @hsheth2 on 2024-02-21 00:02_

The base `acryl-datahub-airflow-plugin` package depends on `acryl-datahub`. The extra is only adding a few other dependencies, but doesn't impact the relationship between the two editable packages.

---

_Comment by @charliermarsh on 2024-02-21 00:03_

Perfect, thanks.

---

_Comment by @charliermarsh on 2024-02-21 00:03_

I’ll make sure it’s fixed by the next release.

---

_Comment by @charliermarsh on 2024-02-21 00:16_

Ugh, sadly the comment you pasted above works for me on `main` when run on that repo.

---

_Comment by @charliermarsh on 2024-02-21 00:18_

I'm trying to reproduce. What exact commit did you run this against, in the datahub repo?

---

_Comment by @hsheth2 on 2024-02-21 00:34_

The CI failure is here https://github.com/datahub-project/datahub/actions/runs/7981141634/job/21792271496?pr=9885. Because we're using the default checkout action, CI ran on a commit that merged that PR into master

That said, I am trying the same commands again locally and it also seems to work for me. I might need to keep digging here

---

_Comment by @hsheth2 on 2024-02-21 00:59_

This seems to reproduce the problem:

```sh
git clone https://github.com/datahub-project/datahub
cd datahub/metadata-ingestion-modules/airflow-plugin
uv venv venv
source venv/bin/activate
uv pip install --upgrade pip uv wheel 'setuptools>=63.0.0'
VIRTUAL_ENV=venv venv/bin/uv pip install -e ../../metadata-ingestion -e '.[ignore]'
```

Note that `VIRTUAL_ENV=venv venv/bin/uv pip install -e ../../metadata-ingestion -e '.'` works, and once that's completed, the original `VIRTUAL_ENV=venv venv/bin/uv pip install -e ../../metadata-ingestion -e '.[ignore]'` command works fine too, but installing new extras e.g. `plugin-v2` fails again. 

The `ignore` extra doesn't actually exist, and is only used to make it easy to append things without needing to conditionally add a comma e.g. `ignore,plugin-v2`.

The other thing that I didn't mention - the `plugin-v2` extra of `acryl-datahub-airflow-plugin` depends on `acryl-datahub[sql-parser]`

---

_Comment by @hsheth2 on 2024-02-21 19:44_

Seems like the issue shows up when using `-e` in conjunction with an extra that doesn't exist. I don't think the fact that there's multiple `-e` directives is related.

I've been able to work around it for now by declaring the `ignore` extra.

---

_Comment by @charliermarsh on 2024-02-21 19:47_

Thanks -- I'll try to get back to this today.

---

_Comment by @charliermarsh on 2024-02-22 02:26_

Thanks, this was my mistake. Just a bug.

---

_Comment by @charliermarsh on 2024-02-22 02:27_

(The bug was in the error reporting path for editable installs with non-existent extras.)

---

_Referenced in [astral-sh/uv#1847](../../astral-sh/uv/pulls/1847.md) on 2024-02-22 02:29_

---

_Closed by @charliermarsh on 2024-02-22 02:55_

---

_Comment by @charliermarsh on 2024-02-22 05:00_

Should be fixed in v0.1.7 (out now).

---
