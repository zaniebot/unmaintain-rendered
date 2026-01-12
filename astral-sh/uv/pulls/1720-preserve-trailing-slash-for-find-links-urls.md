```yaml
number: 1720
title: "Preserve trailing slash for `--find-links` URLs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/find-links
created_at: 2024-02-19T21:20:54Z
updated_at: 2024-02-19T21:26:13Z
url: https://github.com/astral-sh/uv/pull/1720
synced_at: 2026-01-12T16:04:42Z
```

# Preserve trailing slash for `--find-links` URLs

---

_@charliermarsh_

## Summary

We should allow a `--find-links` URL to be provided as _either_ (e.g.) `https://wheelhouse.acsone.eu/manylinux1` or `https://wheelhouse.acsone.eu/manylinux1/`. By using the response URL, we can "always do the right thing" (it will always have a trailing slash, or always return a `.html` suffix) rather than attempting to sniff out the URL kind in advance.

Closes https://github.com/astral-sh/uv/issues/1683.

## Test Plan

- `cargo run pip install requests --force-reinstall --no-index --find-links https://wheelhouse.acsone.eu/manylinux1 -n`
- `cargo run pip install requests --force-reinstall --no-index --find-links https://wheelhouse.acsone.eu/manylinux1/ -n`


---

_Label `bug` added by @charliermarsh on 2024-02-19 21:21_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-19 21:21_

---

_@zanieb approved on 2024-02-19 21:21_

---

_Merged by @charliermarsh on 2024-02-19 21:26_

---

_Closed by @charliermarsh on 2024-02-19 21:26_

---

_Branch deleted on 2024-02-19 21:26_

---
