```yaml
number: 639
title: Preserve verbatim URLs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/relative
created_at: 2023-12-13T21:45:50Z
updated_at: 2023-12-14T15:03:41Z
url: https://github.com/astral-sh/uv/pull/639
synced_at: 2026-01-10T15:44:44Z
```

# Preserve verbatim URLs

---

_Pull request opened by @charliermarsh on 2023-12-13 21:45_

## Summary

This PR adds a `VerbatimUrl` struct to preserve verbatim URLs throughout the resolution and installation pipeline. In short, alongside the parsed `Url`, we also keep the URL as written by the user. This enables us to display the URL exactly as written by the user, rather than the serialized path that we use internally.

This will be especially useful once we start expanding environment variables since, at that point, we'll be able to write the version of the URL that includes the _unexpected_ environment variable to the output file.

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:42 on 2023-12-13 21:47_

The only use-case for this right now is... `precise`. It's challenging, because with `precise`, we take a Git URL like `git://foo.bar` and rewrite it as `git://foo.bar@1278caca0`, with a specific commit SHA. So now, we need a way to apply that change to the original, verbatim string...

---

_@charliermarsh reviewed on 2023-12-13 21:47_

---

_@konstin approved on 2023-12-14 12:34_

---

_Merged by @charliermarsh on 2023-12-14 15:03_

---

_Closed by @charliermarsh on 2023-12-14 15:03_

---

_Branch deleted on 2023-12-14 15:03_

---
