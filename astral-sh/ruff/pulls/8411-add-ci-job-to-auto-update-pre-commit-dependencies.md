```yaml
number: 8411
title: Add CI job to auto-update pre-commit dependencies weekly
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: main
head: zanie/pre-commit-update
created_at: 2023-11-01T17:06:21Z
updated_at: 2024-03-25T18:44:58Z
url: https://github.com/astral-sh/ruff/pull/8411
synced_at: 2026-01-10T22:47:01Z
```

# Add CI job to auto-update pre-commit dependencies weekly

---

_Pull request opened by @zanieb on 2023-11-01 17:06_

pre-commit provides an `autoupdate` command to bump dependency versions in the pre-commit configuration. Not only are our current versions all stale, with #8410 we'll want a reasonable way to update the Ruff version we are using.

Until Dependabot support is released https://github.com/dependabot/dependabot-core/issues/1524, using pre-commits command is the best option. 

Note pre-commit provides a service that does this, but I'm not interested in depending on it.

This job opens a new pull request with updates if there are any.

---

_Comment by @zanieb on 2023-11-01 17:08_

I'm not sure I can test this via dispatch until it is merged

---

_Comment by @mkniewallner on 2023-11-01 18:55_

You might be interested in https://github.com/renovatebot/renovate, which is another free and open-source dependency update manager that handles [way more ecosystems](https://docs.renovatebot.com/modules/manager/) than Dependabot (including `pre-commit`), and is also way more customisable (allowing for instances to group all patch dependencies into a single PR).

We use it on [deptry](https://github.com/fpgmaas/deptry), if you are curious ([here](https://github.com/fpgmaas/deptry/blob/main/renovate.json5)'s our configuration file, and https://github.com/fpgmaas/deptry/pull/503 if you want to see a PR that updates a `pre-commit` dependency), and I've personally been using it quite extensively on multiple projects, so if you think this could be interesting to use on Ruff, I'd be happy to give a hand.

---

_@charliermarsh approved on 2023-11-01 21:31_

Looks reasonable to me, defer to @zanieb on whether we want to use renovate :)

---

_Comment by @zanieb on 2023-11-01 21:35_

Hm... tempting. I've never used it seems like most people seem to find the ux relatively similar.

@mkniewallner if you're interested in opening a pull request adding it I'll certainly review. It'd be nice to group patch version bumps in our cargo dependencies and bumps of development dependencies.



---

_Comment by @mkniewallner on 2023-11-01 22:33_

> Hm... tempting. I've never used it seems like most people seem to find the ux relatively similar.
> 
> @mkniewallner if you're interested in opening a pull request adding it I'll certainly review. It'd be nice to group patch version bumps in our cargo dependencies and bumps of development dependencies.

Happy to! I'll play a bit with the configuration on a fork to have a better representation of PRs that Renovate would create, and eventually open a PR over here.

---

_Closed by @zanieb on 2023-11-07 14:42_

---

_Reopened by @zanieb on 2024-03-23 02:56_

---

_Comment by @zanieb on 2024-03-23 02:56_

Noticing our pre-commit dependencies are way out of date and opening this to investigate them again....

---

_Comment by @AlexWaygood on 2024-03-23 08:23_

FWIW I've used renovate in a few projects, and just switched typeshed over to using it. It's slightly more complex to setup than dependabot, but it's extremely configurable, and it's really useful that it can do pre-commit dependencies as well as other dependencies. I don't have any major complaints with it.

Happy to open a PR setting up the config for it on Monday so you can compare it with this!

---

_Comment by @AlexWaygood on 2024-03-25 12:35_

> Happy to open a PR setting up the config for it on Monday so you can compare it with this!

Here's what the renovate config would look like:
- https://github.com/astral-sh/ruff/pull/10567

---

_Comment by @AlexWaygood on 2024-03-25 18:44_

Closing as superseded by #10567 👍

---

_Closed by @AlexWaygood on 2024-03-25 18:44_

---
