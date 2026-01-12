```yaml
number: 16131
title: "I don't understand documentation for version"
type: issue
state: closed
author: raffaem
labels:
  - bug
assignees: []
created_at: 2025-10-06T01:32:53Z
updated_at: 2025-10-06T09:16:02Z
url: https://github.com/astral-sh/uv/issues/16131
synced_at: 2026-01-12T16:02:24Z
```

# I don't understand documentation for version

---

_@raffaem_

### Summary

[The documentation says](https://docs.astral.sh/uv/guides/package/#updating-your-version):

```
uv version --bump beta
hello-world 1.3.0b1 => 1.3.1b2
```

But for me:

```
PATH>uv version
mylib 1.3.0b1

PATH>uv version --bump beta
Resolved 3 packages in 29ms
      Built mylib @ file:///PATH
Prepared 1 package in 568ms
Uninstalled 1 package in 1ms
Installed 1 package in 23ms
 - mylib ==1.3.0b1 (from file:///PATH)
 + mylib ==1.3.0b2 (from file:///PATH)
mylib 1.3.0b1 => 1.3.0b2
```

Also, seems there are no dollar signs on that code snippet and you can't copy it?

### Platform

Windows

### Version

0.8.2

### Python version

_No response_

---

_Label `bug` added by @raffaem on 2025-10-06 01:32_

---

_Comment by @sterliakov on 2025-10-06 02:20_

Looks like a simple typo in the docs, bumping beta modifier isn't supposed to also bump the patch component.

---

_Comment by @khneal on 2025-10-06 04:08_

Agreed. I opened a PR to fix the error and add the `$` command prompt.

---

_Closed by @konstin on 2025-10-06 09:16_

---
