---
number: 1402
title: "Confusing error if uv thinks `requires-python` is invalid"
type: issue
state: closed
author: avx-rvandegrift
labels:
  - bug
assignees: []
created_at: 2024-02-16T00:28:08Z
updated_at: 2024-02-16T02:12:11Z
url: https://github.com/astral-sh/uv/issues/1402
synced_at: 2026-01-07T13:12:16-06:00
---

# Confusing error if uv thinks `requires-python` is invalid

---

_Issue opened by @avx-rvandegrift on 2024-02-16 00:28_

Here's another oddity:
```
$ uv pip install junos-eznc==2.6.5
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of junos-eznc==2.6.5 and you require junos-eznc==2.6.5, we can conclude that the requirements are unsatisfiable.
```
And yet: https://pypi.org/project/junos-eznc/2.6.5/#files

With `--verbose`, the problem turns out to be somewhat different:
```
$ uv pip install --verbose junos-eznc==2.6.5
...
 uv_resolver::resolver::process_request request=Prefetch junos-eznc ==2.6.5
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/junos-eznc"
       uv_client::cached_client::new_cache file=/home/rvandegrift/.cache/uv/simple-v0/pypi/junos-eznc.rkyv
       uv_client::registry_client::parse_simple_api package=junos-eznc
            0.082628s   3ms  WARN uv_client::registry_client Skipping file for junos-eznc: Invalid 'requires-python' value
            0.082663s   3ms  WARN uv_client::registry_client Skipping file for junos-eznc: Invalid 'requires-python' value
            ... # this repeats a dozen or two times
 uv_resolver::version_map::from_metadata
        0.084030s  61ms DEBUG uv_resolver::resolver Searching for a compatible version of junos-eznc (==2.6.5)
      0.084056s  61ms DEBUG uv_resolver::resolver No compatible version found for: junos-eznc
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of junos-eznc==2.6.5 and you require junos-eznc==2.6.5, we can conclude that the requirements are unsatisfiable.

```

Here's the info from that package:
```
$ lesspipe ~/junos_eznc-2.6.5-py2.py3-none-any.whl | grep -i requires-python
Requires-Python: >=3.*, !=3.0.*, !=3.1.*, !=3.2.*, !=3.3.*
```

---

_Label `bug` added by @zanieb on 2024-02-16 00:28_

---

_Comment by @zanieb on 2024-02-16 00:29_

Thanks for the report! Similar at https://github.com/astral-sh/uv/issues/1361 and https://github.com/astral-sh/uv/issues/1357 

---

_Comment by @zanieb on 2024-02-16 00:35_

Two things here:

1. Our version parser should allow this
2. We should try to propagate invalid distributions into the solver, seems kind of hard cc @BurntSushi 

---

_Comment by @charliermarsh on 2024-02-16 01:33_

I'll fix the `requires-python` part (1).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 01:33_

---

_Referenced in [astral-sh/uv#1410](../../astral-sh/uv/pulls/1410.md) on 2024-02-16 02:04_

---

_Closed by @charliermarsh on 2024-02-16 02:12_

---

_Referenced in [astral-sh/uv#1412](../../astral-sh/uv/issues/1412.md) on 2024-02-16 02:14_

---
