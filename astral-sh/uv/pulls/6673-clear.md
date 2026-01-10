```yaml
number: 6673
title: Clear 
type: pull_request
state: closed
author: Filimoa
labels:
  - documentation
assignees: []
base: main
head: main
created_at: 2024-08-27T06:31:09Z
updated_at: 2024-09-18T15:11:15Z
url: https://github.com/astral-sh/uv/pull/6673
synced_at: 2026-01-10T12:53:33Z
```

# Clear 

---

_Pull request opened by @Filimoa on 2024-08-27 06:31_

## Summary

I realize this might make the docs slightly more cluttered, but maybe it would be helpful to clarify how to update packages to the latest version?  This seems like a super common workflow. 

While it does make sense that you guy are just inheriting from what pip does, it's also not intuitive since you're using the `add` command to install a package in the first place.

Personally I tried:
```
uv update fastapi # cargo, poetry, npm
uv upgrade fastapi # yarn, brew
uv add fastapi@latest # fails + adds a random line to my pyproject.toml?
```

Before finally running `uv add --help`

## Test Plan

n/a


---

_Comment by @zanieb on 2024-08-27 11:29_

Sorry I'm not sure I follow. `uv lock --upgrade` or `uv sync --upgrade` is how you should upgrade to the latest version of a package (as supported by your constraints). I don't think the example you provided will always bump to the latest released version — e.g. if you've provided constraints the pyproject.toml won't change during that invocation.

---

_Label `documentation` added by @zanieb on 2024-08-27 11:29_

---

_Assigned to @zanieb by @zanieb on 2024-08-27 11:29_

---

_Comment by @Filimoa on 2024-08-27 16:21_

Ok thanks for clarifying - I learned something today about poetry. What is the correct way to bump to the latest version available (even if it's outside of your explicit constraints)?


```
fastapi==0.91.0
```

To 

```
fastapi==0.112.2
```

Is this the best practice then? 

```
$ uv add fastapi==0.112.2
```

It seems very tedious to go to pypi every time to check what the latest version is? Npm handles this by `npm install package@latest`.  Poetry also handles this by `poetry add pendulum@latest`.

---

_Comment by @zanieb on 2024-08-27 16:28_

Yes, `uv add` with a new constraint would be proper. There's no reason to pin versions in your `pyproject.toml` though, that's what the `uv.lock` file is for. We'll do all of the pinning for you behind the scenes. The changes I made in #6698 might be helpful to you.

(We don't support bumping the minimum compatible version to the latest version or next semver compatible version yet)

---

_Comment by @Filimoa on 2024-08-27 16:36_

Ok thanks that's helpful. Maybe it would be helpful to have a section in the docs explaining this? I imagine many users are coming from `pip freeze > requirements.txt` and are used to using `pyproject.toml` as essentially a lock file? 

I also realize this might just be a "me" problem - just an idea.

---

_Closed by @zanieb on 2024-09-18 15:11_

---
