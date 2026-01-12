```yaml
number: 8283
title: How to fetch packages from a private package repo (not git repo)
type: issue
state: closed
author: dbogdoll
labels:
  - question
assignees: []
created_at: 2024-10-17T07:04:33Z
updated_at: 2024-10-30T20:11:06Z
url: https://github.com/astral-sh/uv/issues/8283
synced_at: 2026-01-12T15:59:23Z
```

# How to fetch packages from a private package repo (not git repo)

---

_@dbogdoll_

Hi all,

I couldn't find how I can specify which package should be retrieved from which package repository.
Most of the packages are retrieved from pypi, but for some packages we would like to retrieve
them from a gitlab instance, either via a group URL or a project URL. How can I specify that?

In poetry we could add the different URLs and bind them to kind an identifier and then with poetry config
I could add then authentication to those identifiers. Is there something similiar in uv?

Thanks.

---

_Label `question` added by @charliermarsh on 2024-10-17 12:23_

---

_Comment by @charliermarsh on 2024-10-17 15:26_

Yeah we support a similar scheme -- check out the newly updated [index](https://docs.astral.sh/uv/configuration/indexes/) documentation.

---

_Comment by @charliermarsh on 2024-10-17 15:26_

It allows you to assign packages to indexes, define credentials in environment variables, etc.

---

_Comment by @dbogdoll on 2024-10-18 08:00_

That looks great, but could I suggest one improvement. In addition to the ENV variables, could you also add an mechanism to store them in a file somewhere centrally? Or could you add to uv the possibility to point to a .env file where it picks up additional env variables?

Tbh. I would like to have a user and also system wide configuration file, where we could store such informations or at least link to those informations.

---

_Comment by @charliermarsh on 2024-10-30 20:11_

You can already define indexes at the system or user level. See the docs: https://docs.astral.sh/uv/configuration/files/.

---

_Closed by @charliermarsh on 2024-10-30 20:11_

---
