```yaml
number: 9148
title: Git sub dependency
type: issue
state: closed
author: johanolsen
labels: []
assignees: []
created_at: 2024-11-15T13:26:32Z
updated_at: 2024-11-15T13:41:28Z
url: https://github.com/astral-sh/uv/issues/9148
synced_at: 2026-01-12T15:59:43Z
```

# Git sub dependency

---

_@johanolsen_

I have a project with a couple of dependencies (let's call them `foo` and `bar`) that are both in a private gitlab instance - it is ssh only, so I added the dependencies with `uv add git+ssh...` and it worked initially, but has started causing problems. Specifically, `foo` is also a dependency of `bar`, and while adding `foo` works like a charm, adding `bar` gives me the below error
```
 Updated ssh://gitlab@gitlab.domain.net/group/bar (91399c7)
⠋ Resolving dependencies... 
Updating ssh://gitlab.domain.net/group/foo (HEAD)                                                                                                 
× Failed to download and build `foo @ git+ssh://gitlab.domain.net/group/foo`
  ├─▶ Git operation failed
  ├─▶ failed to fetch into: /Users/jpko/.cache/uv/git-v0/db/5d09cd4d890de77e
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'ssh://gitlab.domain.net/group/foo'
      '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      fatal: '/group/foo' does not appear to be a git repository
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
```
It looks like there's som confusion with cloning into the `uv` cache, but I'm not sure...? I'm on a mac and just reinstalled `uv 0.5.2` with the standalone installer. Thanks

---

_Comment by @johanolsen on 2024-11-15 13:41_

Sorry, this may have been a brain fart on my end. Closing

---

_Closed by @johanolsen on 2024-11-15 13:41_

---
