```yaml
number: 15058
title: "Enable `uv run` with a GitHub Gist"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/gist
created_at: 2025-08-04T14:04:15Z
updated_at: 2025-08-08T11:51:55Z
url: https://github.com/astral-sh/uv/pull/15058
synced_at: 2026-01-10T06:44:33Z
```

# Enable `uv run` with a GitHub Gist

---

_Pull request opened by @charliermarsh on 2025-08-04 14:04_

## Summary

You can now run `uv run https://gist.github.com/charliermarsh/ea9eab7f56b1b3d41e51960001cae31d` to execute a single-file Gist without having to go in and copy the raw URL.


---

_@zanieb approved on 2025-08-04 21:20_

Seems nice to have a test case.

---

_Label `enhancement` added by @zanieb on 2025-08-04 21:20_

---

_Marked ready for review by @charliermarsh on 2025-08-04 21:45_

---

_Merged by @charliermarsh on 2025-08-04 23:38_

---

_Closed by @charliermarsh on 2025-08-04 23:38_

---

_Branch deleted on 2025-08-04 23:38_

---

_Comment by @jakeswenson on 2025-08-06 14:14_

Would also be nice if this would work with GitHub enterprise gists as well, maybe by environment variables like `gh` does (`GH_HOST`).

---

_Comment by @charliermarsh on 2025-08-06 14:44_

With GHE though, can we know if the URL is `https:// <hostname>/gist` or `https:// gist.<hostname>`? It seems like that's configurable?

---

_Comment by @silverwind on 2025-08-07 15:59_

I could see this being extended to `uv run <url>` where it will fetch the url and execute it. This will work with all hosting services that offer a "raw" url, including Gist and is unaffected by GitHub API rate limiting.

---

_Comment by @zanieb on 2025-08-07 16:22_

We already support that.

---

_Comment by @silverwind on 2025-08-08 09:20_

Ah, I wouldn't have guessed by the output of `uv run --help`:

```
Usage: uv run [OPTIONS] [COMMAND]
```

The help text does not indicate at all what values `COMMAND` accepts.

---

_Comment by @zanieb on 2025-08-08 11:27_

Sounds like you want the long help... `uv help run`

---

_Comment by @silverwind on 2025-08-08 11:51_

Ah yes, the long help does mention it, thanks.

---
