```yaml
number: 268
title: Are we ready to replace mypy with ty?
type: issue
state: closed
author: YeonwooSung
labels:
  - question
assignees: []
created_at: 2025-05-08T11:10:32Z
updated_at: 2025-12-17T00:45:51Z
url: https://github.com/astral-sh/ty/issues/268
synced_at: 2026-01-12T15:54:22Z
```

# Are we ready to replace mypy with ty?

---

_@YeonwooSung_

### Question

Last year, my team replaced poetry with uv. And it seems like ty is here for killing mypy.

Is ty production ready to replace mypy?
If not, is there some sort of roadmap?

### Version

_No response_

---

_Label `question` added by @YeonwooSung on 2025-05-08 11:10_

---

_Comment by @MichaReiser on 2025-05-08 11:13_

I'm glad you're interested in using our tools.

From the readme

> ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

So no, it's too early to replace any existing type checker with ty.

We plan to release a first preview soon, a beta later this year, and, if everything goes well, a stable version around the end of this year.

---

_Comment by @YeonwooSung on 2025-05-08 12:02_

Fair enough! Will look forward to the day that the first stable version of the ty will be released!
Will keep this issue till then, if you don't mind

---

_Comment by @zanieb on 2025-05-08 14:34_

I'd like to keep the issue tracker actionable — just watch for our stable release announcement.

---

_Closed by @zanieb on 2025-05-08 14:34_

---

_Comment by @MysterHawk on 2025-12-10 08:16_

Are we there yet 👀 

---

_Comment by @Garrett-R on 2025-12-17 00:45_

I see the language "not ready for production use" was [removed](https://github.com/astral-sh/ty/commit/7a6b79d37e165f2e731893bde05f6a548babc006) last month.  

And Astral said today in a [blogpost](https://astral.sh/blog/ty):

> Today, we're announcing the Beta release of ty ... and are ready to recommend it to motivated users for production use.

🚀 🎊 

---
