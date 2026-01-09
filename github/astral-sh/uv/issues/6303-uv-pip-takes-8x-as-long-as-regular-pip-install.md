---
number: 6303
title: uv pip takes ~8x as long as regular pip install with Python 3.13
type: issue
state: closed
author: EwoutH
labels:
  - question
assignees: []
created_at: 2024-08-21T06:51:27Z
updated_at: 2024-08-21T13:32:16Z
url: https://github.com/astral-sh/uv/issues/6303
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip takes ~8x as long as regular pip install with Python 3.13

---

_Issue opened by @EwoutH on 2024-08-21 06:51_

In our CI setup, we noticed uv pip taking very long to install our package, compared to uv pip, on Pyhton 3.13. While normally uv pip is way faster.

- `uv pip`: ~4 min ([log 1](https://github.com/projectmesa/mesa/actions/runs/10484765826/job/29039667322), [log 2](https://github.com/projectmesa/mesa/actions/runs/10484765826/job/29040042574))
- `pip`: 31 seconds ([log](https://github.com/projectmesa/mesa/actions/runs/10484876366/job/29039976496?pr=2173))

Almost all time is in the "Prepared packages" step.

What probably matters, is that with setup-python we cache pip dependencies. But that doesn't work for uv right?

Can we either cache uv, or can something be sped up in the uv preparing packages step to speed this process up?

**Edit:** An [uncached](https://github.com/projectmesa/mesa/actions/runs/10484997604/job/29040420374) pip install on Python 3.13 also takes quire long, because Pandas is build from scratch.

- https://github.com/actions/setup-python/issues/822

---

_Comment by @charliermarsh on 2024-08-21 13:18_

Yeah, you can save the uv cache. I recommend something like this: https://docs.astral.sh/uv/guides/integration/github/#caching

---

_Label `question` added by @charliermarsh on 2024-08-21 13:18_

---

_Comment by @EwoutH on 2024-08-21 13:29_

Thanks for the docs!

---

_Closed by @EwoutH on 2024-08-21 13:29_

---

_Comment by @EwoutH on 2024-08-21 13:32_

Maybe you can enable the [GitHub Discussions](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/enabling-or-disabling-github-discussions-for-a-repository), in many repos it's a nice place to ask questions!

---
