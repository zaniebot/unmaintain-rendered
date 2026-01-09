---
number: 7581
title: Bug/Feature with uv pip install from github
type: issue
state: closed
author: freand76
labels: []
assignees: []
created_at: 2024-09-20T11:47:16Z
updated_at: 2024-09-20T15:21:19Z
url: https://github.com/astral-sh/uv/issues/7581
synced_at: 2026-01-07T13:12:17-06:00
---

# Bug/Feature with uv pip install from github

---

_Issue opened by @freand76 on 2024-09-20 11:47_

When using **uv pip install** from a github repository, a working SHA from any of the repository forks will be picked up as valid.

_Example:_

uv pip install "python-usbtmc @ git+https://github.com/python-ivi/python-usbtmc@7c15a9d2138160024c4069ecf4b2edc33f15e40e"

_Problem/Feature:_

The repo **https://github.com/python-ivi/python-usbtmc** does not contain a commit with the SHA **7c15a9d2138160024c4069ecf4b2edc33f15e40e** but the fork **https://github.com/FOSSBOSS/python-usbtmc** does, i.e. **uv pip install** will install a packet from another repository than expected.

Tested on uv 0.4.10

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 14:09_

---

_Comment by @charliermarsh on 2024-09-20 14:09_

I'll take a look at this.

---

_Comment by @charliermarsh on 2024-09-20 15:03_

I think this is a consequence of GitHub's storage model for repositories. All GitHub repositories and their forks share the same backing storage, so you can access commits from forks through any sibling repository. For example, you can see that commit here: https://github.com/python-ivi/python-usbtmc/commit/7c15a9d2138160024c4069ecf4b2edc33f15e40e.

I need to look into whether other installers also behave this way, and what we can do to avoid it (if desired).

---

_Comment by @freand76 on 2024-09-20 15:11_

As I wrote in the heading it might be a feature, but at the same time it might also be a security issue in this (maybe unlikely) scenario:

`uv pip install <trusted package> @ <link to trusted git repository>@<SHA to an evil forked repository>`

Great that you will look into this!

---

_Comment by @charliermarsh on 2024-09-20 15:18_

I agree with your assessment, though it may be somewhat unsolvable / inherent to GitHub. If you run this, you will successfully fetch the commit in that fork:
```
git fetch --force --update-head-ok https://github.com/python-ivi/python-usbtmc +7c15a9d2138160024c4069ecf4b2edc33f15e40e:refs/remotes/origin/HEAD
```

---

_Comment by @freand76 on 2024-09-20 15:21_

Interesting, then it is out of our / your hands. 

Issue can be closed.

---

_Closed by @charliermarsh on 2024-09-20 15:21_

---
