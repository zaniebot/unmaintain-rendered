```yaml
number: 11762
title: Add logging output to file functionality 
type: pull_request
state: open
author: Choudhry18
labels: []
assignees: []
base: main
head: cli_logs
created_at: 2025-02-25T03:35:13Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11762
synced_at: 2026-01-10T11:10:38Z
```

# Add logging output to file functionality 

---

_Pull request opened by @Choudhry18 on 2025-02-25 03:35_

## Summary

This would introduce the ability to log traces to a log file for a user to view later - closes #9850 .  

## Test Plan

I have done manual testing to check the log file against the trace output on terminal. I have also added some snap shot tests to ensure the `--log` invocation and the subsequent "See <log_file> for details" message behaves as intended.

## Requirements Discussion

There are some requirements and implementation details that need to be discussed as they depend on users would want to use this. As a lot of people who would use this feature are contributors, trying to log while testing it would great to get their input on how they would like to use this. I have added comments that start with "Discuss" to indicate these but here are some things that need to be discussed:

1. If user specifies a log file that already exists, should that result in failure, overwrite existing content or append to it (current implementation.
2. Should the log file also contain print statements - user facing output or only logs from `tracing` crate (current)
3. Should the log file also contain color just like the logs to terminal or no color (currently this and I lean towards this as text editors that I use to view logs (VI and console on mac both don't support ansi be default)
4. Should the hierarchical tree logs and the level of logs that are written to file logs be separated - introduce the equivalent of `RUST_LOG` for file logs, currently they are determined the count of `--log_verbose` flag ranging from 0 to 3
5. Should the "See <log_file> for detailed logs" appear on every invocation where `--log` is set or only on errors and failure exits. 

---

_Assigned to @zanieb by @zanieb on 2025-02-26 18:21_

---

_Comment by @zanieb on 2025-02-26 22:19_

Sweet! I'll try to review this soon.

---

_Comment by @Choudhry18 on 2025-03-25 03:46_

Hey @zanieb , just checking in to see if this is still in your review queue and if there's anything on my end that might be causing a delay.

---

_Comment by @zanieb on 2025-03-26 19:45_

Sorry for the delay, I'm traveling a lot this month — there's not something on your end blocking me.

---
