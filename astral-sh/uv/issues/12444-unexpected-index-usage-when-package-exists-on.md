---
number: 12444
title: unexpected index usage when package exists on multiple indices
type: issue
state: closed
author: hutch3232
labels:
  - bug
assignees: []
created_at: 2025-03-24T17:56:14Z
updated_at: 2025-03-24T18:17:27Z
url: https://github.com/astral-sh/uv/issues/12444
synced_at: 2026-01-10T01:25:19Z
---

# unexpected index usage when package exists on multiple indices

---

_Issue opened by @hutch3232 on 2025-03-24 17:56_

### Summary

Hello - first of all, thanks for the incredible work you all have done. `uv` is amazing.

I'm using `uv` 0.6.9.

I use artifactory for my python package indices. I use `.netrc` for authentication (I don't think I have any issues there).

In my `~/.config/uv/uv.toml` I specify: `index-url`, `extra-index-url`, and `python-install-mirror`. `index-url` is the artifactory mirror of pypi. `extra-index-url` just has one index which is kind of an "in-house" python package repo.

For some reason, someone uploaded `requests` and `urllib3` to our in-house package repo. I noticed that `uv` really wants to use the `requests` and `urllib3` package from my `extra-index-url`. I would have guessed it would default to `index-url` which seems to me more like a primary index - this is what I would like.

Technically, I can use the suggested `--index-strategy` option to be `unsafe-best-match` but then I have to use that all the time and/or configure it in many projects.

I also tried to "yank" the package (since the user just uploaded one version of each) - I hoped that would encourage `uv` to ignore that index (since there were no non-yanked packages). That didn't work. 

I could delete these packages, but I think it would impact people who already added them to their lock files.

Seems related to #9008 

### Platform

Ubuntu 20.04 amd 64bit

### Version

0.6.9

### Python version

Python 3.9.18 (system installed)

---

_Label `bug` added by @hutch3232 on 2025-03-24 17:56_

---

_Comment by @hutch3232 on 2025-03-24 18:17_

Nevermind - this is sufficiently documented here: https://docs.astral.sh/uv/configuration/indexes/

---

_Closed by @hutch3232 on 2025-03-24 18:17_

---
