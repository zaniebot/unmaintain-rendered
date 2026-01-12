```yaml
number: 1536
title: "can't resolve types-pytz==2021.1"
type: issue
state: closed
author: raylu
labels:
  - bug
assignees: []
created_at: 2024-02-16T19:51:31Z
updated_at: 2024-02-16T21:30:55Z
url: https://github.com/astral-sh/uv/issues/1536
synced_at: 2026-01-12T15:58:29Z
```

# can't resolve types-pytz==2021.1

---

_@raylu_

with this requirements.in
```
types-pytz
```
and this requirements.txt
```
types-pytz==2021.1
```
`pip-compile requirements.in` updates it to
```
types-pytz==2021.1.0
```
but uv just hangs at
```
$ RUST_LOG=uv=trace uv pip compile requirements.in -o requirements.txt -v
        0.072319s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of types-pytz (*)
        0.072324s   0ms DEBUG uv_resolver::resolver Selecting: types-pytz==2021.1 (types_pytz-2021.1.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=types-pytz, version=2021.1
     uv_resolver::resolver::distributions_wait package_id=types-pytz-2021.1
              0.072394s   0ms TRACE uv_client::httpcache cached request https://files.pythonhosted.org/packages/ea/cd/143c3cd227ff542c8a0e13f3ad4cda43b8c605774347815208dc38271b5a/types_pytz-2021.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
              0.072422s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/ea/cd/143c3cd227ff542c8a0e13f3ad4cda43b8c605774347815208dc38271b5a/types_pytz-2021.1.0-py3-none-any.whl
    0.072447s TRACE uv_resolver::resolver Received built distribution metadata for: types-pytz==2021.1.0
```

FWIW, `uv pip install` works
```
$ uv pip install types-pytz==2021.1  
Resolved 1 package in 1ms
Installed 1 package in 1ms
 + types-pytz==2021.1.0
```

---

_Comment by @charliermarsh on 2024-02-16 20:22_

Wow so weird! Thank you.

---

_Label `bug` added by @charliermarsh on 2024-02-16 20:22_

---

_Comment by @charliermarsh on 2024-02-16 20:22_

I'll look at this now.

---

_Comment by @charliermarsh on 2024-02-16 20:22_

It seems to work without the `-o requirements.txt` which is odd.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 20:22_

---

_Comment by @charliermarsh on 2024-02-16 20:32_

Ah, ok. It's because of the mismatch between `==2021.1` and `==2021.1.0`. We should treat them equivalently, but we don't in certain places. If you update your requirements.txt to `==2021.1.0`, it'll work as expected.

(But we'll fix this.)

---

_Comment by @raylu on 2024-02-16 20:36_

> If you update your requirements.txt to ==2021.1.0, it'll work as expected.

oh I probably should've mentioned that this is what I ended up doing to work around it

---

_Comment by @charliermarsh on 2024-02-16 21:09_

Fixed in https://github.com/astral-sh/uv/pull/1543.

---

_Closed by @charliermarsh on 2024-02-16 21:30_

---
