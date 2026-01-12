```yaml
number: 534
title: Support granular target Python versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/python-version
created_at: 2023-12-04T03:30:59Z
updated_at: 2023-12-05T02:38:50Z
url: https://github.com/astral-sh/uv/pull/534
synced_at: 2026-01-12T16:04:01Z
```

# Support granular target Python versions

---

_@charliermarsh_

## Summary

Allows, e.g., `--python-version 3.7` or `--python-version 3.7.9`. This was also feedback I received in the original PR.

Closes https://github.com/astral-sh/puffin/issues/533.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-04 03:31_

---

_Review requested from @konstin by @charliermarsh on 2023-12-04 03:31_

---

_Label `enhancement` added by @charliermarsh on 2023-12-04 03:31_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:534 on 2023-12-04 08:56_

Do we want to use inline snapshots here?

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:13 on 2023-12-04 08:57_

I think we should enforce `>=3,<4` here

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:20 on 2023-12-04 08:59_

If we allow patch versions we should allow prereleases too, i have e.g. `3.13.0a1+` installed

---

_@konstin reviewed on 2023-12-04 09:08_

I like the `--python-version 3.7` syntax, but i wonder whether we should allow patch versions or just match independent of anything more granular than minor version, It would be really confusing to specify `--python-version 3.7` and then get a strange (or no) resolution because numpy wants `>=3.7.1`.

---

_Comment by @charliermarsh on 2023-12-04 14:49_

@konstin - How would that work in practice though? I actually _did_ have a case where the source distribution requires 3.7.9 or later (pygls). What would the semantics of `--python-version 3.7` be?

---

_Comment by @konstin on 2023-12-04 14:52_

> How would that work in practice though? I actually did have a case where the source distribution requires 3.7.9 or later (pygls). What would the semantics of --python-version 3.7 be?

Good question, i have two ideas: a) The latest python patch version we're aware of, hardcoded until we have an api to query the latest versions b) We treat the version as `3.x.<usize::MAX>`

---

_Comment by @konstin on 2023-12-04 14:54_

Option c) Do it poetry style, let the user specify `>=3.7` or `~=3.7` and if that's not enough nudge them to specify `>=3.7.9` or `~=3.7.9`.

---

_Comment by @charliermarsh on 2023-12-04 15:14_

I'm unsure... Like, it seems wrong that if you run with `--python-version 3.7`, we then resolve with `3.7.13` or whatever, and then that resolution doesn't work on some versions of Python 3.7.

---

_Comment by @charliermarsh on 2023-12-04 15:24_

Alright, well, why don't we do this:

- Allow patch versions...
- If the user only provides a minor version, assume the most recent (hard-coded) minor version.


---

_Comment by @konstin on 2023-12-04 15:37_

Sounds good!

---

_Merged by @charliermarsh on 2023-12-05 02:38_

---

_Closed by @charliermarsh on 2023-12-05 02:38_

---

_Branch deleted on 2023-12-05 02:38_

---
