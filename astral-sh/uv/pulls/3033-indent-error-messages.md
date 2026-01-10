```yaml
number: 3033
title: " Indent error messages "
type: pull_request
state: closed
author: konstin
labels:
  - error messages
assignees: []
draft: true
base: main
head: konsti/indent-error-messages
created_at: 2024-04-15T09:07:07Z
updated_at: 2024-05-21T16:03:42Z
url: https://github.com/astral-sh/uv/pull/3033
synced_at: 2026-01-10T14:32:20Z
```

#  Indent error messages 

---

_Pull request opened by @konstin on 2024-04-15 09:07_

Before:

![image](https://github.com/astral-sh/uv/assets/6826232/9a07a525-2a33-446d-a671-422985d81ce1)

After:

![image](https://github.com/astral-sh/uv/assets/6826232/9bbca5e2-aef5-40bb-8961-e0bf9ccf64a2)

I'm not sure if we want that, but i wanted to try it out. If we like the style i'll improve the code.

pro:
* clear separation of command output
* supports nesting
con:
* breaks copy and paste of nested output
* output becomes longer

---

_Label `error messages` added by @konstin on 2024-04-15 09:07_

---

_Comment by @konstin on 2024-04-15 16:04_

Indentation with whitespace only:

![image](https://github.com/astral-sh/uv/assets/6826232/9d3bdb19-7f51-483d-91d7-558ea0f413f8)


---

_Closed by @konstin on 2024-05-21 16:03_

---
