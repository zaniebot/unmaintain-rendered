```yaml
number: 7387
title: Add support for remaining pip-supported file extensions
type: pull_request
state: merged
author: Aditya-PS-05
labels:
  - compatibility
assignees: []
merged: true
base: main
head: feature/add-file-extension-aliases-7365
created_at: 2024-09-14T08:13:50Z
updated_at: 2024-09-14T20:12:50Z
url: https://github.com/astral-sh/uv/pull/7387
synced_at: 2026-01-12T16:07:48Z
```

# Add support for remaining pip-supported file extensions

---

_@Aditya-PS-05_

closes #7365 

Summary

This pull request adds support for additional file extension aliases in the SourceDistExtension and ExtensionError enums. The newly supported file extensions include .tbz, .tgz, .txz, .tar.lz, .tar.lzma. These changes align the extensions supported by the SourceDistExtension with those used in Python packaging tools, enhancing compatibility with a broader range of source distribution formats.

Test Plan
should be added or updated to verify that the new extensions are correctly recognized as valid source distributions and that errors are correctly raised when unsupported extensions are provided.

---

_Comment by @Aditya-PS-05 on 2024-09-14 11:03_

I have updated some test cases which were initially for old extensions and as I have updated the extensions. I have updated these tests to work for new extensions also.
Thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-14 12:31_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-14 12:31_

---

_Comment by @charliermarsh on 2024-09-14 19:26_

For future reference:

```
gzip -cd Flask-2.0.0.tar.gz | lzma > Flask-2.0.0.tar.lzma
gzip -cd Flask-2.0.0.tar.gz | xz > Flask-2.0.0.tar.xz
```

---

_Comment by @charliermarsh on 2024-09-14 19:27_

Or, to create `.tar` from `.tar.gz`: `gzip -d Flask-2.0.0.tar.gz`

---

_Renamed from "Feature/add file extension aliases 7365" to "Add support for remaining pip-supported file extensions" by @charliermarsh on 2024-09-14 19:44_

---

_Label `compatibility` added by @charliermarsh on 2024-09-14 19:44_

---

_Merged by @charliermarsh on 2024-09-14 19:59_

---

_Closed by @charliermarsh on 2024-09-14 19:59_

---

_Comment by @Aditya-PS-05 on 2024-09-14 20:12_

> For future reference:
> 
> ```
> gzip -cd Flask-2.0.0.tar.gz | lzma > Flask-2.0.0.tar.lzma
> gzip -cd Flask-2.0.0.tar.gz | xz > Flask-2.0.0.tar.xz
> ```

I will try.

---
