```yaml
number: 6692
title: What is the intended workflow for updating dependencies with uv?
type: issue
state: closed
author: christianplatta1012
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-27T15:14:00Z
updated_at: 2025-11-25T16:09:48Z
url: https://github.com/astral-sh/uv/issues/6692
synced_at: 2026-01-12T15:59:06Z
```

# What is the intended workflow for updating dependencies with uv?

---

_@christianplatta1012_

Hi,
I am looking into uv for several days now and try to determine whether my team should switch from poetry to uv.
One thing that seems completely absent in the documentation is a way to update dependencies.

We are a python-only team with more than 40 separate projects. We get information about available updates/security issues from dependabot and then usually do a `poetry update` to update all dependencies to the latest minor version or a `poetry add <packagename>@latest` if we change major versions. This is usually faster an easier compared to merging dependabot pull requests.

What is the intended workflow in uv to update dependencies? 
I've found https://github.com/astral-sh/uv/issues/1419 that talks about a feature request similar to `poetry update` .
I could probably script something around uv until such a feature exists, but currently I am totally lost how even a single dependency update is intended to work...

Do I really need to know the latest version number for all my dependencies in the pyproject.toml and do a manual uv add <dependecy==version_number>?

---

_Label `documentation` added by @zanieb on 2024-08-27 15:14_

---

_Label `question` added by @zanieb on 2024-08-27 15:14_

---

_Comment by @zanieb on 2024-08-27 15:17_

Hi! In short, you can do `uv lock --upgrade` to upgrade all of the locked versions of your packages.

We don't support "bumping" the dependency constraints in your `pyproject.toml` yet. That's definitely something we're interested in, but we haven't done any design work for that yet.

---

_Assigned to @zanieb by @zanieb on 2024-08-27 15:17_

---

_Comment by @christianplatta1012 on 2024-08-27 15:26_

Thank you, this helps a lot.
Maybe worth to add this as an example to the docs. Searching for "upgrade" in https://docs.astral.sh/uv/ does not yield this as result.

---

_Closed by @christianplatta1012 on 2024-08-27 15:26_

---

_Comment by @zanieb on 2024-08-27 16:01_

Thanks! https://github.com/astral-sh/uv/pull/6698

---

_Comment by @Sam1320 on 2024-11-18 08:15_

> We don't support "bumping" the dependency constraints in your pyproject.toml yet. That's definitely something we're interested in, but we haven't done any design work for that yet.

Any progress on this direction in the meantime? @zanieb  It would be really useful to auto-bump the .toml along with the .lock upgrade. I find myself constantly manually removing `uv remove [package]` and then re-adding it to update the pyproject.toml dependencies.

(also, is there a better workaround than `remove & add` ?)

---

_Comment by @mbeijen on 2024-11-22 08:12_

> > We don't support "bumping" the dependency constraints in your pyproject.toml yet. That's definitely something we're interested in, but we haven't done any design work for that yet.
> 
> Any progress on this direction in the meantime? @zanieb It would be really useful to auto-bump the .toml along with the .lock upgrade. I find myself constantly manually removing `uv remove [package]` and then re-adding it to update the pyproject.toml dependencies.
> 
> (also, is there a better workaround than `remove & add` ?)

I found a solution, it works fine but I'm not fully sure if it is the _best_ way;

 - in your `pyproject.toml` do *not* specify explicit versions, if you don't need to.
 - If you require a specific range, such as latest 4.2 release of Django, use `"Django>=4.2,<4.3"`
 - Install your dependencies using `uv sync` and commit `uv.lock`.
 - Now all your installs, co-workers and deploys use the exact same versions
 - If you want to upgrade, use `uv lock --upgrade` and uv will fetch the latest updates, based on your specified requirements
 - Test and then commit the upgraded `uv.lock`!

---

_Comment by @zed on 2024-11-23 09:46_

To update specific dependency: `uv lock --upgrade-package <package>`

---

_Comment by @john-pixforce on 2024-12-06 16:44_

> We don't support "bumping" the dependency constraints in your `pyproject.toml` yet. That's definitely something we're interested in, but we haven't done any design work for that yet.

hey, @zanieb, do you have any tracker/issue to check updates on it?

---

_Comment by @Goldziher on 2024-12-21 20:06_

also interested in this

---

_Comment by @zanieb on 2024-12-23 17:07_

See https://github.com/astral-sh/uv/issues/6794

---

_Comment by @qiuxiaomu on 2025-05-15 20:17_

> Thanks! [#6698](https://github.com/astral-sh/uv/pull/6698)

Reading the help page for this command, i.e., 

> Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`

I interpreted it as that it would ignore whatever the locked version is and as long as there's version newer than the one in lock file, uv is going to ignore the mandate and update it to the latest available; this isn't the behaviour at the moment right? I see that uv is smart enough to only update what's not going to break the dependencies. 

If I were right, the help page is a bit confusing then, in that uv is not ignoring the locked version (correctly). 


---

_Comment by @cesarqdt on 2025-08-19 18:53_

> To update specific dependency: `uv lock --upgrade-package <package>`

this does not update to a latest available version, it just updates to the latest defined in the `pyproject.toml`

---

_Comment by @mbeijen on 2025-08-19 21:00_

> > To update specific dependency: `uv lock --upgrade-package <package>`
> 
> this does not update to a latest available version, it just updates to the latest defined in the `pyproject.toml`

Yeah, if you have restrictions in pyproject.toml stating django < 5 you'll not get updated to django 5.x when running uv lock --upgrade, the lockfile must adhere to the constraints in the lockfile. You can remove those constraints, or you can see which newer versions would be available using `uv pip list --outdated`

---
