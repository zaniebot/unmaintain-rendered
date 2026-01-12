```yaml
number: 7869
title: UV will add and install packages that are not compatible with current python version
type: issue
state: open
author: chris48s
labels:
  - compatibility
  - needs-design
  - needs-decision
assignees: []
created_at: 2024-10-02T15:01:43Z
updated_at: 2025-02-01T23:30:32Z
url: https://github.com/astral-sh/uv/issues/7869
synced_at: 2026-01-12T15:59:17Z
```

# UV will add and install packages that are not compatible with current python version

---

_@chris48s_

First off, apologies if this has already been reported. I did have a look over the tracker and could not find a similar issue. I am using `uv==0.4.18`. The problem I have hit is that uv will silently add and install packages that are not compatible with the project's python version without throwing an error.

So for example, if I have a project that uses python 3.12 I can `uv add geojson==3.0.0` (this package isn't compatible with python 3.12) without error. I can run manually run `uv pip check` after installing and that will tell me that

```
The package `geojson` requires Python >=3.7, <=3.11, but `3.12.3` is installed
```

(or whatever) but in my opinion, trying to `uv add`, `uv lock`, `uv sync` etc should throw a hard error if I try to add/lock/sync packages that are not compatible with the project python version. The behaviour should be similar to the "No solution found when resolving dependencies" error seen when trying to install 2 incompatible packages.

To demonstrate the problem, I have a made a small repo at https://github.com/chris48s/uv-repro with a minimal UV/python 3.12 in it and a GitHub Actions Workflow that demonstrates the issue https://github.com/chris48s/uv-repro/blob/main/.github/workflows/build.yml
Here's a link to a build: https://github.com/chris48s/uv-repro/actions/runs/11146126874/job/30977334223

If you can't see the individual outputs of the workflow steps, you can fork or copy the repo, push it up and run it yourself to get a repro you can inspect. That said, it should hopefully be pretty easy to reproduce locally.

---

_Comment by @Ravencentric on 2024-10-02 15:14_

This looks more like a `uv pip check` bug. https://pypi.org/project/geojson/#data explicitly declares 3.12 support via the trove classifier and defines the python requirement as `>=3.7`

---

_Comment by @zanieb on 2024-10-02 15:16_

This may be the same as https://github.com/astral-sh/uv/issues/7462#issuecomment-2356029053

---

_Comment by @Ravencentric on 2024-10-02 15:19_

In this case, geojson doesn't define an upper bound

---

_Comment by @zanieb on 2024-10-02 15:22_

It does here https://inspector.pypi.io/project/geojson/3.0.0/packages/ce/f1/44737e1da63e7b0ccd3f89dd95c352f4eb73e1a4ed595cf6e0a1f032a6c5/geojson-3.0.0-py3-none-any.whl/geojson-3.0.0.dist-info/METADATA#line.25

---

_Comment by @Ravencentric on 2024-10-02 15:24_

You're right! I was looking at the latest pypi release and the upper bound got removed there. But 3.0.0 still has it.

---

_Label `question` added by @zanieb on 2024-10-02 15:25_

---

_Comment by @chris48s on 2024-10-02 15:53_

Yes. Note I picked explicitly `geojson==3.0.0` here for this repro because I was looking for something which has an upper bound constraint. In this case `<=3.11`

I get that there are a lot of packages out there with a caret constraint on python-requires (e.g: `^3.7`) that implies `<4` (perhaps unintentionally) and it can be pragmatic to consider `python >=3.7` and `python >=3.7, <4` to be "the same", particularly when the project being managed is a package rather than an application.

A few thoughts:

- Even if you're going to say ignoring `<4` is pragmatic, a package that explicitly declares `>=3.7, <=3.11` (for example) should perhaps be considered differently?
- Ignoring upper-bound python constraints should be configurable IMO. Ideally you should opt in to ignoring it, but it should at least be possible to opt-out of this behaviour. Is there any option or configuration for this?
- In the context where UV is managing an application with one target python version to install/deploy on rather than a package (which targets a range of python versions), not installing (or at least warning about) packages that have explicitly declared they are incompatible with that version seems particularly key.

That said, I'm familiar with _the literature_ :books: 

- https://iscinumpy.dev/post/bound-version-constraints/#pinning-the-python-version-is-special
- https://discuss.python.org/t/requires-python-upper-limits/12663

etc

---

_Label `question` removed by @zanieb on 2024-10-21 21:58_

---

_Label `needs-design` added by @zanieb on 2024-10-21 21:58_

---

_Label `needs-decision` added by @zanieb on 2024-10-21 21:58_

---

_Label `compatibility` added by @zanieb on 2024-10-21 21:58_

---

_Comment by @dimbleby on 2025-02-01 23:30_

Linking #4022

---
