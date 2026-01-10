```yaml
number: 2136
title: Unable to release due to PyPI project size limits
type: issue
state: closed
author: charliermarsh
labels:
  - release
assignees: []
created_at: 2023-01-24T16:40:38Z
updated_at: 2023-03-28T23:06:55Z
url: https://github.com/astral-sh/ruff/issues/2136
synced_at: 2026-01-10T11:09:45Z
```

# Unable to release due to PyPI project size limits

---

_Issue opened by @charliermarsh on 2023-01-24 16:40_

PyPI has a 10GB total size limit for projects. We’re at 200 releases, with 15 wheels each, and ~5MB per wheel, so we’ve now hit that limit.

---

_Comment by @Jackenmen on 2023-01-24 16:53_

Wow, took you just 5 months to reach that 😄 

In case you don't already know, project size limit increase requests can be filed here: https://github.com/pypi/support/issues/new?assignees=&labels=limit+request&template=limit-request-project.yml&title=Project+Limit+Request%3A+PROJECT_NAME+-+00+GB



---

_Comment by @charliermarsh on 2023-01-24 16:58_

Ugh, yeah, I feel bad!

Gonna file there when I’m back on my computer :)

---

_Comment by @charliermarsh on 2023-01-24 17:42_

https://github.com/pypi/support/issues/2545

---

_Comment by @LefterisJP on 2023-01-24 22:50_

ahahhaahahahh oh that's a weird problem to have. Had no idea there was such a limit, but thinking about it makes total sense. Guess they will adjust the limit for ruff. Hope you keep releasing daily though. It's nice to check the new changes when I find time :)

---

_Label `release` added by @charliermarsh on 2023-01-25 04:47_

---

_Comment by @charliermarsh on 2023-01-25 23:30_

Resolved for now.

---

_Closed by @charliermarsh on 2023-01-25 23:30_

---

_Comment by @charliermarsh on 2023-01-25 23:31_

For the future, we have two (three?) options:

1. Build fewer wheels (e.g., we could make some decisions based on platform popularity).
2. Release less often (once-a-week, probably).
3. Ask for a bigger exemption.

---

_Comment by @Jackenmen on 2023-01-26 00:30_

Could also remove wheels (while keeping the sdist) from older releases when the limit approaches. I don't know if GitHub releases on OSS projects have a project size limit but if not, the wheels for older releases could remain available there once they get removed from PyPI. It doesn't go well with pertaining to the immutability of releases on PyPI that users may expect but technically it is an option.

---

_Comment by @hugovk on 2023-01-26 11:07_

Deleting wheels from PyPI would mean installs would fail for people who have pinned them, which is the default with [pre-commit](https://pre-commit.com/), and common for linters, especially fast-moving ones!

Also worth bearing in mind that the wheel sizes are increasing over time as more rules and so on is added.

---

_Comment by @charliermarsh on 2023-01-26 12:25_

We could also consider not building quite as many wheels (by looking at platform / target popularity), although that feels bad too, since it’s so easy for us to ship them.

---

_Comment by @pradyunsg on 2023-03-28 22:26_

FWIW, I feel like I should clarify that the intent with not going with a large number straight up was not to disincentivise the project from supporting users or reducing velocity of development. PyPI is a shared resource and I want to be cautious in setting a precedent for "give me lots of space on a shared resource" requests being granted on a blanket basis.

It's 100% OK for you to come back around and say "hey, we considered changing things but didn't for $reasons" if you hit the limit in a few more months. :)


---

_Comment by @charliermarsh on 2023-03-28 23:06_

Thanks @pradyunsg, I really appreciate that clarification. By the same token, we of course want to be good members of the community and not abuse those shared resources. So we'll continue to be mindful of the rate of increase -- _but_, per your note, I'll try to avoid letting those concerns get in the way of positive user impact.

On the bright side, we kind of naturally fell into a more deliberate ~once-a-week cadence over the past ~month. So while we'll likely hit the revised cap eventually, it should take longer than last time.


---
