```yaml
number: 14152
title: "Require disambiguated relative paths for `--index`"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: jtfm/disambiguate-index
created_at: 2025-06-20T13:53:59Z
updated_at: 2025-12-19T14:19:32Z
url: https://github.com/astral-sh/uv/pull/14152
synced_at: 2026-01-12T16:11:03Z
```

# Require disambiguated relative paths for `--index`

---

_@jtfmumm_

We do not currently support passing index names to `--index` for installing packages. However, we do accept relative paths that can look like index names. This PR adds the requirement that `--index` values must be disambiguated with a prefix (`./` or `../` on Unix and Windows or `.\\` or `..\\` on Windows). For now, if an ambiguous value is provided, uv will warn that this will not be supported in the future.

Currently, if you provide an index name like `--index test` when there is no `test` directory, uv will error with a `Directory not found...` error. That's not very informative if you thought index names were supported. The new warning makes the context clearer.

Closes #13921

---

_Label `enhancement` added by @jtfmumm on 2025-06-20 13:54_

---

_Label `breaking` added by @jtfmumm on 2025-06-20 13:54_

---

_Label `error messages` added by @jtfmumm on 2025-06-20 13:57_

---

_@konstin reviewed on 2025-06-22 14:52_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5073 on 2025-06-22 14:52_

I'd put Unix and Windows paths on the same level, to reflect that Windows has equal first class support to Linux and Mac.

---

_@konstin reviewed on 2025-06-22 14:53_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index.rs`:308 on 2025-06-22 14:53_

`path.starts_with('/')` is redundant with `Path::new(path).is_absolute()`

---

_@konstin approved on 2025-06-22 14:55_

---

_Label `breaking` removed by @jtfmumm on 2025-06-23 12:00_

---

_Comment by @jtfmumm on 2025-06-23 13:22_

I have updated this to warn instead of error, and removed the `breaking` label.

---

_@konstin approved on 2025-06-24 20:39_

---

_Merged by @jtfmumm on 2025-06-25 08:02_

---

_Closed by @jtfmumm on 2025-06-25 08:02_

---

_Branch deleted on 2025-06-25 08:02_

---

_Comment by @bfontaine on 2025-12-19 14:19_

> We do not currently support passing index names to --index for installing packages

IMHO the error message should be explicit about this, because if you don’t know, you’re a bit confused as to why you get a message about a relative path when all you want is to install from an index name.

---
