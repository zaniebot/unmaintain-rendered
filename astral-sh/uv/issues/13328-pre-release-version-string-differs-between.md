---
number: 13328
title: Pre-release version string differs between pyproject.toml and uv version
type: issue
state: open
author: Steinkreis
labels:
  - bug
assignees: []
created_at: 2025-05-07T11:17:49Z
updated_at: 2025-05-09T13:55:04Z
url: https://github.com/astral-sh/uv/issues/13328
synced_at: 2026-01-10T01:25:32Z
---

# Pre-release version string differs between pyproject.toml and uv version

---

_Issue opened by @Steinkreis on 2025-05-07 11:17_

### Summary

I've noticed a discrepancy between how the pre-release version is defined in pyproject.toml and how it's returned via the uv version command.

In our pyproject.toml, the version is specified as:
```toml
version = "2.2.3-rc.1+dgup-2555"
```

However, when running `uv version` or `uv version --short,` the output is:
`2.2.3rc1+dgup.2555`

Please let me know if this is intended behavior or a parsing/display bug.

### Platform

Linux 5.19.0-1028-aws x86_64 GNU/Linux

### Version

0.7.0

### Python version

3.10

---

_Label `bug` added by @Steinkreis on 2025-05-07 11:17_

---

_Comment by @charliermarsh on 2025-05-07 12:50_

This is generally expected as we normalize versions internally. Those versions mean have identical meaning in the Python spec.

---

_Comment by @Steinkreis on 2025-05-07 13:08_

Thanks for the quick reply!

We have downstream processes that rely on the exact version string format as defined in pyproject.toml. Since uv version alters the formatting, we're currently unable to reliably use this feature.

Question:
Is there a way to retrieve the original version string exactly as defined in pyproject.toml? A flag like uv version --raw or --pyproject would be very helpful for our use case.

Thanks again, and happy to hear if this could be considered.

---

_Comment by @zanieb on 2025-05-07 13:29_

I would strongly recommend updating your downstream process to operate on the spec-compliant canonicalized version.

I'm supportive of a `--raw` flag or similar, but a little hesitant since it encourages non-standard behavior downstream.

---

_Comment by @zanieb on 2025-05-07 13:29_

cc @Gankra 

---

_Comment by @Steinkreis on 2025-05-07 13:53_

Thanks for the hint @zanieb ! I'll surely discuss that with my team 👍

---

_Comment by @notatallshaw on 2025-05-07 14:39_

You can normalize the string with the Python packaging library:

```
uv run --with packaging python -c "from packaging.version import Version; print(Version('2.2.3-rc.1+dgup-2555'))"
```

---

_Comment by @Gankra on 2025-05-09 13:54_

Ah yeah I've got plans to do some improvements here!

---

_Assigned to @Gankra by @konstin on 2025-05-09 13:55_

---
