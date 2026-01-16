```yaml
number: 17529
title: Broken links on GitHub Documentation - looking fro
type: issue
state: closed
author: michellepace
labels:
  - question
assignees: []
created_at: 2026-01-16T16:46:20Z
updated_at: 2026-01-16T18:19:11Z
url: https://github.com/astral-sh/uv/issues/17529
synced_at: 2026-01-16T19:01:52Z
```

# Broken links on GitHub Documentation - looking fro

---

_@michellepace_

### Summary

Hello, 

BUG: On this page: https://github.com/astral-sh/uv/blob/main/docs/reference/index.md the links "**Commands**" and "**Settings**" take me to a 404

QUESTION: If possible, could you please provide the GitHub Link for this page https://docs.astral.sh/uv/reference/cli ?

Thanks in advance,
Michelle

### Platform

GitHub

---

_Label `bug` added by @michellepace on 2026-01-16 16:46_

---

_Comment by @zanieb on 2026-01-16 16:48_

That's the source for the documentation. Why do you need GitHub links? That file is generated at build time.

---

_Label `bug` removed by @zanieb on 2026-01-16 16:48_

---

_Label `question` added by @zanieb on 2026-01-16 16:48_

---

_Comment by @chilin0525 on 2026-01-16 16:56_

FYI — this commit may be relevant: 5a6f2ea

---

_Comment by @zanieb on 2026-01-16 17:14_

Yes we no longer commit those generated files.

---

_Comment by @michellepace on 2026-01-16 17:23_

@zanieb Thanks for the reply. Because when I scrape the file with FireCrawl it is not 100% clean. I save them offline so I can easily reference them when working with Claude Code. Its cleaner when I come directly to GitHub 99% of the time. 

---

_Comment by @zanieb on 2026-01-16 17:28_

You should scrape from https://docs.astral.sh/uv/llms.txt instead

---

_Comment by @michellepace on 2026-01-16 17:29_

Thank you!!!

---

_Closed by @zanieb on 2026-01-16 18:19_

---
