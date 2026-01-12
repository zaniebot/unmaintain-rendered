```yaml
number: 6139
title: Ignore specific lines in every file
type: issue
state: open
author: bryevdv
labels:
  - suppression
  - needs-decision
assignees: []
created_at: 2023-07-28T01:23:19Z
updated_at: 2023-07-28T18:29:43Z
url: https://github.com/astral-sh/ruff/issues/6139
synced_at: 2026-01-12T15:54:45Z
```

# Ignore specific lines in every file

---

_@bryevdv_

Here is the situation I am in: We have received a new license comment header from corporate that we are mandated to use. The license header contains e.g. SPDX markers in a very specific format. We have no leeway to modify this header in any way, e.g by adding `noqa` to any lines, as this will interfere with some sort of mandatory automated license scanning. 

Here is the problem:

* The first line of the license header is 99 characters long. 
* Our current enforced codebase line limit is 80 characters. 
* 😦 

We *really* do not want to change the line length to 100 and cause every file to be reformatted, vandalizing `git blame` history in the process. How can we ignore this one line in every single file? Some config option ideas that seem like they would work:

* setting to ignore the first N lines of every file (and/or file glob)
* setting to ignore specific line(s) of every file (and/or file glob)
* Ignore any initial comment block entirely
* setting to ignore any line in any file that matches a regex pattern

Would it be possible to implement any of these in `ruff`? Alternatively is there any other existing solution I am missing? 

---

_Comment by @charliermarsh on 2023-07-28 01:26_

Haha. I feel your pain. I'm sure we can find a way to help. Are you able to post the license comment header here, if it's public? I am just curious if there's anything in it that we can leverage.

---

_Label `noqa` added by @charliermarsh on 2023-07-28 04:30_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-28 04:30_

---

_Comment by @bryevdv on 2023-07-28 18:02_

It's not a public file so will err on the side of caution. I'll just say it starts with a designated and identifiable classifier that could be matched, if matching was an option.

Also full disclosure, we were granted a last minute reprieve in the form of an alternate permissible spelling. So this is not quite as pressing as it was, but I do still think it is a useful feature and that this case is still one that definitely can occur in the wild. 

---

_Comment by @charliermarsh on 2023-07-28 18:29_

👍 Yeah no worries, I only meant if it were, like, already committed in some public Git repo.

---
