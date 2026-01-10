```yaml
number: 7767
title: Allow spaces in path requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/spaces
created_at: 2024-09-28T21:23:29Z
updated_at: 2024-10-02T23:00:20Z
url: https://github.com/astral-sh/uv/pull/7767
synced_at: 2026-01-10T12:53:55Z
```

# Allow spaces in path requirements

---

_Pull request opened by @charliermarsh on 2024-09-28 21:23_

## Summary

This PR modifies our parsing to allow spaces in URLs. I don't know if this is a correct change... But we now parse URLs until we see:

- A newline.
- A semicolon (marker) or hash (comment), _preceded_ by a space. We parse the URL until the last
  non-whitespace character (inclusive).
- A semicolon (marker) or hash (comment) _followed_ by a space. We treat this as an error, since
  the end of the URL is ambiguous (e.g., `https://foo.com; marker`) would be a URL that ends in `;`).

Closes https://github.com/astral-sh/uv/issues/6032.


---

_Label `bug` added by @charliermarsh on 2024-09-28 21:23_

---

_Review requested from @konstin by @charliermarsh on 2024-09-28 21:23_

---

_Marked ready for review by @charliermarsh on 2024-09-28 21:23_

---

_@charliermarsh reviewed on 2024-09-29 17:28_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:842 on 2024-09-29 17:28_

These rules are definitely somewhat dubious.

---

_@konstin approved on 2024-09-30 08:35_

---

_Merged by @charliermarsh on 2024-09-30 16:38_

---

_Closed by @charliermarsh on 2024-09-30 16:38_

---

_Branch deleted on 2024-09-30 16:38_

---

_Comment by @ofek on 2024-10-02 21:54_

I haven't reviewed this in full but note that the Python `packaging` library requires a space before the semicolon as shown in this example: https://hatch.pypa.io/latest/config/dependency/#complex-syntax

---

_Comment by @zanieb on 2024-10-02 22:05_

I think that matches

> A semicolon (marker) or hash (comment), preceded by a space. We parse the URL until the last non-whitespace character (inclusive).

---

_Comment by @charliermarsh on 2024-10-02 22:27_

Yes we also require that.

---

_Comment by @charliermarsh on 2024-10-02 22:33_

(Although I don't believe pip requires that, either for PEP 508 or unnamed URL requirements.)

---

_Comment by @ofek on 2024-10-02 23:00_

Great thanks!

---
