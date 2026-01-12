```yaml
number: 2990
title: Mismatch between documentation and code comment on PyPI Index priority
type: issue
state: closed
author: tdejager
labels:
  - documentation
assignees: []
created_at: 2024-04-11T12:23:51Z
updated_at: 2024-04-11T17:31:36Z
url: https://github.com/astral-sh/uv/issues/2990
synced_at: 2026-01-12T15:58:41Z
```

# Mismatch between documentation and code comment on PyPI Index priority

---

_@tdejager_

The code in `crates/distribution-types/src/index_url.rs:167` says: 

```
/// The index locations to use for fetching packages.
///
/// By default, uses the PyPI index.
///
/// "pip treats all package sources equally" (<https://github.com/pypa/pip/issues/8606#issuecomment-788754817>),
/// and so do we, i.e., you can't rely that on any particular order of querying indices.
///
/// If the fields are none and empty, ignore the package index, instead rely on local archives and
/// caches.
///
/// From a pip perspective, this type merges `--index-url`, `--extra-index-url`, and `--find-links`.
```

Hower the `PIP_COMPATIBILITY.md` mentions:

> In both uv and pip, users can specify multiple package indexes from which to search for the available versions of a given package. However, uv and pip differ in how they handle packages that exist on multiple indexes.
>For example, imagine that a company publishes an internal version of requests on a private index (--extra-index-url), but also allows installing packages from PyPI by default. In this case, the private requests would conflict with the public [requests](https://pypi.org/project/requests/) on PyPI.
>When uv searches for a package across multiple indexes, it will iterate over the indexes in order (preferring the --extra-index-url over the default index), and stop searching as soon as it finds a match. This means that if a package exists on multiple indexes, uv will limit its candidate versions to those present in the first index that contains the package.
> etc.

I feel like this contradicts each other. I think the code comment is out of date or am I missing something else 😄 ?

---

_Comment by @charliermarsh on 2024-04-11 14:06_

The comment is out-of-date, I'll update it.

---

_Label `documentation` added by @charliermarsh on 2024-04-11 14:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 14:06_

---

_Comment by @charliermarsh on 2024-04-11 14:06_

(The compatibility guide is correct.)

---

_Comment by @tdejager on 2024-04-11 15:33_

Great thank you! :)

---

_Closed by @charliermarsh on 2024-04-11 17:31_

---
