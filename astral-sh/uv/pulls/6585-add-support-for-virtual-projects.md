```yaml
number: 6585
title: Add support for virtual projects
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/virtuals
created_at: 2024-08-24T16:03:38Z
updated_at: 2024-08-27T17:42:48Z
url: https://github.com/astral-sh/uv/pull/6585
synced_at: 2026-01-12T16:07:26Z
```

# Add support for virtual projects

---

_@charliermarsh_

## Summary

The basic idea here is: any project can either be a package, or not ("virtual").

If a project is virtual, we don't build or install it.

A project is virtual if either of the following are true:

- `tool.uv.virtual = true` is set.
- `[build-system]` is absent.

The concept of "virtual projects" only applies to workspace member right now; it doesn't apply to `path` dependencies which are treated like arbitrary Python source trees.

TODOs that should be resolved prior to merging:

- [ ] Documentation
- [ ] How do we reconcile this with "virtual workspace roots" which are a little different -- they omit `[project]` entirely and don't even have a name?
- [x] `uv init --virtual` should create a virtual project rather than a virtual workspace.
- [x] Running `uv sync` in a virtual project after `uv init --virtual` shows `Audited 0 packages in 0.01ms`, which is awkward. (See: https://github.com/astral-sh/uv/pull/6588.)

Closes https://github.com/astral-sh/uv/issues/6511.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-24 16:03_

---

_@charliermarsh reviewed on 2024-08-24 16:04_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:65 on 2024-08-24 16:04_

Unfortunately, we do need to set an explicit build backend for our test projects now. This is fine and correct, we were just omitting it before.

---

_Comment by @charliermarsh on 2024-08-24 16:05_

This needs documentation.

---

_Comment by @charliermarsh on 2024-08-24 16:07_

I wonder if we should use different terminology here to differentiate from a "virtual workspace root".

---

_Comment by @charliermarsh on 2024-08-24 16:13_

We might be better off with a concept of "applications" vs. "libraries" here?

---

_Comment by @charliermarsh on 2024-08-24 16:13_

Or, even better, we remove virtual workspace roots entirely.

---

_Comment by @charliermarsh on 2024-08-24 16:21_

I feel pretty good about this apart from the use of "virtual" here and the fact that a "virtual workspace root" is slightly different.

---

_Comment by @mkniewallner on 2024-08-24 18:42_

"virtual" seems a bit abstract to me, and not really self-explanatory about what it does.

Some background on this:
- PDM uses a [`distribution`](https://pdm-project.org/en/latest/usage/project/#library-or-application) boolean
- Poetry uses a [`package-mode`](https://python-poetry.org/docs/basic-usage/#operating-modes) boolean

From those two, I find Poetry's one to be a better fit, as "distribution" seems like this is only about preventing publishing a package.

What about using something a bit more explicit like `project-type` that could either be `application` or `package` (or `library`)? I personally find those concepts closer to what people are used to when creating Python projects. Using an enum also make it possible to add more values in the future (although I don't really see what else we could have).

---

_Comment by @charliermarsh on 2024-08-24 18:53_

My preferred framing here is application vs. library (in Rust it’s “binary” vs. “library”), but curious to hear from @zanieb on it when they’re back.

---

_Comment by @a-reich on 2024-08-25 13:21_

IIUC, [project] still must have name and version for virtual projects/applications - does it make sense to make them optional?

---

_Comment by @charliermarsh on 2024-08-25 13:42_

No, I still plan to require those. Otherwise, it’s not clear how to identify a given package in the lockfile.

---

_Comment by @a-reich on 2024-08-25 13:50_

Hmm ok, I guess I misunderstood the mental model, as it hadn’t occurred to me that a project was allowed to depend on an application.

---

_Comment by @charliermarsh on 2024-08-25 13:54_

Just depends on the semantics we want to allow!

---

_Comment by @philgyford on 2024-08-25 15:11_

> No, I still plan to require those. Otherwise, it’s not clear how to identify a given package in the lockfile.

Fair enough if there’s no technical way around it, but it feels very odd to require it for something like a website.

Even on my solo, personal sites I might commit and deploy several times a day, and having to decide on a version number every time (and remember to change it) would be a real pain. 

---

_Comment by @z3z1ma on 2024-08-25 17:54_

> Even on my solo, personal sites I might commit and deploy several times a day, and having to decide on a version number every time (and remember to change it) would be a real pain.

You dont need to. Set the fields once in pyproject toml. Since you are not building a package with a build backend, the values are moot. Just use them as metadata for your app. 

---

_Comment by @keysmashes on 2024-08-27 13:31_

I'm not sure that the "library"/"application" framing necessarily corresponds well to "should/should not be installable". You want a library to be an installable package, so that it can be depended upon; but you also (sometimes?) want an application to be an installable package, so that you can install and run it (say, `pipx install uv`).

---

_Comment by @minkooseo on 2024-08-27 16:16_

What about "non package" or "not a package" then? Not sure if it's a good flag name, but it's something we can use to explain what this is about. It's also a term used in #6578. 

Coming from the experience of always putting sources at the top, 'git push' and then 'git pull' at the server to serve web traffic, it was quite new to me that I should put code under src/project_name. I learned that I build a package and publish it that way, and I can create a package and install it on the server.

It is still a project for me, so I was searching the doc to understand what's a project then if mine is _virtual_. (I still don't know)

It is an application and not a library, but it is now clear to me that application can still be an installable package. It may create confusion for people with knowledge.

---

_Comment by @charliermarsh on 2024-08-27 16:18_

That's actually what we're using in this PR. The flag is `package = false` etc.

---

_Comment by @zanieb on 2024-08-27 16:27_

@minkooseo might be helpful to look at #6689 too, but we're going to push out a bunch of changes around this soon.

---

_@zanieb approved on 2024-08-27 16:45_

---

_Merged by @charliermarsh on 2024-08-27 17:42_

---

_Closed by @charliermarsh on 2024-08-27 17:42_

---

_Branch deleted on 2024-08-27 17:42_

---
