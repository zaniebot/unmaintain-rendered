---
number: 15346
title: Support for installing packages from Subversion
type: issue
state: open
author: waszil
labels:
  - enhancement
assignees: []
created_at: 2025-08-18T11:55:05Z
updated_at: 2025-11-18T13:41:52Z
url: https://github.com/astral-sh/uv/issues/15346
synced_at: 2026-01-07T13:12:19-06:00
---

# Support for installing packages from Subversion

---

_Issue opened by @waszil on 2025-08-18 11:55_

### Summary

I would really like to use uv, it seems a great tool. However at my company we have a lot of python packages in SVN, and currently pip can handle this no problem by specifying an svn url like this:
mypackage @ svn+https://svnserver.company.com/mypackage/tags/latest#egg=mypackage
I know that uv is a modern tool and I know that SVN is a dinosaur, but it is still very much in use at a lot of companies, and if uv tells about itself that it can be a drop-in replacement for pip, I think it would be nice to support dinosaurs as well :)
Thanks for any feedbacks.


---

_Label `enhancement` added by @waszil on 2025-08-18 11:55_

---

_Comment by @zanieb on 2025-08-18 14:14_

Related

- https://github.com/astral-sh/uv/issues/13131
- https://github.com/astral-sh/uv/issues/14504

I think Charlie's comment on the latter one sums up our stance

> It could make sense to support it, it's just a matter of prioritization and I'd want to see more user demand before investing in it. Beyond the initial implementation, it's also a lot of work to maintain support over time.

---

_Comment by @waszil on 2025-08-18 14:18_

I guessed so. Well then I will spread the word to generate demand… 🥲

---

_Comment by @superlou on 2025-11-18 13:41_

I'm also trying to sell our organization on UV, and while doing `uv pip install ...` is ok, it's definitely less ergonomic.

---
