---
number: 8199
title: "feature: uvx for github repos"
type: issue
state: closed
author: sslivkoff
labels:
  - question
assignees: []
created_at: 2024-10-15T07:21:26Z
updated_at: 2025-09-17T02:10:30Z
url: https://github.com/astral-sh/uv/issues/8199
synced_at: 2026-01-10T01:24:25Z
---

# feature: uvx for github repos

---

_Issue opened by @sslivkoff on 2024-10-15 07:21_

`uvx` is an awesome feature. It would be very nice to have something like

`uvx <github_repo_url>`

to run packages/commits that are not on python pypi

Use cases
1) execute the latest commit of a package without needing to cut a pypi release
2) execute private github repos that cannot go on pypi
3) if able to execute specific commits, this would enable an efficient form of git bisect

---

_Comment by @snth on 2024-11-27 12:45_

I believe you can do this with the [`--from`](https://docs.astral.sh/uv/guides/tools/#commands-with-different-package-names) option, for example:

    uvx --from git+https://github.com/snth/markdown-crawler@uvx markdown-crawler

In general

    uvx --from git+https://github.com/<USER>/<REPO>@<TREEISH> <ENTRYPOINT>


---

_Label `question` added by @zanieb on 2024-11-27 15:17_

---

_Closed by @zanieb on 2024-11-27 15:17_

---

_Comment by @cbrnr on 2025-02-03 06:50_

Is there an easy way to check out a PR using its number? I know that it works by appending the branch name associated with the PR, but I'd find it easier to just use the PR number.

This works:

```
uvx --from git+https://github.com/cbrnr/mnelab@export-bv-events mnelab
```

But this doesn't (the PR corresponding to the `export-bv-events` branch is 417):

```
uvx --from git+https://github.com/cbrnr/mnelab/pr/417 mnelab
```

---

_Comment by @thatch on 2025-08-05 14:36_

> Is there an easy way to check out a PR using its number?

Yes, for github specifically see 
https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/checking-out-pull-requests-locally

Append `@pull/$num/head` as if it were a branch name.

---

_Comment by @richardkmichael on 2025-09-17 02:10_

The tools-related documentation does not mention this, but `--from` is not required, and neither is the command name if it matches the repository ("package") name.

This permits a tidy invocation to run the command named `bar`:

`uvx git+https://github.com/foo/bar`

---
