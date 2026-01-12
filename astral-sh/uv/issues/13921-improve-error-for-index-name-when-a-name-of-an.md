```yaml
number: 13921
title: "Improve error for `--index <name>` when a name of an existing index is used"
type: issue
state: closed
author: jtfmumm
labels:
  - error messages
assignees: []
created_at: 2025-06-09T16:16:34Z
updated_at: 2025-06-25T08:02:08Z
url: https://github.com/astral-sh/uv/issues/13921
synced_at: 2026-01-12T16:01:39Z
```

# Improve error for `--index <name>` when a name of an existing index is used

---

_@jtfmumm_

The current message (added in #13917) is:

```
error: Directory not found for index: file://path/to/index
```

We can make this more informative, particularly since the user may have intended to specify a named index instead of a URL.

---

_Label `documentation` added by @jtfmumm on 2025-06-09 16:16_

---

_Label `error messages` added by @jtfmumm on 2025-06-09 16:25_

---

_Label `documentation` removed by @jtfmumm on 2025-06-09 16:25_

---

_Renamed from "Improve message for `--index` value pointing to non-existent directory" to "Improve error for `--index <name>` when a name of an existing index is used" by @zanieb on 2025-06-09 16:29_

---

_Comment by @zanieb on 2025-06-09 16:30_

My comment on #13917 was specifically for the case where the named index exists. We might always want to error here, regardless of whether the directory exists? Or require the user to disambiguate with `./<path>`?

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-09 16:58_

---

_Comment by @konstin on 2025-06-10 10:58_

I'd prefer requiring something that explicitly disambiguates paths from names, such as the `./path`.

---

_Closed by @jtfmumm on 2025-06-25 08:02_

---
