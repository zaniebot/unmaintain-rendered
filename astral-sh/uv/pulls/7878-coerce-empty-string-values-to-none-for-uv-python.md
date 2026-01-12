```yaml
number: 7878
title: "Coerce empty string values to `None` for `UV_PYTHON` env var"
type: pull_request
state: merged
author: krishnan-chandra
labels:
  - configuration
assignees: []
merged: true
base: main
head: fix-empty-uv-python-var
created_at: 2024-10-02T19:11:05Z
updated_at: 2024-10-05T01:16:40Z
url: https://github.com/astral-sh/uv/pull/7878
synced_at: 2026-01-12T16:08:03Z
```

# Coerce empty string values to `None` for `UV_PYTHON` env var

---

_@krishnan-chandra_

## Summary

Closes #7841. If there are other env vars that would also benefit from this value parser, please let me know and I can add them to this PR.

## Test Plan

When running the same example from the linked issue, it now works:

```
UV_PYTHON= cargo run -- init x
   Compiling ...
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 29.06s
     Running `target/debug/uv init x`
Initialized project `x` at `/Users/krishnanchandra/Projects/uv/x`
```


---

_Comment by @charliermarsh on 2024-10-03 12:02_

It looks like this will error. Do we want it to error, or is it supposed to "unset" the env var? Do you know what other tools do?

---

_Comment by @zanieb on 2024-10-03 14:37_

I think we should silently ignore the variable if it's empty, generally. I am curious what other tools do though.

---

_Comment by @krishnan-chandra on 2024-10-03 15:52_

Good question. I don't know what other tools do here but I think it feels reasonable to coerce empty strings to `None` rather than having them error out - I'm happy to make that change here.

---

_Comment by @charliermarsh on 2024-10-03 16:03_

We have this pattern for `UV_INDEX_URL` -- we wrap it in a `Maybe` type, where it's `Maybe::None` when empty. It has to be distinct from `Option` because Clap has special handling for Option.

---

_Comment by @krishnan-chandra on 2024-10-03 21:27_

I implemented your suggestion re: `Maybe` - I think I did this properly but let me know if I didn't.

---

_Renamed from "Use `clap::builder::NonEmptyStringValueParser` for `UV_PYTHON` env var" to "Coerce empty string values to `None` for `UV_PYTHON` env var" by @krishnan-chandra on 2024-10-03 21:39_

---

_@charliermarsh approved on 2024-10-04 11:31_

---

_Merged by @charliermarsh on 2024-10-04 11:32_

---

_Closed by @charliermarsh on 2024-10-04 11:32_

---

_Comment by @charliermarsh on 2024-10-04 11:32_

Thanks, that's great!

---

_Label `configuration` added by @charliermarsh on 2024-10-04 11:32_

---

_Branch deleted on 2024-10-05 01:16_

---
