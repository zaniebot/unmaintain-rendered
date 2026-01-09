---
number: 11661
title: Migrating from pip-tools workflow to uv project
type: issue
state: closed
author: mario-at-intercom
labels:
  - question
assignees: []
created_at: 2025-02-20T11:13:19Z
updated_at: 2025-02-24T08:29:48Z
url: https://github.com/astral-sh/uv/issues/11661
synced_at: 2026-01-07T13:12:18-06:00
---

# Migrating from pip-tools workflow to uv project

---

_Issue opened by @mario-at-intercom on 2025-02-20 11:13_

### Question

Hey, i'm looking into migrating one of my projects from pip-tools to uv project.

I can't find a guidance on doing that. I'd like my versions in requirements.txt staying the same (and I have two different files for two different platforms) when i move to the uv's lockfile.

### Platform

macOS, Windows

### Version

uv 0.6.2

---

_Label `question` added by @mario-at-intercom on 2025-02-20 11:13_

---

_Comment by @konstin on 2025-02-20 11:22_

Can you describe what you tried and what didn't work for your case?

---

_Comment by @zanieb on 2025-02-20 14:13_

I don't think there's an easy way to use multiple `requirements.txt` files for different platforms to constrain the versions of a new uv lockfile.

---

_Comment by @mario-at-intercom on 2025-02-20 15:55_

@konstin I haven't tried any approach yet because I have no ideas to try.

My mental model:
- I have to create a brand new project with uv
- Have to move all dependencies from requirements.in (no versions defined) to pyproject.toml
- Have to make sure that uv.lock keeps versions from requirements.txt (will this happen automatically?). This is the piece I have no ideas about even to try.

@zanieb I have two requirements.txt because I need to install a different tensorflow-text package based on the platform. Platform selector (probably not the right term?!) wasn't supported back then so I had to resort to
- requirements-common.in
- requirements.in
- requirements-macos.in

That compile to requirements.txt and requirements-macos.txt. If I can use platform==darwin and platform!=darwin in uv project definition and that get properly translated to single uv lockfile, I don't need multiple requirements files.


---

_Comment by @zanieb on 2025-02-20 16:27_

You'd need to copy your `requirements.txt` contents into `tool.uv.constraint-dependencies` (https://docs.astral.sh/uv/reference/settings/#constraint-dependencies), presumably you'd also want to add the platform markers relevant for the requirements file.

Then, you can `uv add -r requirements.in` and the lockfile should respect the pinned versions from your compiled requirements files.

>  If I can use platform==darwin and platform!=darwin in uv project definition and that get properly translated to single uv lockfile,

Yes you can do this with markers, e.g., https://docs.astral.sh/uv/concepts/projects/dependencies/#platform-specific-dependencies

---

_Comment by @konstin on 2025-02-20 16:32_

(zanie wrote their response at the same time, sorry for the overlap)

> My mental model:
> 
>     * I have to create a brand new project with uv
> 
>     * Have to move all dependencies from requirements.in (no versions defined) to pyproject.toml
> 
>     * Have to make sure that uv.lock keeps versions from requirements.txt (will this happen automatically?). This is the piece I have no ideas about even to try.

The first two steps are correct, the third one is tricky. There's no 1:1 translation from locked requirement.txt to uv.lock due to the slightly different models (mainly that pip is platform specific and uv's lockfile is universal), but there's some tricks that can help you get close. 

One option is using constraints. Constraints use the same syntax as requirements and are passed explicitly, for the uv project interface in `tool.uv`:

```toml
[tool.uv]
constraint-dependencies = ["...", "..."]
```

You could add `; sys_platform == "darwin"` to each entry in the one requirements file and `; sys_platform != "darwin"` to the other, then insert them as constraints here. This is the most accurate approach but it my fail with a conflict due to uv's resolver being different from pip's, requiring some manual editing until it works. Once you have your `uv.lock`, you can remove the constraints, uv updates `uv.lock` only when required or requested.

Another is doing the first with `--exclude-newer` and the timestamp of the pip freezing, if that is available, then remove the `exclude-newer` line from the resulting `uv.lock`. This trick only works with pypi and simulates a resolution at that time, which may be sufficient to get the right lockfile.

---

_Comment by @mario-at-intercom on 2025-02-21 08:40_

Thanks @zanieb and @konstin. I think I understand what I need to do to migrate us. I don't know when exactly we'll do that but I needed to understand the complexity we might hit. Thanks for that.

It's probably safe to close the issue now since the path to migration is outlined.

---

_Closed by @konstin on 2025-02-24 08:29_

---
