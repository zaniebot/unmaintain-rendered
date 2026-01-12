```yaml
number: 9841
title: "Dim uninstalled Python keys in `uv python list`"
type: pull_request
state: open
author: zanieb
labels:
  - cli
assignees: []
draft: true
base: main
head: zb/list-dim
created_at: 2024-12-12T15:25:54Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/9841
synced_at: 2026-01-12T16:09:00Z
```

# Dim uninstalled Python keys in `uv python list`

---

_@zanieb_

Attempted to improve the experience in https://github.com/astral-sh/uv/issues/9830 by dimming the key as well as the `<download available>` bit

<img width="1322" alt="Screenshot 2024-12-12 at 9 25 18 AM" src="https://github.com/user-attachments/assets/59f5c231-51ec-4b93-8d0e-5c540a404f19" />


---

_Label `cli` added by @zanieb on 2024-12-12 15:25_

---

_Comment by @zanieb on 2024-12-12 15:27_

Perhaps if we dim these we should sort them _after_ the installed versions?

---

_Comment by @hwine on 2024-12-12 18:00_

I like styling both columns in each line the same -- that strengthens the relationship.

I _think_ I like sorting available-but-not-yet-installed to the bottom, but that will be a "visually breaking change".

This discussion is helping me focus on what my main root issue is with `uv` installations. `uv` divides the python-sources into two worlds (managed & system), but "system", to me (and I think a lot of prior python install discussions), means "installed & managed by the OS". (This was certainly true during the python 2 to 3 migration era.)

So, where does that leave the user installed versions, such as those handled by `pyenv` or manual installs from `python.org`?

Which is a long winded way of saying that, on reflection, I think the "does uv consider this installed python managed or system" question is more important than "is this installed yet". Knowing that distinction is a requirement to choosing the correct `--python-preference` option for my current use case. (I'm unaware of any other command that shows that information. afaik, you have to guess, then confirm, by setting an option to `uv python list`)

Even more succinct, I would prefer addressing the (just opened) #9847 to this issue. (Sorry for the churn, but thanks for helping me think this through.)


---

_Comment by @shauneccles on 2024-12-12 21:19_

I wonder if the list has n table groups, where n is types of install status?

```
PS D:\Programming\LedFx> uv python list
System Installed  
cpython-3.12.1-windows-x86_64-none                 C:\Users\shaun\AppData\Local\Programs\Python\Python312\python.exe
uv Installed
cpython-3.10.15-windows-x86_64-none                C:\Users\shaun\AppData\Roaming\uv\python\cpython-3.10.15-windows-x86_64-none\python.exe
cpython-3.9.20-windows-x86_64-none                 C:\Users\shaun\AppData\Roaming\uv\python\cpython-3.9.20-windows-x86_64-none\python.exe
uv Available
cpython-3.13.1+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.1-windows-x86_64-none                 <download available>
cpython-3.12.8-windows-x86_64-none                 <download available>
cpython-3.11.11-windows-x86_64-none                <download available>
cpython-3.10.16-windows-x86_64-none                <download available>
cpython-3.9.21-windows-x86_64-none                 <download available>
cpython-3.8.20-windows-x86_64-none                 <download available>
cpython-3.7.9-windows-x86_64-none                  <download available>
pypy-3.10.14-windows-x86_64-none                   <download available>
pypy-3.9.19-windows-x86_64-none                    <download available>
pypy-3.8.16-windows-x86_64-none                    <download available>
pypy-3.7.13-windows-x86_64-none                    <download available>
uv Unavailable
some-random-version-that-isn't-available
```

---

_Comment by @zanieb on 2024-12-12 21:23_

We've been intentionally avoiding tables here, they're harder to parse and I don't think the complexity is merited. It's tough to definitely tough the information density / simplicity right though.

---

_Comment by @BurntSushi on 2024-12-17 20:08_

I like keeping them sorted as-is. I think it's easy to be disoriented if there are two different groups, each sorted, in the output.

I don't mind using color to dim things that are uninstalled. From the screenshot, I wonder if the color here is a little too dim. But I like the idea.

---

_Comment by @konstin on 2024-12-18 11:59_

I like separate "installed" and "download available" sections, this would make it easier to skim what is actually installed and what's only in the list.

We could also reduce some duplication, e.g. those are all one python interpreter on my machine:

```
cpython-3.12.3-linux-x86_64-gnu                 .local/bin/python3.12 -> .pyenv/versions/3.12.3/bin/python
cpython-3.12.3-linux-x86_64-gnu                 .local/bin/python3 -> python3.12
cpython-3.12.3-linux-x86_64-gnu                 .local/bin/python -> python3.12
```

For reference, that's what my list currently looks like:

![image](https://github.com/user-attachments/assets/6608bb52-d963-4ae7-b2ce-e5eabed07257)


---

_Comment by @zanieb on 2024-12-18 15:22_

>  separate "installed" and "download available" sections, this would make it easier to skim what is actually installed and what's only in the list.

This is intentional though, like, you can _use_ those versions automatically even if they're not explicitly installed yet. That's why they're listed in the first place. There's `--only-installed` if you don't want to see them.

> We could also reduce some duplication, e.g. those are all one python interpreter on my machine ..

This is also intentional, so you can see what each executable on your path refers to. If we collapsed these, you'd lose that `python` and `python3` are aliases for `python3.12`.

---
