```yaml
number: 6205
title: switch to jiff from chrono
type: pull_request
state: merged
author: BurntSushi
labels:
  - breaking
assignees: []
merged: true
base: release/3
head: ag/jiff
created_at: 2024-08-19T13:58:21Z
updated_at: 2024-08-19T17:36:01Z
url: https://github.com/astral-sh/uv/pull/6205
synced_at: 2026-01-12T16:07:16Z
```

# switch to jiff from chrono

---

_@BurntSushi_

This PR migrates uv's use of `chrono` to `jiff`.

I did most of this work a while back as one of my tests to ensure Jiff
could actually be used in a real world project. I decided to revive
this because I noticed that `reqwest-retry` dropped its Chrono dependency,
which is I believe the only other thing requiring Chrono in uv.
(Although, we use a fork of `reqwest-middleware` at present, and that
hasn't been updated to latest upstream yet. I wasn't quite sure of the
process we have for that.)

In course of doing this, I actually made two changes to uv:

First is that the lock file now writes an RFC 3339 timestamp for
`exclude-newer`. Previously, we were using Chrono's `Display`
implementation for this which is a non-standard but "human readable"
format. I think the right thing to do here is an RFC 3339 timestamp.

Second is that, in addition to an RFC 3339 timestamp, `--exclude-newer`
used to accept a "UTC date." But this PR changes it to a "local date."
That is, a date in the user's system configured time zone. I think
this makes more sense than a UTC date, but one alternative is to drop
support for a date and just rely on an RFC 3339 timestamp. The main
motivation here is that automatically assuming UTC is often somewhat
confusing, since just writing an unqualified date like `2024-08-19` is
often assumed to be interpreted relative to the writer's "local" time.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-19 13:58_

---

_Review requested from @konstin by @BurntSushi on 2024-08-19 13:58_

---

_@charliermarsh approved on 2024-08-19 14:01_

---

_Review requested from @zanieb by @BurntSushi on 2024-08-19 14:02_

---

_Comment by @zanieb on 2024-08-19 14:05_

Since e change interpretation of exclude newer times this is "breaking", right?

---

_Comment by @BurntSushi on 2024-08-19 14:06_

> Since e change interpretation of exclude newer times this is "breaking", right?

Yup.

---

_Label `breaking` added by @BurntSushi on 2024-08-19 14:06_

---

_Comment by @BurntSushi on 2024-08-19 14:08_

This is also breaking because uv won't parse old lock files either, since the datetime formats won't match.

---

_Comment by @zanieb on 2024-08-19 14:11_

Let's just group it into 0.3.0, we might start merging things for that today? I'll let you know.

---

_Comment by @BurntSushi on 2024-08-19 14:28_

OK, with #6206 merged, Chrono is now removed from `uv`'s dependency tree:

```
$ cargo tree -e normal | rg chrono
$
```

`chrono` does still appear in the lock file, but I think that's from
`axoupdater->homedir->wmi->chrono`.

> Let's just group it into 0.3.0, we might start merging things for that today? I'll let you know.

SGTM.

---

_Merged by @zanieb on 2024-08-19 17:36_

---

_Closed by @zanieb on 2024-08-19 17:36_

---

_Branch deleted on 2024-08-19 17:36_

---
