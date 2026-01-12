```yaml
number: 15024
title: About using uv to download some dependencies stuck
type: issue
state: closed
author: YuanJie2001
labels:
  - question
assignees: []
created_at: 2025-08-02T02:55:40Z
updated_at: 2025-08-05T13:25:04Z
url: https://github.com/astral-sh/uv/issues/15024
synced_at: 2026-01-12T16:02:02Z
```

# About using uv to download some dependencies stuck

---

_@YuanJie2001_

### Question

I used uv to download graalpy, which was very slow, but I still managed to download it successfully. Then I tried to download langgraph, but it got stuck. I set up a mirror source, but the download still depended on luck. I don't understand what's going on, and I can't take it anymore. such as:

<img width="1094" height="816" alt="Image" src="https://github.com/user-attachments/assets/78b0ad61-21fe-4ff4-ae14-a657c3d8be71" />

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @YuanJie2001 on 2025-08-02 02:55_

---

_Comment by @charliermarsh on 2025-08-02 03:03_

None of those are downloads. Your machine is attempting to build those dependencies from source (a process that is unrelated to uv). You should look into why that is. For example, perhaps you're using a very new version of Python and those versions of those packages don't have pre-compiled distributions for them.

---

_Comment by @YuanJie2001 on 2025-08-02 03:18_

oh, thank you . I think it's graalpy and immature.

---

_Closed by @YuanJie2001 on 2025-08-02 03:21_

---

_Comment by @charliermarsh on 2025-08-02 03:25_

Ah yeah, unfortunately none of those packages publish wheels for GraalPy.

---

_Comment by @YuanJie2001 on 2025-08-05 13:25_

Thanks to the big guys for the tips, graalpy is able to run dependencies such as langgraph, the pitfalls that arise are mainly the following:
1. uv blocks the underlying Python warnings.
2. graalpy needs to rely on system dependency managers such as patch to supplement its unique diff file.
3. For uv graalpy, there is a lack of quality domestic mirror sources.
4. There is the problem of network instability.
5. In Windows system, patch operation can not be executed with normal user rights, you must start the editor as an administrator.

---
