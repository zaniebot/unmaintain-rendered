```yaml
number: 13732
title: update python build standalone, unblocklist linux updates
type: pull_request
state: merged
author: Gankra
labels:
  - uv python
assignees: []
merged: true
base: main
head: gankra/blockless
created_at: 2025-05-30T15:28:06Z
updated_at: 2025-05-30T16:34:13Z
url: https://github.com/astral-sh/uv/pull/13732
synced_at: 2026-01-10T11:10:42Z
```

# update python build standalone, unblocklist linux updates

---

_Pull request opened by @Gankra on 2025-05-30 15:28_

_No description provided._

---

_Label `internal` added by @Gankra on 2025-05-30 15:28_

---

_Renamed from "close the bad linux range" to "update python build standalone, unblocklist linux updates" by @Gankra on 2025-05-30 15:42_

---

_@geofft approved on 2025-05-30 15:45_

---

_Comment by @geofft on 2025-05-30 15:56_

LGTM, we now have a working interpreter that still statically links libpython:

```
ubuntu@ip-172-16-0-59:~/add$ uv run --managed-python python -c 'import repro; repro.f()'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import repro; repro.f()
                  ~~~~~~~^^
  File "/home/ubuntu/add/repro.py", line 4, in f
    a += Foo("hi")
TypeError: must be str, not list
ubuntu@ip-172-16-0-59:~/add$ curl -LOJ https://github.com/astral-sh/uv/raw/862a4e1d3854b5f27e9b2a2436102e41843c286d/crates/uv-python/download-metadata.json
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100  907k  100  907k    0     0  6860k      0 --:--:-- --:--:-- --:--:-- 6860k
ubuntu@ip-172-16-0-59:~/add$ UV_PYTHON_DOWNLOADS_JSON_URL=~/download-metadata.json uv python install --reinstall 3.13
Installed Python 3.13.3 in 2.37s
 ~ cpython-3.13.3-linux-x86_64-gnu
ubuntu@ip-172-16-0-59:~/add$ uv run --managed-python python -c 'import repro; repro.f()'
ubuntu@ip-172-16-0-59:~/add$ ldd `uv python find --managed-python`
	linux-vdso.so.1 (0x00007ffd0879c000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x000073dd188bf000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x000073dd188ba000)
	libutil.so.1 => /lib/x86_64-linux-gnu/libutil.so.1 (0x000073dd188b5000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x000073dd187cc000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x000073dd187c7000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x000073dd18400000)
	/lib64/ld-linux-x86-64.so.2 (0x000073dd188cf000)
```

---

_Assigned to @zanieb by @zanieb on 2025-05-30 16:04_

---

_@zanieb reviewed on 2025-05-30 16:04_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:70 on 2025-05-30 16:04_

Can we not just drop this entirely? Or do we need it hide some releases?

---

_@Gankra reviewed on 2025-05-30 16:05_

---

_Review comment by @Gankra on `crates/uv-python/fetch-download-metadata.py`:70 on 2025-05-30 16:05_

Hmm good point, after getting a clarification from geoffery I *think* we can drop it entirely now. Let me check if anything changes.

---

_@Gankra reviewed on 2025-05-30 16:08_

---

_Review comment by @Gankra on `crates/uv-python/fetch-download-metadata.py`:70 on 2025-05-30 16:08_

You were correct, pushed.

---

_Merged by @Gankra on 2025-05-30 16:33_

---

_Closed by @Gankra on 2025-05-30 16:33_

---

_Branch deleted on 2025-05-30 16:33_

---

_Label `internal` removed by @Gankra on 2025-05-30 16:33_

---

_Label `uv python` added by @Gankra on 2025-05-30 16:34_

---
