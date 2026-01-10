```yaml
number: 15843
title: "Avoid ANSI codes in debug! messages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ansi
created_at: 2025-09-14T18:52:11Z
updated_at: 2025-09-17T14:30:44Z
url: https://github.com/astral-sh/uv/pull/15843
synced_at: 2026-01-10T06:36:15Z
```

# Avoid ANSI codes in debug! messages

---

_Pull request opened by @charliermarsh on 2025-09-14 18:52_

## Summary

I spent time trying to figure out how to support this but came up empty. It _seems_ like maybe the `DefaultFields` implementation in `tracing-subscriber` uses debug formatting for fields...? So if you have a string with ANSI codes, they end up printing as unformatted values? I even reverted all our custom formatting in `logging.rs` and saw the same thing.

Closes https://github.com/astral-sh/uv/issues/15840.


---

_Label `bug` added by @charliermarsh on 2025-09-14 18:52_

---

_Marked ready for review by @charliermarsh on 2025-09-14 18:52_

---

_Comment by @charliermarsh on 2025-09-14 19:24_

There's currently no way to avoid this escaping, AFAICT: https://github.com/tokio-rs/tracing/issues/3369.

---

_@konstin approved on 2025-09-15 07:27_

---

_Comment by @konstin on 2025-09-15 07:27_

I asked upstream to not strip color codes and/or give us more control: https://github.com/tokio-rs/tracing/issues/3378

---

_Comment by @charliermarsh on 2025-09-15 14:09_

There are a lot of other logs we'd need to change here unfortunately, grr...

---

_Comment by @charliermarsh on 2025-09-17 14:19_

I'm going to merge this since it's an improvement over the status quo. I'll open a separate issue to track re-enabling.

---

_Merged by @charliermarsh on 2025-09-17 14:30_

---

_Closed by @charliermarsh on 2025-09-17 14:30_

---

_Branch deleted on 2025-09-17 14:30_

---
