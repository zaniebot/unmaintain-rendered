---
number: 8413
title: Support for custom netrc location appears to be partially bugged.
type: issue
state: closed
author: EdgyMeshx
labels:
  - external
assignees: []
created_at: 2024-10-21T09:45:00Z
updated_at: 2024-10-21T14:13:08Z
url: https://github.com/astral-sh/uv/issues/8413
synced_at: 2026-01-10T01:24:28Z
---

# Support for custom netrc location appears to be partially bugged.

---

_Issue opened by @EdgyMeshx on 2024-10-21 09:45_

I have multiple logins for different work projects so a single `.netrc` doesn't cover my usecase.

Supplying `NETRC=/my/custom/.netrc` almost works, it is picked up when checking for `api.github.com` but then seems to get ignored when fetching from `github.com`.

```
$ NETRC=~/.netrc_custom uv lock -vvv                                                                                                                                                                                                                               
    0.001817s DEBUG uv uv 0.4.25
    0.002429s DEBUG uv_workspace::workspace Found workspace root: `/code/backend`
    0.002453s DEBUG uv_workspace::workspace Adding current workspace member: `/code/backend`
    0.002930s DEBUG uv::commands::project The virtual environment's Python version satisfies `Python ==3.11.*`
 uv_client::linehaul::linehaul 
    0.003258s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=backend @ file:///code/backend
    0.004335s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: backend @ file:///code/backend
    0.004688s   1ms DEBUG uv_workspace::workspace No workspace root found, using project root
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=common @ git+https://github.com/org/common.git@v0.1.1
    0.005164s   0ms DEBUG uv_git::resolver Fetching source distribution from Git: https://github.com/org/common.git
    0.005304s DEBUG uv_fs Acquired lock for `https://github.com/org/common`
 uv_git::source::fetch repository=https://github.com/org/common.git, rev=None
    0.007528s   2ms DEBUG uv_git::source Updating Git source `https://github.com/org/common.git`
    0.011735s   6ms DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/org/common/commits/v0.1.1
    0.230026s 224ms DEBUG uv_auth::middleware Checking netrc for credentials for https://api.github.com/repos/org/common/commits/v0.1.1
    0.230071s 224ms DEBUG uv_auth::middleware Found credentials in netrc file for https://api.github.com/repos/org/common/commits/v0.1.1
    0.434944s 429ms DEBUG uv_git::git Performing a Git fetch for: https://github.com/org/common.git
Username for 'https://github.com': 
```

As you can see it picks up netrc credentials for api.github.com, then fails when fetching. If I also put a `~/.netrc` with same credentials in it, it works fine at the git fetch step. So it appears that the `NETRC` environment variable is honored in one place but then ignored in the other.

---

_Comment by @EdgyMeshx on 2024-10-21 09:47_

contents of ~/.netrc_custom (and ~/.netrc when testing fallback works)
```
machine github.com
login EdgyMeshx
password PAT

machine api.github.com
login EdgyMeshx
password PAT
```

---

_Comment by @EdgyMeshx on 2024-10-21 10:10_

For completeness, output when no .netrc file exists or is provided.

```
    0.008606s   2ms DEBUG uv_git::source Updating Git source `https://github.com/org/common.git`
    0.012958s   6ms DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/org/common/commits/v0.1.1
    0.047237s  40ms DEBUG uv_auth::middleware No netrc file found
    0.047497s  40ms DEBUG uv_git::git failed to check github HTTP status client error (404 Not Found) for url (https://api.github.com/repos/org/common/commits/v0.1.1)
    0.047528s  40ms DEBUG uv_git::git Performing a Git fetch for: https://github.com/org/common.git
Username for 'https://github.com': 
```

---

_Comment by @zanieb on 2024-10-21 13:46_

Note we use a [library](https://github.com/gribouille/netrc) to manage netrc files so I'm not sure we'll be able to fix this — you might need to reproduce and open an issue up there.

---

_Label `upstream` added by @zanieb on 2024-10-21 13:46_

---

_Comment by @EdgyMeshx on 2024-10-21 13:55_

Wouldn't the bug somewhat lie in the way you are utilising it? It finds the supplied netrc file, then proceeds to either forget about it or not honor it the next time it is required. 

I don't speak rust well enough to dive into how this might be occurring. If the fault ends up lying in the library you are using, you would be better equipped to raise an issue on their repository with the details of what the actual bug entails.

---

_Comment by @zanieb on 2024-10-21 13:59_

So.. looking closer..

We don't read the `NETRC` variable ourselves, that logic is all in the library (hence my original suggestion to report he issue there).

Your failure is occurring during the git fetch

> uv_git::git Performing a Git fetch for: https://github.com/org/common.git

in which we call out the `git` CLI. That's why the netrc isn't read by uv — those requests are all done outside of our HTTP client. Presumably this means that `git` doesn't support alternative `NETRC` file locations?

---

_Comment by @EdgyMeshx on 2024-10-21 14:13_

Simple local test would seem to suggest the same:
```
NETRC=~/.netrc_meshx git fetch https://github.com/org/repo.git -vvv
Username for 'https://github.com':
```

Thanks for the help, I'll look into git credentials and see if I can solve my usecase that way.

---

_Closed by @EdgyMeshx on 2024-10-21 14:13_

---

_Referenced in [astral-sh/uv#8441](../../astral-sh/uv/issues/8441.md) on 2024-10-22 09:40_

---
