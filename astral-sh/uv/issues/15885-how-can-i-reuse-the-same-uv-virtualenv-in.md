---
number: 15885
title: How can I reuse the same uv virtualenv in different project?
type: issue
state: open
author: havanoname
labels:
  - question
assignees: []
created_at: 2025-09-16T02:42:51Z
updated_at: 2025-11-19T04:32:26Z
url: https://github.com/astral-sh/uv/issues/15885
synced_at: 2026-01-10T01:26:01Z
---

# How can I reuse the same uv virtualenv in different project?

---

_Issue opened by @havanoname on 2025-09-16 02:42_

### Question

I've used Conda as a package manager before. But now I want to try UV, I'm having some problems.
i have different project, for exmple: _test1_ and _test2_. The same packages are used for both projects.
In Conda, I just create a virtual environment and choose the same interpreter for different projects. I read [docs](https://docs.astral.sh/uv/guides/projects/), it seems uv environment must be created in each project. How can I reuse the same uv virtualenv in different project?


### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @havanoname on 2025-09-16 02:42_

---

_Comment by @zanieb on 2025-09-16 02:47_

Hi!

Why do you want to use the same environment for each project? What's the benefit of reusing it? The intent of uv is that each project declares its dependencies and you don't need to think about what virtual environment you're using.

---

_Comment by @havanoname on 2025-09-16 03:25_

Because my project needs to use many packages to handle different data, like pytorch and tensorflow, etc, which are quite large. If I had a separate virtual environment for each project, each project folder would large, even just a test project.

---

_Comment by @konstin on 2025-09-16 07:38_

uv uses hardlinks for venvs, so each now pytorch or tensorflow venv is small on disk.

---

_Comment by @notatallshaw-gts on 2025-09-16 16:05_

> uv uses hardlinks for venvs, so each now pytorch or tensorflow venv is small on disk.

Only if you are then keeping each project in sync. If you have a dozen projects that you are updating at different frequencies you quite quickly end up with close to a dozen versions of each package, in this case uv's strategy for saving space becomes significantly less effective, and uv's cache strategy of unpacked packages often uses quite a lot more space.

For my work I still use conda environments until uv has support for something like https://github.com/astral-sh/uv/issues/1495 that covers this use case.

---

_Comment by @zanieb on 2025-09-16 18:45_

> Only if you are then keeping each project in sync. If you have a dozen projects that you are updating at different frequencies you quite quickly end up with close to a dozen versions of each package, in this case uv's strategy for saving space becomes significantly less effective, and uv's cache strategy of unpacked packages often uses quite a lot more space.

This is more about our cache design than sharing environments though? even if you shared a single environment, we'd have multiple unpacked versions in the cache.

---

_Comment by @zanieb on 2025-09-16 18:46_

Without getting into the details of uv's cache, which I think is out of scope here and has various trade-offs, the environments will share files so any duplicate package versions will not consume additional disk space (as noted by konsti).

---

_Comment by @notatallshaw-gts on 2025-09-16 19:15_

> > Only if you are then keeping each project in sync. If you have a dozen projects that you are updating at different frequencies you quite quickly end up with close to a dozen versions of each package, in this case uv's strategy for saving space becomes significantly less effective, and uv's cache strategy of unpacked packages often uses quite a lot more space.
> 
> This is more about our cache design than sharing environments though? even if you shared a single environment, we'd have multiple unpacked versions in the cache.

But those can be cleaned up by clearing the cache, when you have a dozen projects that validly point to almost a dozen versions of large packages you can not clean them up in one command, you have to go through laborious process of getting everything in line in each project.

> Without getting into the details of uv's cache, which I think is out of scope here and has various trade-offs, the environments will share files so any duplicate package versions will not consume additional disk space (as noted by konsti).

It's related in the sense that uv's cache can use a lot more disk space, so you can regularly hit disk space limits because of such large cache, so you're forced into cleaning up diskspace more often, and the lack of first class support for sharing environments is quite noticeable as the different versions of the packages in each project use a lot more space than using a smaller number of reusable environments.

As I say, I am using conda to solve this problem, so it's not pressing for me, but it is a real annoyance.

---

_Comment by @kmantel on 2025-10-07 07:53_

I think this and #1495 would get me to start using uv. I basically went looking for pyenv + package sharing. For me, the benefit aside from my preference of having an active env regardless of directory is in a situation like where packages a and b have a dependency d, and say 

package a:
`d>=1, <2`

package b
`d>=1, <1.6`

If I have two separate envs, then I could end up with two copies, d==1.7 and d==1.5, when with one env I can just have one copy of d==1.5 without needing to micromanage

---

_Comment by @KercyDing on 2025-11-19 04:32_

I had the same issue when migrating from Conda to uv. Really missed being able to share environments across projects.

Looks like others have hit this too (#14988, #14856) - the "one project, one env" model doesn't fit all workflows, especially for those coming from Conda.

So I built [uvup](https://github.com/KercyDing/uvup) to handle this:

```bash
# Create a global environment
uvup create myenv
uvup add numpy pandas requests

# Use it in different projects
cd ~/test1
uvup activate myenv
uv run script.py

# Reuse the same env
cd ~/test2
uvup activate myenv
uv run another_script.py
```

Environments live in `~/.uvup/`, not tied to any project directory.

It also supports template-based project creation, cross-platform shell integration (bash/zsh/fish/PowerShell), and more.

Check out the [repo](https://github.com/KercyDing/uvup) and [docs](https://kercyding.github.io/uvup/) for details.

The project's still pretty early stage, so there might be some rough edges. Contributions welcome if you run into issues!😘


---
