```yaml
number: 9137
title: "`tool_run_cache` is flaky on Windows"
type: issue
state: closed
author: charliermarsh
labels:
  - testing
  - windows
assignees: []
created_at: 2024-11-15T00:25:15Z
updated_at: 2024-11-18T02:43:04Z
url: https://github.com/astral-sh/uv/issues/9137
synced_at: 2026-01-12T15:59:42Z
```

# `tool_run_cache` is flaky on Windows

---

_@charliermarsh_

See: https://github.com/astral-sh/uv/actions/runs/11847662623/job/33017797151?pr=9136

![Screenshot 2024-11-14 at 7 24 52 PM](https://github.com/user-attachments/assets/6bb8d655-a4d4-4eb4-b904-8a5b23c71a01)


---

_Label `testing` added by @charliermarsh on 2024-11-15 00:25_

---

_Comment by @zanieb on 2024-11-15 04:31_

This seems to be new, I hadn't seen it before.

---

_Comment by @konstin on 2024-11-15 09:51_

It's failing a lot: https://github.com/astral-sh/uv/actions/runs/11853487114/job/33034634717

---

_Comment by @charliermarsh on 2024-11-15 13:58_

(I saw it at least once this week prior to reporting here.)

---

_Comment by @zanieb on 2024-11-15 14:24_

Me too, but not prior to this week? Or maybe I saw it _once_ quite a while ago?

---

_Label `windows` added by @AlexWaygood on 2024-11-16 15:22_

---

_Comment by @charliermarsh on 2024-11-16 16:41_

I sort of have no idea what could be flaking here.

---

_Comment by @FishAlchemist on 2024-11-17 10:48_

Although the reason is unknown, the temporary environment used in the first and second ``uv tool run`` might be different. While this has little impact on usage due to the rapid environment setup, it can still lead to a certain probability of test failure.

My guess is that this PR(https://github.com/astral-sh/uv/pull/9106) included the problem, as it wasn't there before. However, I can't say for sure since this issue is not always reproducible.


This is the commit hash of this PR on the main branch.
```
git checkout a552f7430828e18e7ce98dfc5d96fb1592f9b1bc
```

Edit:
This script has the potential to cause the mentioned issue in Windows PowerShell.
It seems like setting ``UV_EXCLUDE_NEWER`` is mandatory; otherwise, the issue won't occur.
```powershell
git checkout a552f7430828e18e7ce98dfc5d96fb1592f9b1bc
$CACHE_DIR = "cache"
if((Test-Path -Path $CACHE_DIR))
{
	echo "Remove cache_dir"
	rm -Recurse -Force $CACHE_DIR
}
$env:UV_EXCLUDE_NEWER="2024-03-25T00:00:00Z"
$env:UV_SHOW_RESOLUTION="1"
cargo run -- tool run -p 3.12 --cache-dir $CACHE_DIR black --version
echo ""
echo ""
cargo run -- tool run -p 3.12 --cache-dir $CACHE_DIR black --version
echo ""
echo ""
cargo run -- tool run -p 3.12 --cache-dir $CACHE_DIR black --version
```

---

_Closed by @charliermarsh on 2024-11-18 02:43_

---

_Closed by @charliermarsh on 2024-11-18 02:43_

---
