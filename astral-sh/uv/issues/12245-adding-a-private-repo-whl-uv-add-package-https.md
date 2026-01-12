```yaml
number: 12245
title: "adding a private repo whl `uv add package @ https://github.com/ORG/PRIVATE-REPO/releases/download/...whl`"
type: issue
state: open
author: rllin
labels:
  - question
assignees: []
created_at: 2025-03-17T18:40:12Z
updated_at: 2025-09-17T23:47:22Z
url: https://github.com/astral-sh/uv/issues/12245
synced_at: 2026-01-12T16:00:58Z
```

# adding a private repo whl `uv add package @ https://github.com/ORG/PRIVATE-REPO/releases/download/...whl`

---

_@rllin_

### Question

is this meant to work?
```bash
  × Failed to download `XXX @ http://github.com/XXX/XXX-private/releases/download/XXX.whl`
  ├─▶ Failed to fetch: `http://github.com/XXX/XXX-private/releases/download/XXX.whl`
  ╰─▶ HTTP status client error (404 Not Found) for url (https://github.com/XXX/XXX-private/releases/download/XXX.whl)
```
I confirmed when I click on the link, it'll trigger a download in my browser. I think this is an issue with auth

is this where maybe a private index url helps?  but its not obvious what the index url should be?

I also tried `git+ssh://git@github.com/...`

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @rllin on 2025-03-17 18:40_

---

_Comment by @rllin on 2025-03-17 19:08_

i think what i'm looking for is something like `#subdirectory=` or `--tag` or `--rev` but `--releases`

https://docs.astral.sh/uv/concepts/projects/dependencies/#git

---

_Comment by @charliermarsh on 2025-03-18 19:59_

Are you trying to install a pre-built wheel within a GitHub repository? Or are you trying to install the GitHub repository itself, as a project?

---

_Comment by @konstin on 2025-03-28 18:06_

> I confirmed when I click on the link, it'll trigger a download in my browser. I think this is an issue with auth

If you're logged in in your browswer, it has a login cookie that it sends for each request, authenticating the download. For uv, you need to create a token in git and pass it as an HTTP request auth token (https://docs.astral.sh/uv/configuration/authentication/#git-authentication). You might also need to use `https:` over `http:`.

---

_Comment by @keshavbabu on 2025-09-17 23:47_

Hey have this same question as well.

I'm trying to pull a wheel from Github releases on a private repo as a dependency. Notably, I want to avoid building the project and just include the wheel that is attached to a Github release as an asset.

---
