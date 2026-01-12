```yaml
number: 15883
title: "Add `UV_NO_SOURCES` as an environment variable"
type: pull_request
state: merged
author: BlairAllan
labels: []
assignees: []
merged: true
base: main
head: blairallan/env-no-sources
created_at: 2025-09-15T22:52:44Z
updated_at: 2025-11-02T20:25:20Z
url: https://github.com/astral-sh/uv/pull/15883
synced_at: 2026-01-12T16:12:00Z
```

# Add `UV_NO_SOURCES` as an environment variable

---

_@BlairAllan_

## Summary

This is an enhancement that makes the cli flag `--no-sources` an environment variable - "UV_NO_SOURCES"

Why is this a relevant change? 

When working across different environments, in our case remote vs local, we often have our packages hosted in a artifact registry but when developing locally we build our packages from github.  This results in us using the uv.tool.sources table quite a bit however this then also forces us to use `--no-sources` for all our remote work. 

This change enables us to set an environment variable once and to never have to type --no-sources after every uv run command again.

## Test Plan

Expanded on the current --no-sources tests, to test when UV_NO_SOURCES=true/false the behaviour is the same as the flag. Additionally ensured that the cli overrides the env variable.


---

_Review requested from @zanieb by @konstin on 2025-09-16 07:27_

---

_@charliermarsh approved on 2025-11-02 20:13_

---

_Merged by @charliermarsh on 2025-11-02 20:25_

---

_Closed by @charliermarsh on 2025-11-02 20:25_

---
