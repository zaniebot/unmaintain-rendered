```yaml
number: 16793
title: I/O operation failed during extraction
type: issue
state: open
author: wpcarro
labels:
  - question
assignees: []
created_at: 2025-11-20T20:31:39Z
updated_at: 2025-11-24T15:56:25Z
url: https://github.com/astral-sh/uv/issues/16793
synced_at: 2026-01-12T16:02:38Z
```

# I/O operation failed during extraction

---

_@wpcarro_

### Question

`uv add numcodecs` is failing, but I'm not sure how to troubleshoot the root cause

All I see is `Streaming failed for numcodecs==0.16.4; downloading wheel to disk (I/O operation failed during extraction)`

Any troubleshooting tips? I'd like to be able to self-service better

### Platform

Linux 6.12.22+bpo-amd64 x86_64 GNU/Linux

### Version

uv 0.8.14

---

_Label `question` added by @wpcarro on 2025-11-20 20:31_

---

_Comment by @nooscraft on 2025-11-21 03:33_

Yeah, that error message is pretty unhelpful, it's hiding the actual I/O error details. The underlying `std::io::Error` should tell us what's really going on, but it's not being surfaced properly. That's definitely something we should fix.

In the meantime, here's are a few troubleshooting steps:

**Disk space** - Check with:
```bash
df -h ~/.cache/uv
```
If it's full, either free up space or point uv to a different cache location:
```bash
UV_CACHE_DIR=/somewhere/with/space uv add numcodecs
```

**Permissions** - Quick test:
```bash
touch ~/.cache/uv/test && rm ~/.cache/uv/test
```
If that fails, the cache dir probably has wrong ownership. Fix it or use a different location.

**Corrupted cache** - The wheel might be borked in cache. Try:
```bash
uv cache clean numcodecs
uv add numcodecs
```

**Filesystem issues** - Check if the mount is weird or read-only:
```bash
mount | grep cache
touch ~/.cache/uv/test_write && rm ~/.cache/uv/test_write
```

To get the actual error details, run with backtrace:
```bash
RUST_BACKTRACE=1 uv add numcodecs 2>&1 | tee uv-error.log
```
That should show the real `std::io::Error` with the OS error code, which will tell us exactly what's failing.

You can also manually test the wheel to see if it's corrupted:
```bash
# Grab the wheel URL from PyPI
curl -O <wheel-url>
unzip -t <wheel-file>.whl
```

Start with the cache clean and disk space steps, and see how it goes. If it's still failing, paste the backtrace output



---

_Comment by @konstin on 2025-11-24 15:56_

The message you share should be a warning, can you share the complete output you get?

---
