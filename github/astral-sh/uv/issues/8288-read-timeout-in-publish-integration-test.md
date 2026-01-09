---
number: 8288
title: Read timeout in publish integration test
type: issue
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-10-17T14:20:09Z
updated_at: 2024-11-03T18:05:41Z
url: https://github.com/astral-sh/uv/issues/8288
synced_at: 2026-01-07T13:12:17-06:00
---

# Read timeout in publish integration test

---

_Issue opened by @zanieb on 2024-10-17 14:20_

e.g. https://github.com/astral-sh/uv/actions/runs/11386806549/job/31680084192?pr=8269

```
DEBUG Writing Python versions to `/home/runner/work/uv/uv/scripts/publish/astral-test-token/.python-version`
DEBUG Detected existing version control system: git
Initialized project `astral-test-token` at `/home/runner/work/uv/uv/scripts/publish/astral-test-token`
Traceback (most recent call last):
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_transports/default.py", line 72, in map_httpcore_exceptions

    yield
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_transports/default.py", line 236, in handle_request
    resp = self._pool.handle_request(req)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/connection_pool.py", line 216, in handle_request
    raise exc from None
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/connection_pool.py", line 196, in handle_request
    response = connection.handle_request(
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/connection.py", line 101, in handle_request
    return self._connection.handle_request(request)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/http11.py", line 143, in handle_request
    raise exc
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/http11.py", line [113](https://github.com/astral-sh/uv/actions/runs/11386806549/job/31680084192?pr=8269#step:7:114), in handle_request
    ) = self._receive_response_headers(**kwargs)
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/http11.py", line 186, in _receive_response_headers
    event = self._receive_event(timeout=timeout)
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_sync/http11.py", line 224, in _receive_event
Publish astral-test-token for pypi-token
    data = self._network_stream.read(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_backends/sync.py", line 124, in read
    with map_exceptions(exc_map):
         ^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/contextlib.py", line 158, in __exit__
    self.gen.throw(value)
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpcore/_exceptions.py", line 14, in map_exceptions
    raise to_exc(exc) from exc
httpcore.ReadTimeout: The read operation timed out

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/home/runner/work/uv/uv/scripts/publish/test_publish.py", line 269, in <module>
    main()
  File "/home/runner/work/uv/uv/scripts/publish/test_publish.py", line 265, in main
    publish_project(project_name, uv)
  File "/home/runner/work/uv/uv/scripts/publish/test_publish.py", line 135, in publish_project
    create_project(project_name, uv)
  File "/home/runner/work/uv/uv/scripts/publish/test_publish.py", line 124, in create_project
    new_version = get_new_version(project_name)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/work/uv/uv/scripts/publish/test_publish.py", line 100, in get_new_version
    data = httpx.get(url).text
           ^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_api.py", line 210, in get
    return request(
           ^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_api.py", line [118](https://github.com/astral-sh/uv/actions/runs/11386806549/job/31680084192?pr=8269#step:7:119), in request
    return client.request(
           ^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_client.py", line 837, in request
    return self.send(request, auth=auth, follow_redirects=follow_redirects)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_client.py", line 926, in send
    response = self._send_handling_auth(
               ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_client.py", line 954, in _send_handling_auth
    response = self._send_handling_redirects(
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_client.py", line 991, in _send_handling_redirects
    response = self._send_single_request(request)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_client.py", line 1027, in _send_single_request
    response = transport.handle_request(request)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_transports/default.py", line 235, in handle_request
    with map_httpcore_exceptions():
         ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/hostedtoolcache/Python/3.12.7/x64/lib/python3.12/contextlib.py", line 158, in __exit__
    self.gen.throw(value)
  File "/home/runner/.cache/uv/archive-v0/vtfy5YDR3oRBRdbiHNWH_/lib/python3.12/site-packages/httpx/_transports/default.py", line 89, in map_httpcore_exceptions
    raise mapped_exc(message) from exc
httpx.ReadTimeout: The read operation timed out
DEBUG Command exited with code: 1
```

---

_Label `internal` added by @zanieb on 2024-10-17 14:20_

---

_Label `testing` added by @zanieb on 2024-10-17 14:20_

---

_Referenced in [astral-sh/uv#8289](../../astral-sh/uv/pulls/8289.md) on 2024-10-17 14:25_

---

_Comment by @konstin on 2024-11-03 18:05_

The script now has retries

---

_Closed by @konstin on 2024-11-03 18:05_

---
