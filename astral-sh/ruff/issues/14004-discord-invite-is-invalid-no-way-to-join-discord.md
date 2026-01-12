```yaml
number: 14004
title: Discord invite is invalid / no way to join discord server
type: issue
state: closed
author: jackklika
labels:
  - documentation
assignees: []
created_at: 2024-10-30T18:08:52Z
updated_at: 2024-10-30T19:13:34Z
url: https://github.com/astral-sh/ruff/issues/14004
synced_at: 2026-01-12T15:54:53Z
```

# Discord invite is invalid / no way to join discord server

---

_@jackklika_

Hi, I can't find a way to join the discord channel. The current invite link on the README of this project and the link in the docs links to an "invite invalid" screen.

I also cannot find the channel when searching `astral` `astral-sh` or `ruff` on the discord "explore channels" screen.

---

_Comment by @MichaReiser on 2024-10-30 18:14_

Thanks for reporting this. Hmm, I just clicked on the link and it correctly opened [Astral's discord](https://discord.com/invite/astral-sh). Can you share the link that you clicked? Just to make sure we're talking bout the same link 

---

_Label `documentation` added by @MichaReiser on 2024-10-30 18:14_

---

_Comment by @jackklika on 2024-10-30 19:13_

I just opened `https://discord.com/invite/astral-sh` and see the following:
![image](https://github.com/user-attachments/assets/4b0d9a12-2c60-41c9-be1f-cd87f9381d11)

Looking at the dev tools I'm getting the following 403 so it is probably my account...
```
{
    "message": "You need to verify your account in order to perform this action.",
    "code": 40002
}
```
I got in by opening incognito window and logging in there. Not sure why my current session was unverified. 

This can be closed, thanks for your patience.

---

_Closed by @jackklika on 2024-10-30 19:13_

---
