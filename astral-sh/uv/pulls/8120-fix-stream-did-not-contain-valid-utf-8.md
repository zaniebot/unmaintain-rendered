```yaml
number: 8120
title: Fix stream did not contain valid UTF-8
type: pull_request
state: merged
author: kahojyun
labels: []
assignees: []
merged: true
base: main
head: fix-utf8
created_at: 2024-10-11T04:52:49Z
updated_at: 2024-10-12T02:50:48Z
url: https://github.com/astral-sh/uv/pull/8120
synced_at: 2026-01-10T12:54:03Z
```

# Fix stream did not contain valid UTF-8

---

_Pull request opened by @kahojyun on 2024-10-11 04:52_

## Summary

Related issues: #8009 #7549

Although `PYTHONIOENCODING=utf-8` forces python to use UTF-8 for `stdout`/`stderr`, it can't prevent code like `sys.stdout.buffer.write()` or `subprocess.call(["cl.exe", ...])` to bypass the encoder. This PR uses lossy UTF-8 conversion to avoid decoding error.

## Alternative

Using `bstr` crate might be better since it can preserve original information. Or we should follow the Windows convention, unset `PYTHONIOENCODING` and decode with system default encoding.

## Test Plan

Running locally with non-ASCII character in `UV_CACHE_DIR` works fine, but I have no unit test plan. Testing locale problem is hard :(

---

_Comment by @charliermarsh on 2024-10-11 08:32_

\cc @BurntSushi 

---

_Review comment by @BurntSushi on `crates/uv-build-frontend/src/lib.rs`:962 on 2024-10-11 12:40_

First thought upon seeing this was thinking, "but what about `\r`?" But it looks like you handled that above. Nice. :-)

---

_Review comment by @BurntSushi on `crates/uv-build-frontend/src/lib.rs`:932 on 2024-10-11 13:07_

I do think `bstr` would be better for this, and would suggest using it (since we already depend on it, albeit transitively, in uv), but my memory here is that we need to write valid UTF-8. So while `bstr` wouldn't require this "lossy" step, I believe [writing to the underlying stderr would still require valid UTF-8 anyway](https://github.com/rust-lang/rust/blob/ce697f919d88b143d70312c0e7f3952fcb9869db/library/std/src/sys/pal/windows/stdio.rs#L102-L138). So `bstr` would just absolve you of needing to handle line iteration, but I think this is fine as-is.

---

_@BurntSushi approved on 2024-10-11 13:07_

---

_Merged by @BurntSushi on 2024-10-11 13:10_

---

_Closed by @BurntSushi on 2024-10-11 13:10_

---

_Comment by @Super1Windcloud on 2024-10-12 02:50_

@charliermarsh   
 # please provide next fixed version  as  soon as  possible  

---
