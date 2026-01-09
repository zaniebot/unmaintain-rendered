---
number: 17266
title: "0.9.20: DNS failures take ~4x longer due to nested retries"
type: issue
state: closed
author: shayonj
labels:
  - bug
assignees: []
created_at: 2025-12-30T14:24:16Z
updated_at: 2026-01-05T20:06:40Z
url: https://github.com/astral-sh/uv/issues/17266
synced_at: 2026-01-07T13:12:19-06:00
---

# 0.9.20: DNS failures take ~4x longer due to nested retries

---

_Issue opened by @shayonj on 2025-12-30 14:24_

## Summary

DNS failures that took ~4 seconds in 0.9.18 now take ~17-19 seconds in 0.9.20. Looking at debug logs, it appears the retry logic is running at two layers now (middleware + cached client), causing way more retries than expected I think?

This was caught from our CI specs because we have a 10 second timeout waiting for a Python harness to start when dealing malformed PyPi clients or similar. 

## Minimal reproduction

```bash
# Setup
mkdir -p /tmp/uv-test && cd /tmp/uv-test
mkdir -p packages
pip download six==1.17.0 --dest packages -q

cat > uv.toml << 'EOF'
[pip]
find-links = ["./packages"]
index-url = "http://invalid-hostname-xyz123"
EOF

echo "six==1.17.0" > requirements.txt
uv venv .venv -q

# Time it
time UV_CONFIG_FILE=./uv.toml uv pip install -r requirements.txt --python .venv/bin/python
```

**0.9.18:** ~4 seconds
**0.9.20:** ~17-19 seconds

## What I'm seeing

With `RUST_LOG=debug`, 0.9.18 shows the expected pattern - middleware retries 3 times, then fails:

```
WARN Retry attempt #0. Sleeping 494ms before the next attempt
WARN Retry attempt #1. Sleeping 1.27s before the next attempt
WARN Retry attempt #2. Sleeping 1.57s before the next attempt
error: Request failed after 3 retries
```

But 0.9.20 shows the middleware retrying, then the cached client layer triggers _another_ round:

```
WARN Retry attempt #0. Sleeping 897ms before the next attempt
WARN Retry attempt #1. Sleeping 1.18s before the next attempt
WARN Retry attempt #2. Sleeping 1.41s before the next attempt
DEBUG Transient failure while handling response...; retrying after 0.1s...   <-- starts over
WARN Retry attempt #0. Sleeping 981ms before the next attempt
...
DEBUG Transient failure while handling response...; retrying after 0.1s...   <-- and again
WARN Retry attempt #0. Sleeping 815ms before the next attempt
...
DEBUG Transient failure while handling response...; retrying after 2.6s...   <-- and again
...
error: Request failed after 3 retries
```

Ends up doing 16+ attempts instead of 4.

## Platform

macOS 14.3.0 arm64

## Version

Tested on:

- uv 0.9.18 (0cee76417 2025-12-16) - works as expected
- uv 0.9.20 (765a96723 2025-12-29) - regression?
## Version

Tested on:

- uv 0.9.18 (0cee76417 2025-12-16) - works as expected
- uv 0.9.20 (765a96723 2025-12-29) - regression

## Likely cause

The timing lines up with #17104 and #17105 which unified retry handling. It looks like both the middleware (`RetryTransientMiddleware`) and the cached client (`RetryState::should_retry`) now independently retry transient errors, creating nested retry loops.

## Potential fix

I've tested locally that having `RetryState::should_retry` return `None` for transient errors (since middleware already handles those) fixes the issue while keeping all existing tests passing. Happy to submit a PR if helpful.


### Platform

macOS 14.3.0 arm64, Ubuntu/Debian

### Version

uv 0.9.20

### Python version

Python 3.14.0

---

_Label `bug` added by @shayonj on 2025-12-30 14:24_

---

_Comment by @zanieb on 2025-12-30 14:38_

Thanks for the report and research!

I believe @konstin's intent with that refactor was to avoid nested retries, he'll probably have an opinion on the fix.

---

_Referenced in [astral-sh/uv#17267](../../astral-sh/uv/pulls/17267.md) on 2025-12-30 14:50_

---

_Comment by @shayonj on 2025-12-30 14:51_

sounds good, thank you for the quick response. I was experimenting with something like this - https://github.com/astral-sh/uv/pull/17267 and the replication script above and seems to have fixed. Happy to iterate on the PR with feedback or also close it in favor of a different change. Thanks!

---

_Comment by @EliteTK on 2025-12-30 15:26_

For a slightly shorter reproducer:

```
uv init test
cd test
uv venv
uv pip install --index-url http://domain.invalid package
```

---

_Referenced in [astral-sh/uv#17274](../../astral-sh/uv/pulls/17274.md) on 2025-12-30 17:58_

---

_Comment by @EliteTK on 2025-12-30 17:59_

Just wanted to track it down but it was a quick fix.

---

_Closed by @EliteTK on 2026-01-05 20:06_

---
