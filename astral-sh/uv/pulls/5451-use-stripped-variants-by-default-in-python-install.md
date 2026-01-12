```yaml
number: 5451
title: Use stripped variants by default in Python install
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/strip
created_at: 2024-07-25T17:20:52Z
updated_at: 2024-07-29T01:47:46Z
url: https://github.com/astral-sh/uv/pull/5451
synced_at: 2026-01-12T16:06:49Z
```

# Use stripped variants by default in Python install

---

_@charliermarsh_

## Summary

Will file a separate ticket to support `--debug`.

Closes https://github.com/astral-sh/uv/issues/5447.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-25 17:20_

---

_Label `enhancement` added by @charliermarsh on 2024-07-25 17:20_

---

_Label `preview` added by @charliermarsh on 2024-07-25 17:20_

---

_Marked ready for review by @charliermarsh on 2024-07-25 17:21_

---

_@zanieb approved on 2024-07-25 17:24_

---

_Comment by @charliermarsh on 2024-07-25 17:27_

They're no smaller on macOS, but like 40% smaller on Windows and 10-20% smaller on Linux IIRC.

---

_Merged by @charliermarsh on 2024-07-25 17:29_

---

_Closed by @charliermarsh on 2024-07-25 17:29_

---

_Branch deleted on 2024-07-25 17:29_

---

_Comment by @bulletmark on 2024-07-29 01:16_

> They're no smaller on macOS, but like 40% smaller on Windows and 10-20% smaller on Linux IIRC.

@charliermarsh , just to correct your understanding - For the latest (3.12.4) python for the most popular linux distribution (x86_64-unknown-linux-gnu), `install_only` download is 238M, and `install_only_stripped` is 82M, i.e. 66% smaller!

---

_Comment by @charliermarsh on 2024-07-29 01:35_

@bulletmark -- That's awesome. Is that the variant that's confusingly large? Wonder how much smaller we can make it.

---

_Comment by @bulletmark on 2024-07-29 01:47_

@charliermarsh yes, this was discussed [here](https://github.com/indygreg/python-build-standalone/issues/275). Python 3.12 jumped massively in size compared to 3.11.

---
