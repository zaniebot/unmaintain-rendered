```yaml
number: 6104
title: uv lock is extremely slow with google artifact registry
type: issue
state: closed
author: ewianda
labels:
  - performance
assignees: []
created_at: 2024-08-15T04:02:33Z
updated_at: 2025-08-22T11:16:54Z
url: https://github.com/astral-sh/uv/issues/6104
synced_at: 2026-01-10T03:23:52Z
```

# uv lock is extremely slow with google artifact registry

---

_Issue opened by @ewianda on 2024-08-15 04:02_

Before the merge of PR #5089, using uv lock with Google Artifact Registry was quite efficient. However, post-merge, the locking process for a project with around 200 requirements has drastically slowed down, now taking up to an hour. Additionally, the cache size has escalated to approximately 300 GB, which seems excessive.

As a comparison, reconfiguring the repository to use [PyPI](https://pypi.org/simple/) reduces the lock time to under 3 minutes without utilizing the cache.

Thank you for looking into this matter. I appreciate your efforts in maintaining and improving this tool.

---

_Comment by @charliermarsh on 2024-08-15 11:55_

Wow, interesting that it was so much faster before! Hmm, ok. We'll likely need to revert then. Is there any chance you could share `--verbose` logs of a lock step on Google Artifact Registry?


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-15 11:55_

---

_Label `performance` added by @charliermarsh on 2024-08-15 11:55_

---

_Comment by @ewianda on 2024-08-15 12:25_

Sure I can share the logs. Do you want me to run the lock to completion, or just for a while and stop 

---

_Comment by @charliermarsh on 2024-08-15 12:27_

A run to completion would be great, but it doesn't need to be the entire set of 200 requirements. Just a representative sample would be great. Ideally with `-vv`.

---

_Comment by @ewianda on 2024-08-15 12:42_

[uv.log](https://github.com/user-attachments/files/16625226/uv.log)

I ran it for just 1 package (apache-beam[gcs]) and it took almost a minute.

---

_Comment by @charliermarsh on 2024-08-15 13:06_

Thanks. It's such a shame that the registry doesn't support [PEP 658](https://peps.python.org/pep-0658/) _or_ range requests. I'm amazed by how many commercial registries don't at least support the standard.

---

_Comment by @morotti on 2024-08-15 16:03_

(not a uv dev)

In my experience, I've found that apache-beam and many google libraries have the issue of pinning highly specific versions of dependencies, notably `protobuf`. I often see pip running into troubles trying to resolve the massive dependency tree with deep conflicts.

it may be worth trying to install your package by forcing a specific version of protobuf. does that get the installation to resolve faster?
`manuv pip install yourpackage protobuf==4.25.4`
`manuv pip install yourpackage protobuf==3.20.3`
`manuv pip install yourpackage protobuf==5.27.3`

from the logs:

```    
4.831562s   4s  DEBUG uv_resolver::resolver Adding transitive dependency for apache-beam==2.58.0: protobuf>=3.20.3, <4.0.dev0 | >=4.1.dev0, <4.21.dev0 | >=4.22.dev0, <4.22.0 | >4.22.0, <4.23.dev0 | >=4.25.dev0, <4.26.0
```


---

_Comment by @ewianda on 2024-08-15 16:05_

I did do that, basically, I exported my requirment.txt with all the pinned version into pyproject.toml to see if that will limit the resolution but that didn't make a difference. 

---

_Comment by @charliermarsh on 2024-08-15 16:08_

> I did do that, basically, I exported my requirment.txt with all the pinned version into pyproject.toml to see if that will limit the resolution but that didn't make a difference.

Is this materially slower than when you `uv pip sync` those specific versions?

---

_Comment by @ewianda on 2024-08-15 16:13_

> > I did do that, basically, I exported my requirment.txt with all the pinned version into pyproject.toml to see if that will limit the resolution but that didn't make a difference.
> 
> Is this materially slower than when you `uv pip sync` those specific versions?

Not it is not. Though I am getting a hash mis match error

```
warning: `uv sync` is experimental and may change without warning
Resolved 83 packages in 14ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: httplib2==0.22.0
  Caused by: Hash mismatch for `httplib2==0.22.0`

Expected:
  sha256:d7a10bc5ef5ab08322488bde8c726eeee5c8618723fdb399597ec58f3d82df81

Computed:
  sha256:0f3338153b99b37ab0cdff4613b2b4f391e7b4cf9daa07a59f40c88a39b87d5a
```

---

_Comment by @charliermarsh on 2024-08-15 16:14_

Does your package have an invalid hash? Have you checked what the registry provides vs. the true hash of the distribution?

---

_Comment by @ewianda on 2024-08-15 16:31_

Hmm, acutally it is due to the way are setting up  google artifact registry

We have a Virtual repository that points to a standard repository ( internal packages) and to  pypi as a remote repository

It seems the virtual repository only provides the distribution (  sha256:d7a10bc5ef5ab08322488bde8c726eeee5c8618723fdb399597ec58f3d82df81) , while the remote repository has both wheel and sdist files.

TL;DR
The sync fails with virtual repositories and works with a remote repository on google artifact.

Note: Changing the lock process to use the remote-repository doesn't change the lock time.

---

_Closed by @charliermarsh on 2024-08-22 23:54_

---

_Closed by @charliermarsh on 2024-08-22 23:54_

---

_Comment by @kpal-lilt on 2025-08-22 11:16_

Regarding PEP 658 support for Python packages in Google Artifact Registry, there is already a long-standing issue: https://issuetracker.google.com/issues/300035693. Is there any way to increase pressure on Google to implement this?

It’s a pity that we have such improved tools like uv and cannot fully exploit them in enterprise environments. Perhaps uv could also display a warning when it encounters repositories that don’t support expected features, to better inform users.

---
