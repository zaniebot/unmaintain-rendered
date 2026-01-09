---
number: 7528
title: "Support `--with-editable` in `uv tool install` "
type: issue
state: closed
author: gaborbernat
labels:
  - enhancement
  - help wanted
  - cli
assignees: []
created_at: 2024-09-19T01:07:36Z
updated_at: 2024-10-23T00:06:34Z
url: https://github.com/astral-sh/uv/issues/7528
synced_at: 2026-01-07T13:12:17-06:00
---

# Support `--with-editable` in `uv tool install` 

---

_Issue opened by @gaborbernat on 2024-09-19 01:07_

Would it be possible to specify the editable flag for the with options too? Something like:

```
 uv tool install -e tox@/Users/bernat/git/github/tox  \
					   --with-editable tox-uv@/Users/bernat/git/github/tox-uv  \
						--force-reinstall --force
```


---

_Comment by @charliermarsh on 2024-09-19 02:09_

Yeah I think this probably _should_ exist, it may be an oversight \cc @zanieb in case you know otherwise.

---

_Label `enhancement` added by @charliermarsh on 2024-09-19 02:09_

---

_Label `help wanted` added by @charliermarsh on 2024-09-19 02:09_

---

_Label `cli` added by @charliermarsh on 2024-09-19 02:09_

---

_Comment by @zanieb on 2024-09-19 02:14_

Yeah I presumed the contribution at https://github.com/astral-sh/uv/pull/6744 did this but it only was `uv tool run`.

cc @kyoto7250 if you're interested.

---

_Renamed from "uv tool with editable" to "Support `--with-editable` in `uv tool install` " by @zanieb on 2024-09-19 02:14_

---

_Comment by @kyoto7250 on 2024-09-19 03:26_

@zanieb 
Let me work on it!

---

_Comment by @kyoto7250 on 2024-09-27 04:20_

~~I'll be able to publish a PR on the weekend.~~
It may be more difficult than originally estimated. I am continuing to work on this.


---

_Comment by @kyoto7250 on 2024-10-22 15:13_

I still have the will to work on this issue, but I've been busy with work for a while, so I don't think I'll be able to do it this month.

If there is someone who is interested, it would be nice if that person could be in charge.


---

_Referenced in [astral-sh/uv#8472](../../astral-sh/uv/pulls/8472.md) on 2024-10-22 17:43_

---

_Closed by @charliermarsh on 2024-10-23 00:06_

---
