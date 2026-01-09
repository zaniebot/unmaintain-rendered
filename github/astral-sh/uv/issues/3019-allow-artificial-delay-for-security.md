---
number: 3019
title: Allow artificial delay for security considerations for uv pip compile
type: issue
state: closed
author: mikeckennedy
labels:
  - question
assignees: []
created_at: 2024-04-13T18:08:11Z
updated_at: 2024-04-15T20:31:59Z
url: https://github.com/astral-sh/uv/issues/3019
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow artificial delay for security considerations for uv pip compile

---

_Issue opened by @mikeckennedy on 2024-04-13 18:08_

Consider this a feature request and a bit of a weird one. But I'd love to hear your thoughts on it. Close it if it's not inline with the uv vision.

I've been thinking about what can be done to limit exposure to the supply chain attacks that were once theoretical but [seem to be weekly now](https://www.bleepingcomputer.com/news/security/pypi-suspends-new-user-registration-to-block-malware-campaign/).

These are generally caught but may persist for a period of time before detected. 

How do you feel about something like:

```
uv pip compile requirements.piptools --upgrade --delay-in-days 7
```

That is adding `--delay-in-days 7` which would only upgrade a library to a newer version if that update has been out for the specific number of days.  So say there is pydantic 2.7.1 and we have pydantic 2.7.0 specified, --upgrade won't switch it to 2.7.1 until the 2.7.1 is at least 7 days old.

It's not a perfect fix against these pypi attacks. But letting the rest of the world "go first" in trying out the updates could be a good hedge against just installing whatever was just published to pypi that hour.

Another really valuable aspect to this would be big-time bugs that get shipped and then pulled quickly. A recent example was mongoengine shipped a catastrophic bug that when used on a web server with threading=true enabled (uwsgi? gunicorn? I don't think it mattered) it would 100% fail all queries. This was quickly fixed but still took down my site because I didn't test it locally in a threaded server. Maybe this feature would have helped. At least the git hub issues would have been out and discussed by the time I started looking for what went wrong.

Any interest in this idea? I discussed it with the pip folks a year or two ago and I think they didn't see any value in it.


---

_Comment by @notatallshaw on 2024-04-13 18:52_

uv already has `--exclude-newer` but you have to give it a specific timestamp, with sufficently good shell/utilities you should be able to do something like:

```
uv pip compile requirements.piptools --upgrade --exclude-newer $(date --utc -d "7 days ago" "+%Y-%m-%dT%H:%M:%SZ")
```


---

_Label `question` added by @zanieb on 2024-04-14 16:17_

---

_Closed by @zanieb on 2024-04-14 16:17_

---

_Comment by @mikeckennedy on 2024-04-15 20:17_

Good to know that there is this option. It's not the most user friendly :) but I use uv through a bunch of shell aliases anyway so I should be able to make it work. Thank you.

---

_Comment by @zanieb on 2024-04-15 20:19_

It seems kind of complicated to add at the top-level right now but if there's significant interest we can consider it in the future.

---

_Comment by @mikeckennedy on 2024-04-15 20:23_

Sounds good @zanieb Perhaps it's the kind of thing that belongs in some sort of config file. Maybe the equivalent of ruff.toml if you ever create such a thing for uv.

---

_Comment by @zanieb on 2024-04-15 20:31_

See 
- https://github.com/astral-sh/uv/pull/3007
- #1511

---

_Referenced in [astral-sh/uv#14992](../../astral-sh/uv/issues/14992.md) on 2025-07-31 15:46_

---
