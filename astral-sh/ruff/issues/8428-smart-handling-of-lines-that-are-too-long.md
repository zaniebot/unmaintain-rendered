```yaml
number: 8428
title: Smart handling of lines that are too long
type: issue
state: closed
author: pitosalas
labels:
  - question
  - formatter
assignees: []
created_at: 2023-11-02T01:18:28Z
updated_at: 2023-11-05T19:28:53Z
url: https://github.com/astral-sh/ruff/issues/8428
synced_at: 2026-01-12T15:54:48Z
```

# Smart handling of lines that are too long

---

_@pitosalas_

In my code I have some lines which are too long. Perhaps it's a long f string for output or a long expression. It would be nice if ruff did some smart things to deal with the too long lines in a way that looks good. For example turning the string into several concatenated strings or something. Is that possible?

I have tried various pyproject.toml settings but they don't seem to have any effect. Maybe that's my problem>?

---

_Comment by @zanieb on 2023-11-02 01:28_

This is available in Black's preview style (https://github.com/psf/black/issues/182) and we are working on an implementation for it — but it's a difficult feature to get correct.

---

_Label `formatter` added by @zanieb on 2023-11-02 01:28_

---

_Comment by @MichaReiser on 2023-11-02 01:46_

@pitosalas are you using ruff's formatter or is this feedback related to `E501` (line-too-long) rule?

---

_Comment by @pitosalas on 2023-11-02 10:56_

@MichaReiser Honestly I am not sure. This is my first use of ruff. But my intent is that the formatter notices a line too long and does semi-intelligent things with indentation and even notational changes to make the line not be too long anymore. 

---

_Comment by @charliermarsh on 2023-11-02 13:49_

@pitosalas - Have you tried running `ruff format` instead of `ruff check`? That will attempt to wrap lines for you.

---

_Label `question` added by @charliermarsh on 2023-11-02 13:49_

---

_Comment by @charliermarsh on 2023-11-05 19:28_

Gonna close for now in the absence of more info, but recommend trying out `ruff format`!

---

_Closed by @charliermarsh on 2023-11-05 19:28_

---
