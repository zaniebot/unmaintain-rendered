```yaml
number: 2041
title: Warn when trying many versions
type: pull_request
state: closed
author: konstin
labels:
  - enhancement
assignees: []
base: main
head: konsti/warn-on-slow-resolve
created_at: 2024-02-28T10:25:17Z
updated_at: 2024-05-21T16:03:28Z
url: https://github.com/astral-sh/uv/pull/2041
synced_at: 2026-01-12T16:04:50Z
```

# Warn when trying many versions

---

_@konstin_

In pathological cases, it's hard to follow why the resolution is slow. This is an attempt to give the user an indication what is slow (boto3) and how to fix this (add a manual boto3 constraint). This also helps to determine if this is a pathological case or a server that doesn't support range requests.

I picked the download function as point-of-counting so we can potentially ignore selection versions that we immediately discard, while network requests (both cached and uncached) are what makes the resolution slow. The threasholds i picked are somewhat arbitrary.

```
$ uv pip install bio_embeddings
  ⠸ pytest==8.0.2
warning: Downloading metadata for more than 50 different versions of boto3. Consider adding a stricter version constraint for this package to speed up resolution.
warning: Downloading metadata for more than 50 different versions of botocore. Consider adding a stricter version constraint for this package to speed up resolution.
  ⠼ boto3==1.26.85
```

`warn_user!` interacts badly with the `Reporter`, do we have a way to pause-and-restart the restart the reporter when showing our own messages, similar to tqdm or `IndicatifLayer`?


---

_Label `enhancement` added by @konstin on 2024-02-28 10:25_

---

_Comment by @MichaReiser on 2024-02-28 11:51_

> warn_user! interacts badly with the Reporter, do we have a way to pause-and-restart the restart the reporter when showing our own messages, similar to tqdm or IndicatifLayer?

Could we lock/buffer the output instead and flush the warnings once the reporter is done? 

---

_Comment by @konstin on 2024-02-28 12:11_

We want to report this during the resolution so the user can abort, add constraints and restart. We also want to have during resolution to give a sense for "why is this taking so long".

---

_Comment by @zanieb on 2024-02-28 18:13_

For what it's worth, I find these warnings kind of _confusing_ in `pip` but I don't have a clear answer why. I think this seems like a good start I guess I'll see if I find these bothersome in the same way :) 

---

_Comment by @notatallshaw on 2024-02-29 19:43_

> For what it's worth, I find these warnings kind of _confusing_ in `pip` but I don't have a clear answer why.

I think I know why it's confusing.

Pip says it is trying many different versions of a package, but there is no clear relationship between the package it names and what a user could do to help solve this problem.

Pip is not saying there are many versions of that package. Pip is not saying that package is the reasons for the slow resolution.

Pip is saying that due to it's algorithm that's what is currently trying a lot. Usually that means Pip has a poor heuristic on what to backtrack on, and it's a sign to someone who wants to improve the resolution algorithm this might be a good requirement example to test on. It is rarely ever helpful to the end user.

I hope once I get my latest PR through that Pip's heuristic on what to backtrack on will be so sufficiently improved the package it mentions in these cases *might* be useful to the end user, but I am not at all confident.


---

_Comment by @konstin on 2024-03-04 10:14_

What about we merge this and see how users like it? We can roll it back in the next release, it's only a cosmetic change.

---

_Review requested from @MichaReiser by @konstin on 2024-03-04 10:14_

---

_Review requested from @charliermarsh by @konstin on 2024-03-04 10:14_

---

_Review requested from @zanieb by @konstin on 2024-03-04 10:14_

---

_Closed by @konstin on 2024-05-21 16:03_

---
