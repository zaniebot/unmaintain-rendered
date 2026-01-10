```yaml
number: 10808
title: uv roadmap
type: issue
state: open
author: HenriBlacksmith
labels:
  - question
assignees: []
created_at: 2025-01-21T08:02:57Z
updated_at: 2026-01-06T10:40:40Z
url: https://github.com/astral-sh/uv/issues/10808
synced_at: 2026-01-10T03:11:33Z
```

# uv roadmap

---

_Issue opened by @HenriBlacksmith on 2025-01-21 08:02_

Hey,

I am super excited about uv and all the work done already.

A few project management features beyond dependency management ones have been discussed in other issues, I was curious about the existence of a public roadmap mentioning the expected features at least (not necessarily dates but if some exist, why not 😀!)

Features like build-backend or version management look super promising and I would be interested to see those in the list.

I am aware of feature-specific tracking (which is already super nice) but I was looking at one level above:

- https://github.com/astral-sh/uv/issues/8779

Thanks in advance!

---

_Assigned to @zanieb by @zanieb on 2025-01-21 17:25_

---

_Label `question` added by @zanieb on 2025-01-21 17:25_

---

_Comment by @zanieb on 2025-01-21 17:26_

I'm working on a public roadmap right now 

---

_Comment by @alexandreCameron on 2025-02-20 11:33_

Hi,

The uv product is awesome, but the stable version is not released yet:
> uv does not yet have a stable API; once uv's API is stable (v1.0.0), the versioning scheme will adhere to Semantic Versioning.
<https://docs.astral.sh/uv/reference/policies/versioning/>

Having a view of the roadmap would be great.

In the 0.6.0 pre-stable minor release, there a section indicating that a feature in stabilized and no longer in preview:
> Stabilizations
> * uv publish is no longer in preview
<https://github.com/astral-sh/uv/releases/tag/0.6.0>

I’ll like to deploy uv in production, but I’d like to avoid using feature that are not stable.

Do you have page referencing the list of stable/very-unlikely-to-change features ?

I'm especially interested in:

- uv pip install
- uv pip compile
- uv pip tree

The information could require less effort than publishing the roadmap and still convince teams to put uv in production even if the stable version is not out yet.

@zanieb 

---

_Comment by @zanieb on 2025-02-20 13:53_

All of uv is fairly stable — especially the `uv pip` commands.

So far, we've made small breaking changes every ~30 releases or so and they're very clearly marked. You can see examples in the CHANGELOG.

I'm kind of surprised I wrote that in the versioning policy. I don't think it conveys the correct message about uv's readiness for production.

---

_Comment by @zanieb on 2025-02-20 13:54_

And as an update here, I have written a roadmap — it's just a lot of work to translate it to GitHub issues and that remains on my backlog. I've been prioritizing fixing bugs over sharing it more broadly.

---

_Comment by @alexandreCameron on 2025-02-20 14:56_

Awesome, you've made my day 🎉 

---

_Comment by @HenriBlacksmith on 2025-02-25 08:06_

> And as an update here, I have written a roadmap — it's just a lot of work to translate it to GitHub issues and that remains on my backlog. I've been prioritizing fixing bugs over sharing it more broadly.

Thanks for the update, could it be shared in a synthetic form here?

I am super interested in progress on package and project management features (like build-backend stabilization and version bump capabilities). 

I feel like other tools like `hatch` are not releasing much changes since `uv` arrived and having feature parity (and even more) in uv would allow to get rid of those tools.

---

_Comment by @zanieb on 2025-02-26 19:11_

Both of those things are high priority items on the roadmap, we're actively moving the build-backend towards stabilization (#11446) and I'm working on a version bump interface this month.

---

_Comment by @HenriBlacksmith on 2026-01-06 10:40_

@zanieb, thanks for all the work done on uv over the past months!

I am still interested in roadmap related information, I suppose the GitHub issues are part of the answer but might not fully reflect Astral's priorities and plans.

As mentioned in other issues, I am still interested in bump interface evolutions (for instance git tag/commit additions, but also things like GitLab release creation could be nice). Things related to upgrading dependencies are another set of features that I am interested in.

Do you still have plans to share some form of roadmap?
Could also be a categorization of issues by feature or blog posts describing Astral's plans.

Thanks in advance for the answers!

---
