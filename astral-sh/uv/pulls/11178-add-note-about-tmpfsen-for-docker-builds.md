```yaml
number: 11178
title: Add note about tmpfsen for Docker builds
type: pull_request
state: closed
author: akx
labels: []
assignees: []
base: main
head: tmp-doc
created_at: 2025-02-03T09:45:56Z
updated_at: 2025-04-16T15:23:49Z
url: https://github.com/astral-sh/uv/pull/11178
synced_at: 2026-01-10T11:10:34Z
```

# Add note about tmpfsen for Docker builds

---

_Pull request opened by @akx on 2025-02-03 09:45_

## Summary

Documents `--mount=type=tmpfs,target=/tmp` as a workaround/fix for stray lockfiles, etc., as discussed in #10773.

## Test Plan

A crack team of highly trained marmots equipped with adorable pince-nez glasses took multiple critical looks at this text and found nothing alarming about it.

---

_Assigned to @zanieb by @zanieb on 2025-02-03 14:06_

---

_Comment by @zanieb on 2025-02-10 22:07_

We're looking at avoiding writing these to `/tmp`

---

_Comment by @akx on 2025-02-11 08:17_

> We're looking at avoiding writing these to `/tmp`

Are you planning to write them into e.g. somewhere under the `venv` directory..? That'd be worse, since if, as discussed in #10773, there's no reliable way to delete them after use, the Dockerfile author would have to clean them up with a `find` incantation or similar afterwards...

---

_Comment by @zanieb on 2025-02-11 15:16_

We're considering both co-located (i.e., in the virtual environment) or in a dedicated alternative directory. Can you explain _why_ this file needs to be deleted? I don't see any problem with this file being present in the virtual environment — and it seems dubious that one more file there is going to cause measurable overhead?

---

_Comment by @akx on 2025-02-11 19:41_

> Can you explain why this file needs to be deleted?

Because it shouldn't be there when it's not needed. 😅 

> and it seems dubious that one more file there is going to cause measurable overhead?

Believe it or not, every file in a Docker image has measurable impact, since the extraction of an image layer into the filesystem is serialized. 

From an experiment I did last year while investigating https://github.com/moby/moby/issues/48601; two images, the exact same size, with 2 layers, from a local registry (so bandwidth doesn't matter):

```
localhost:5000/x-small         latest      008ff0304c6d   528MB
localhost:5000/x-large         latest      2b9b389c20e1   528MB
```
The output of `hyperfine` comparing `docker pull` speed:

```
  docker rmi -f localhost:5000/x-large && docker pull localhost:5000/x-large ran
    1.10 ± 0.09 times faster than docker rmi -f localhost:5000/x-small && docker pull localhost:5000/x-small
```

The only difference between the images is that one has 1000 small files, the other has 5 large files.

Given enough files (e.g. .pyc files, see #7696) this sort of thing does compound, especially in the cloud, where what might seem like a local disk really is not.

**EDIT:** Another place where extra files really do hurt is e.g. distributed filesystems like Lustre. [LUMI, the fastest computer in Europe, advises against Python virtualenvs due to large-number-of-small-files.](https://docs.lumi-supercomputer.eu/software/installing/python/#discouraged-installation-methods)

---

_Comment by @zanieb on 2025-02-11 22:31_

> The only difference between the images is that one has 1000 small files, the other has 5 large files.

We're not talking about a thousand small files here though, we're talking about one lockfile per mutated Python environment — which as you note already have _many_ small files.

---

_Comment by @zanieb on 2025-04-16 15:23_

I think I'm going to bias towards keeping the guide simple for now unless we see more people that have problems here.

---

_Closed by @zanieb on 2025-04-16 15:23_

---
