```yaml
number: 15458
title: "Move `uv format` unformatted test file into const"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - internal
  - testing
assignees: []
created_at: 2025-08-22T16:26:02Z
updated_at: 2025-08-25T12:40:21Z
url: https://github.com/astral-sh/uv/issues/15458
synced_at: 2026-01-10T03:23:54Z
```

# Move `uv format` unformatted test file into const

---

_Issue opened by @zanieb on 2025-08-22 16:26_

> There are a lot of duplicated code among these tests (mainly, the same `pyproject.toml`, and the formatted/unformatted code snippets). I preferred to maintain the previous style and copypaste the mock data. I'm not sure what approach do you usually follow here (to extract into constants/fixtures vs always copypaste)

_Originally posted by @jorgehermo9 in https://github.com/astral-sh/uv/pull/15438#discussion_r2292327872_
            

---

_Comment by @zanieb on 2025-08-22 16:26_

I'd probably focus on the repeated code snippets more than the `pyproject.toml` repetition.

---

_Label `good first issue` added by @zanieb on 2025-08-22 16:26_

---

_Label `internal` added by @zanieb on 2025-08-22 16:26_

---

_Label `testing` added by @zanieb on 2025-08-22 16:26_

---

_Comment by @pposca on 2025-08-22 20:57_

It seems if you use assert_snapshot!(reference_value, @"snapshot") to compare 2 values, you have to use a @"string literal", so you cannot use a const or var there (at least, I couldn't find the way):

https://github.com/astral-sh/uv/blob/e6def431107dabf0a2339be5687d318631f9f503/crates/uv/tests/it/format.rs#L43C5-L53C9

Other option is to create 2 or 3 auxiliary functions like create_unformated_file, check_file_formated and check_file_not formated. Is that something you like?

---

_Comment by @zanieb on 2025-08-22 21:53_

I don't think we want to replace the snapshot with a string literal, just the file we're writing for testing?

We could also just simplify the file dramatically, e.g., `x      = 1` so there's less noise.

---

_Comment by @pposca on 2025-08-23 09:38_

> Hey. Can i take this one? Please assign

Sorry, I'm about to send a PR.

---

_Comment by @pposca on 2025-08-23 09:59_

@zanieb , I find tests clearer like this, but I'm not sure if this is what you had in mind.

---

_Comment by @jorgehermo9 on 2025-08-23 11:05_

I really like what you did to simplify them @pposca 

---

_Closed by @zanieb on 2025-08-25 12:40_

---
