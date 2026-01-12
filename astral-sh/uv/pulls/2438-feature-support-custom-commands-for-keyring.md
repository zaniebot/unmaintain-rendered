```yaml
number: 2438
title: "feature:  Support custom commands for keyring"
type: pull_request
state: closed
author: BakerNet
labels: []
assignees: []
base: main
head: feature/custom-keyring-command
created_at: 2024-03-14T00:00:24Z
updated_at: 2024-03-15T16:11:19Z
url: https://github.com/astral-sh/uv/pull/2438
synced_at: 2026-01-12T16:05:02Z
```

# feature:  Support custom commands for keyring

---

_@BakerNet_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds ability to use a script or program other than `keyring` with the same interface as `keyring` to get password.

Based on suggestion/request from https://github.com/vlad-ivanov-name in #2254 

## Test Plan

<!-- How was it tested? -->

I created a script: `my-keyring.sh` with a couple variants
First:  Wrap actual keyring
```
#!/bin/bash
keyring get $2 $3
```
Second:  Wrap gcloud auth command
```
#!/bin/bash
# should verify domain here
gcloud auth application-default print-access-token --scopes="https://www.googleapis.com/auth/cloud-platform" --quiet
```
and tested on a project I knew to be working by replacing `--keyring-provider subprocess` with `--keyring-provider custom --custom-keyring-command ./my-keyring.sh` for both variants

---

_Assigned to @zanieb by @zanieb on 2024-03-14 00:02_

---

_Comment by @BakerNet on 2024-03-14 00:43_

Maybe using the keyring namespace isn't the desired way to go and just having a "custom auth script" option is better?  But since it was requested, I figured I'd throw up this easy PR.

The gcloud auth command script in the PR description `Test Plan` shows how a custom script can be an easy/nice way to generate tokens for GAR without having to rely on the python keyring dependency for those looking to migrate.

---

_Comment by @zanieb on 2024-03-14 23:24_

It's an interesting idea but I think it's too soon to include something like this.

Maybe some sort of custom authentication script makes sense in the future? Not sure. There's also requests like https://github.com/astral-sh/uv/issues/1369 I'd like to take into consideration for whatever workflow we come up with.

---

_Comment by @BakerNet on 2024-03-14 23:27_

> It's an interesting idea but I think it's too soon to include something like this.
> 
> Maybe some sort of custom authentication script makes sense in the future? Not sure. There's also requests like #1369 I'd like to take into consideration for whatever workflow we come up with.

Understandable - feel free to close.

---

_Closed by @zanieb on 2024-03-15 16:11_

---
