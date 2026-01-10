```yaml
number: 2960
title: Support -e Flag in requirements.in for pip compile/install?
type: issue
state: closed
author: brberry
labels: []
assignees: []
created_at: 2024-04-10T14:08:45Z
updated_at: 2024-04-10T14:29:00Z
url: https://github.com/astral-sh/uv/issues/2960
synced_at: 2026-01-10T05:40:32Z
```

# Support -e Flag in requirements.in for pip compile/install?

---

_Issue opened by @brberry on 2024-04-10 14:08_

[uv 0.1.31, Windows]

Pip allows for entries in requirements.in like:

-e git+ssh://git@github.com/myusername/myrepo.git#egg=myrepo

uv does not. When executing either of these:

`uv pip install -r requirements.in`    
`uv pip compile requirements.in`    

it throws the error:    
`error: Unsupported URL (expected a file:// scheme) in requirements.in: git+ssh://git@github.com/myusername/myrepo.git`

Does uv support this and I'm just not finding the right params?

---

_Comment by @charliermarsh on 2024-04-10 14:09_

We don't support editable installs for HTTP or Git dependencies. Either of these work though:

```
# Non-editable.
git+ssh://[git@github.com](mailto:git@github.com)/myusername/myrepo.git#egg=myrepo

# Editable local dependency.
-e ./path/to/file
```

What's the use-case for an editable Git install?


---

_Comment by @brberry on 2024-04-10 14:29_

Well....good point. Editable local dependency definitely meets my needs. 

Thanks for the fast reply - you are just as fast as your programs.

---

_Closed by @brberry on 2024-04-10 14:29_

---
