```yaml
number: 11774
title: Adds Persistent fils logs to testing
type: pull_request
state: open
author: Choudhry18
labels: []
assignees: []
base: main
head: LOG_DIR
created_at: 2025-02-25T08:51:24Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11774
synced_at: 2026-01-10T11:10:39Z
```

# Adds Persistent fils logs to testing

---

_Pull request opened by @Choudhry18 on 2025-02-25 08:51_

## Summary

Adds persistent file logs to snapshot testing that can be viewed later.  This builds on to #11762  and attempts to resolve #8351 . This introduces a dedicated log directory that keeps logs from test runs, unless another directory is specified. 

## Test Plan

I have manually checked if there is a log file for each test file after running the tests. I do believe testing should be a bit more robust, however i am unsure how can that be done - check file logs against cli logs is not easily possible because of print statements in cli. 

## Discuss

I wanted to get more input on whether there should be log file per command invocation or other other frequency. Currently there is a log file for each test file. To implement one log for each test is quite simple but it requires tediously adding a `--log` flag to each test. 

---

_Assigned to @zanieb by @zanieb on 2025-02-26 15:44_

---
