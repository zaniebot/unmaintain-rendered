```yaml
number: 8541
title: ":sparkles: use UV_SYNC_ALL_EXTRAS env var in uv sync command"
type: pull_request
state: open
author: danielgafni
labels: []
assignees: []
base: main
head: sync-all-extras-config
created_at: 2024-10-24T20:16:54Z
updated_at: 2025-04-29T19:46:43Z
url: https://github.com/astral-sh/uv/pull/8541
synced_at: 2026-01-12T16:08:21Z
```

# :sparkles: use UV_SYNC_ALL_EXTRAS env var in uv sync command

---

_@danielgafni_

## Summary

`uv sync` CLI command now respects `UV_SYNC_ALL_EXTRAS` environment variable.

---

Is can also add a config option if you think it makes sense to have one. 

---

_Review requested from @charliermarsh by @zanieb on 2024-10-24 20:21_

---

_@konstin approved on 2024-10-30 11:26_

---

_Comment by @zanieb on 2024-10-30 12:50_

Just wondering, what's the use-case for this?

---

_Comment by @danielgafni on 2024-10-30 14:23_

Multiple `uv sync` invocations in a single `Dockerfile`

---

_Comment by @zanieb on 2024-10-30 14:26_

Why are you making multiple invocations where `--all-extras` matters? Just trying to understand the use-case because this isn't the kind of thing we've been exposing as an environment variable.

---

_Comment by @danielgafni on 2024-10-30 15:21_

I don't think the use cases are super solid. It's more like a nice to have feature. 

Imo ideally all CLI arguments should have corresponding env variables. Is your opinion different or you are just looking for clarification?

This just make usage easier in some (probably rare) cases: doing multiple invocations with other arguments varying in docker/CI, controlling this behavior in CI pipelines without changing the CLI command (CI pipelines often pass env variables to each other), setting a development default for a project (what actually made me start this PR).

The latter should really be a config parameter tho, but as a Rust newbee I decided to start from something more straightforward like an env variable.  

---

_Comment by @zanieb on 2024-10-30 15:26_

> Imo ideally all CLI arguments should have corresponding env variables. Is your opinion different or you are just looking for clarification?

I think this is actually against our philosophy. I don't think every CLI argument should have a corresponding variable — only ones that configure something you'd commonly want to set as persistent state.

It sounds more like your use-case here would be solved by a `default-extras` setting.

---

_Comment by @danielgafni on 2024-10-30 15:30_

Understood, let me know if you still want this PR

---

_Comment by @konstin on 2025-04-28 12:50_

Would this be covered by https://github.com/astral-sh/uv/pull/12965?

---

_Comment by @danielgafni on 2025-04-29 19:46_

I don't think so --- originally I needed this when building a docker image for a uv workspace with multiple internal extras. So I didn't want this to be a part of the global config, but rather a one time settings just for the docker image. 

---
