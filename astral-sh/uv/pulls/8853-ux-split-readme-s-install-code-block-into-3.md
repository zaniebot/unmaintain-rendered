```yaml
number: 8853
title: "[UX] Split README's install code block into 3"
type: pull_request
state: merged
author: willingc
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-11-06T03:44:33Z
updated_at: 2024-11-08T00:14:51Z
url: https://github.com/astral-sh/uv/pull/8853
synced_at: 2026-01-12T16:08:32Z
```

# [UX] Split README's install code block into 3

---

_@willingc_

- Broke up the large code block into 3 blocks so that copy paste icon works

Howdy Astral friends! This is a small PR which is a small user experience improvement for smooth installs.

---

_Comment by @zanieb on 2024-11-07 17:49_

Won't the copy paste fail because of the leading `$` anyway?

---

_Comment by @zanieb on 2024-11-07 17:52_

For some more context, in https://github.com/astral-sh/uv/issues/5397 we made it so that the `$` is stripped from copied console commands in the documentation but that doesn't apply to the README. I think we'll need to move the comments and `$` out of the code blocks to achieve the desired effect here.

Nice to see you Carol! Thanks for taking the time to contribute :)

---

_Comment by @willingc on 2024-11-07 22:06_

I'm not sure about the $. But I figured that I would mention it since the copy paste grabbed all of lines and made the shell grumpy. Good to see you virtually too.

---

_Comment by @zanieb on 2024-11-07 22:32_

I gave it a try [in the preview](https://github.com/astral-sh/uv/blob/7ab5f7676fa7533b2dbc124a8c8e25111b82302e/README.md#installation) and we'll need to change that before we can merge. I can futz with it sometime soon if you don't want to.

---

_Comment by @willingc on 2024-11-07 23:40_

It does fail if you keep the preceding `$`.  I've removed the `$` and switched the syntax highlighting to `bash`. I'm able to copy-paste fine on Mac. YMMV on Windows (I don't have a good test machine for that.) @zanieb 

---

_Comment by @zanieb on 2024-11-08 00:13_

Thank you!

---

_Label `documentation` added by @zanieb on 2024-11-08 00:13_

---

_@zanieb approved on 2024-11-08 00:13_

---

_Merged by @zanieb on 2024-11-08 00:13_

---

_Closed by @zanieb on 2024-11-08 00:13_

---

_Branch deleted on 2024-11-08 00:14_

---
