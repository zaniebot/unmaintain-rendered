```yaml
number: 14405
title: "Ignore Python patch version for `--universal` pip compile"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/uni
created_at: 2025-07-02T01:59:12Z
updated_at: 2025-07-02T15:12:19Z
url: https://github.com/astral-sh/uv/pull/14405
synced_at: 2026-01-12T16:11:12Z
```

# Ignore Python patch version for `--universal` pip compile

---

_@charliermarsh_

## Summary

The idea here is that if a user runs `uv pip compile --universal`, we should ignore the patch version on the current interpreter. I think this makes sense... `--universal` tries to resolve for all future versions, so it seems a bit odd that we'd start at the _current_ patch version.

Closes https://github.com/astral-sh/uv/issues/14397.


---

_Label `enhancement` added by @charliermarsh on 2025-07-02 01:59_

---

_Marked ready for review by @charliermarsh on 2025-07-02 01:59_

---

_@zanieb approved on 2025-07-02 03:17_

Seems reasonable. Will this cause breakage?

---

_@konstin approved on 2025-07-02 12:31_

sgtm

---

_Merged by @charliermarsh on 2025-07-02 15:11_

---

_Closed by @charliermarsh on 2025-07-02 15:11_

---

_Branch deleted on 2025-07-02 15:11_

---

_Comment by @charliermarsh on 2025-07-02 15:12_

I don't expect this to cause any issues for users (though it's _possible_).

---
