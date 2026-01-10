```yaml
number: 10186
title: "`uv publish` raise error `Caused by: error decoding response body`"
type: issue
state: closed
author: andrew000
labels:
  - bug
assignees: []
created_at: 2024-12-26T23:20:40Z
updated_at: 2024-12-26T23:40:26Z
url: https://github.com/astral-sh/uv/issues/10186
synced_at: 2026-01-10T04:36:21Z
```

# `uv publish` raise error `Caused by: error decoding response body`

---

_Issue opened by @andrew000 on 2024-12-26 23:20_

Using `astral-sh/setup-uv@v5` and UV_VERSION `latest` or `0.5.12` causes error when publishing package: https://github.com/andrew000/FTL-Extract/actions/runs/12509313775/job/34898612613#step:8:12

Using UV_VERSION `0.5.11` works good:
https://github.com/andrew000/FTL-Extract/actions/runs/12509365391/job/34898720613

---

_Comment by @Frazzer951 on 2024-12-26 23:23_

I'm also getting the same error when trying to install a package through an AWS code-artifact. `0.5.12` fails while `0.5.11` works fine. 

---

_Comment by @charliermarsh on 2024-12-26 23:24_

Thank you. @andrew000 -- do you know if you intend to be using trusted publishing?

(@Frazzer951 -- I assume you mean _publishing_? Or are you seeing a different issue?)

---

_Comment by @charliermarsh on 2024-12-26 23:27_

It looks like this might be due to a change in `reqwest`. I will likely revert and publish a new version: https://github.com/seanmonstar/reqwest/commit/d36c0f5fd93f8190c9f39990ce4ec859c2b6d567

---

_Comment by @Frazzer951 on 2024-12-26 23:28_

@charliermarsh Mine occurs when doing a `uv pip install`
```sh
> uv pip install -e ".[dev]" --index-url https://<<AWS CodeArtifact Url>>
Using Python 3.11.9 environment at: /Users/luke/.pyenv/versions/3.11.9/envs/analytics
  × Failed to build `pipeline @ file:///Users/luke/Sonarverse/analytics`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `setuptools`
  ├─▶ Failed to fetch: `https://<<AWS CodeArtifact Url>>/setuptools/`
  ├─▶ error decoding response body
  ╰─▶ there are extra bytes after body has been decompressed
```


---

_Comment by @charliermarsh on 2024-12-26 23:28_

Got it, thanks. I'll go ahead and revert!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-26 23:31_

---

_Label `bug` added by @charliermarsh on 2024-12-26 23:31_

---

_Closed by @charliermarsh on 2024-12-26 23:40_

---

_Closed by @charliermarsh on 2024-12-26 23:40_

---
