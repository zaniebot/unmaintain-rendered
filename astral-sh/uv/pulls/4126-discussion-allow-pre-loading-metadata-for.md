```yaml
number: 4126
title: "[Discussion] Allow pre-loading metadata for installed dists"
type: pull_request
state: closed
author: aochagavia
labels: []
assignees: []
base: main
head: in-memory-metadata
created_at: 2024-06-07T07:51:54Z
updated_at: 2024-06-11T12:15:21Z
url: https://github.com/astral-sh/uv/pull/4126
synced_at: 2026-01-12T16:06:03Z
```

# [Discussion] Allow pre-loading metadata for installed dists

---

_@aochagavia_

Note: this is more an issue than a PR, but since I've already made the changes in my fork, I thought I'd open a PR so you can see the code next to my explanation.

## Problem

I'm using uv's solver in a custom environment, where distributions that are considered to be already installed are not actually present in the filesystem. Instead, I manually tell the solver about them. However, the solver tries to load the distributions' metadata from the filesystem, which causes an error (because it cannot find the `METADATA` file).

## Proposed solution

Instead of going directly to the filesystem, we could make it possible to provide pre-loaded metadata when specifying installed dists. If absent, the solver can fall back to the filesystem, as we do today. This is what I have implemented in this PR.

## Additional considerations

In my current implementation I focused in getting this working, so it's probably on the ugly side. If there's enough interest, I can try to rearchitect it in a way that fits the uv codebase better (though some guidance will be needed). Also, I totally understand if my proposal deviates too much from what uv is trying to achieve, so feel free to close this PR and then I'll make sure to regularly rebase my fork (this feature is a deal-breaker to me, unless I can find a workaround).

## Test Plan

I tested this manually, but I guess we should have automated tests if we decide we want this feature.


---

_Comment by @zanieb on 2024-06-07 13:58_

Interesting. I don't have much to say about the change, it's not very invasive but we're generally pretty hesitant to pick up code that would be "dead" on our end.

> where distributions that are considered to be already installed are not actually present in the filesystem

Can you share more about the use case for this? It sounds peculiar :)

---

_Comment by @aochagavia on 2024-06-07 18:45_

> We're generally pretty hesitant to pick up code that would be "dead" on our end.

That makes sense to me... It's probably what I myself would do as well.

> Can you share more about the use case for this? It sounds peculiar :)

I'm prototyping a system that analyses container images, determines which pip packages are installed in a specific virtual environment, installs additional packages requested by the user, and returns a new container image. Instead of starting the container or extracting its files to disk, I'm going through the layers in-memory (they are just tar files) and extract package information as I find it (the `METADATA` and `direct_url.json` files). The idea (which still needs to be validated) is to derive container images significantly faster than with dockerfiles, in part due to new caching possibilities (e.g. each package becomes its own image layer, so you can mix and match them).

---

_Comment by @aochagavia on 2024-06-11 06:55_

Since this is not really useful for uv itself, I'm closing the issue. Thanks for the discussion 😉

---

_Closed by @aochagavia on 2024-06-11 06:55_

---
