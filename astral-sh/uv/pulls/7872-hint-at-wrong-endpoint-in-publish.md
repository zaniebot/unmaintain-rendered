```yaml
number: 7872
title: Hint at wrong endpoint in publish
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/hint-at-upload-error
created_at: 2024-10-02T16:30:40Z
updated_at: 2024-10-08T19:16:03Z
url: https://github.com/astral-sh/uv/pull/7872
synced_at: 2026-01-12T16:08:03Z
```

# Hint at wrong endpoint in publish

---

_@konstin_

Improve hints when using the simple index URL instead of the upload URL in `uv publish`. This is the most common confusion when publishing, so we give it some extra care and put it more centrally in the CLI help.

Fixes #7860

---

_Label `enhancement` added by @konstin on 2024-10-02 16:30_

---

_Review requested from @zanieb by @konstin on 2024-10-02 16:30_

---

_@zanieb reviewed on 2024-10-02 17:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4398 on 2024-10-02 17:14_

This feels too aggressive and isn't always true right? (i.e. below you say "typically")

---

_@zanieb reviewed on 2024-10-02 17:15_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4400 on 2024-10-02 17:15_

Should we say something like this?

I don't think users struggling with this will know what the simple index API is?

> Note that there are typically different URLs for index access (e.g., `.../simple`) and index upload.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4398 on 2024-10-02 17:19_

I feel like the hint in the error is going to go a long way here already.

Maybe...

```suggestion
    /// The URL of the upload endpoint (typically differs from the index URL).
```

---

_@zanieb reviewed on 2024-10-02 17:19_

---

_@konstin reviewed on 2024-10-04 09:12_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4398 on 2024-10-04 09:12_

It's a different URL at least for pypi, gitlab, aws and google and it's the top thing i want to push users towards, so i think this is warranted.

---

_Review requested from @zanieb by @konstin on 2024-10-07 16:43_

---

_@zanieb reviewed on 2024-10-07 17:04_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4398 on 2024-10-07 17:04_

I think it's too aggressive to include an exclamation point in the help menu.

```suggestion
    /// The URL of the upload endpoint (not the index URL).
```

---

_Merged by @zanieb on 2024-10-08 19:16_

---

_Closed by @zanieb on 2024-10-08 19:16_

---

_Branch deleted on 2024-10-08 19:16_

---
