```yaml
number: 11501
title: "Can't install packages from private PYPI source since v0.5.15"
type: issue
state: closed
author: Remalloc
labels:
  - bug
assignees: []
created_at: 2025-02-14T05:57:04Z
updated_at: 2025-02-19T13:12:55Z
url: https://github.com/astral-sh/uv/issues/11501
synced_at: 2026-01-10T03:50:31Z
```

# Can't install packages from private PYPI source since v0.5.15

---

_Issue opened by @Remalloc on 2025-02-14 05:57_

### Summary

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10,<3.13"
dependencies = [
    "datahub",
]

[[tool.uv.index]]
name = "aliyun_alpha"
url = "https://passwd:username@packages.aliyun.com/xxx/pypi/repo-eclfx"
publish-url = "https://passwd:username@packages.aliyun.com/xxx/pypi/repo-eclfx"

[tool.uv.sources]
datahub = { index = "aliyun_alpha" }
```

```bash
uv sync
error: Failed to fetch: `https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl`
  Caused by: HTTP status client error (400 Bad Request) for url (https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl)

curl -L https://passwd:username@packages.aliyun.com/xxx/pypi/repo-eclfx
{"object":"NOT_FOUND_PYPI_INDEX","successful":true}#

```

When using version 0.5.14 or earlier, there was no issue. This issue affects all platforms.



### Platform

Linux 6.5.0-44-generic x86_64 GNU/Linux

### Version

0.5.15 ~ 0.5.31

### Python version

Python 3.10 ~ 3.12

---

_Label `bug` added by @Remalloc on 2025-02-14 05:57_

---

_Comment by @zanieb on 2025-02-14 13:13_

I'm not sure what could have caused this.

Can you do a HEAD request with curl?

If you do `uvx uv@0.5.14 <command>` does it definitely succeed still?

Can you share `RUST_LOG=trace` for the failure?

---

_Comment by @Remalloc on 2025-02-14 14:15_


> Can you do a HEAD request with curl?
```
curl -I
HTTP/2 400 
date: Fri, 14 Feb 2025 14:09:58 GMT
content-length: 20
set-cookie: XSRF-TOKEN=7ae7c6fc-8a6f-4c48-b446-5b46597f3891; Path=/
set-cookie: grayTraffic=; Max-Age=0; Expires=Thu, 01 Jan 1970 00:00:10 GMT; Path=/
set-cookie: grayTraffic=; Max-Age=0; Expires=Thu, 01 Jan 1970 00:00:10 GMT
artlab-api-version: artlab/1.0
x-forwarded-for: 14.127.12.7
```

> If you do `uvx uv@0.5.14 <command>` does it definitely succeed still?

Yes, it works.

> Can you share `RUST_LOG=trace` for the failure?
```
RUST_LOG=trace uv sync
DEBUG uv 0.5.15
DEBUG Found project root: `/opt/alphamodel/py/alphamodel`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/opt/alphamodel/py/alphamodel/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `3.12`
TRACE Caching credentials for https://passwd:w6_username@packages.aliyun.com/xxx/pypi/repo-eclfx
DEBUG Using request timeout of 30s
⠙ Resolving dependencies...                                                                                                                                                                                                         DEBUG Found static `pyproject.toml` for: alphamodel @ file:///opt/alphamodel/py/alphamodel
DEBUG No workspace root found, using project root
warning: Missing version constraint (e.g., a lower bound) for `datahub`
⠋ Resolving dependencies...                                                                                                                                                                                                         TRACE Performing lookahead for alphamodel @ file:///opt/alphamodel/py/alphamodel
⠙ Resolving dependencies...                                                                                                                                                                                                         DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.11, <3.13
TRACE assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: alphamodel*
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies    
TRACE assigned packages: root==0a0.dev0
TRACE Chose package for decision: alphamodel. remaining choices: 
DEBUG Searching for a compatible version of alphamodel @ file:///opt/alphamodel/py/alphamodel (*)
⠙ alphamodel==0.1.0                                                                                                                                                                                                                 DEBUG Adding direct dependency: datahub*
INFO add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies    
TRACE assigned packages: root==0a0.dev0, alphamodel==0.1.0
TRACE Chose package for decision: datahub. remaining choices: 
TRACE Fetching metadata for datahub from https://passwd:w6_username@packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/
TRACE cached request https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ is storable because its response has a heuristically cacheable status code 200
TRACE could not determine freshness lifetime, assuming none exists
TRACE cached request https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ has a cached response that does not allow staleness
TRACE request https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ does not have a fresh cache because its age is 181 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/
DEBUG Sending revalidation request for: https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/
TRACE Handling request for https://passwd:****@packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/
TRACE Request for https://passwd:****@packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ is already fully authenticated
TRACE checkout waiting for idle connection: ("https", packages.aliyun.com)
DEBUG starting new connection: https://packages.aliyun.com/    
TRACE Http::connect; scheme=Some("https"), host=Some("packages.aliyun.com"), port=None
DEBUG connecting to 39.105.151.219:443
DEBUG connected to 39.105.151.219:443
TRACE ALPN negotiated h2, updating pool
DEBUG binding client connection
DEBUG client connection bound
DEBUG send frame=Settings { flags: (0x0), enable_push: 0, initial_window_size: 2097152, max_frame_size: 16384, max_header_list_size: 16384 }
TRACE encoding SETTINGS; len=24
TRACE encoding setting; val=EnablePush(0)
TRACE encoding setting; val=InitialWindowSize(2097152)
TRACE encoding setting; val=MaxFrameSize(16384)
TRACE encoding setting; val=MaxHeaderListSize(16384)
TRACE encoded settings rem=33
TRACE inc_window; sz=65535; old=0; new=65535
TRACE inc_window; sz=65535; old=0; new=65535
TRACE Prioritize::new; flow=FlowControl { window_size: Window(65535), available: Window(65535) }
TRACE set_target_connection_window; target=5242880; available=65535, reserved=0
TRACE http2 handshake complete, spawning background dispatcher task
TRACE put; add idle connection for ("https", packages.aliyun.com)
DEBUG pooling idle connection for ("https", packages.aliyun.com)
TRACE checkout dropped for ("https", packages.aliyun.com)
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=27
TRACE decoding frame from 27B
TRACE frame.kind=Settings
DEBUG received frame=Settings { flags: (0x0), max_concurrent_streams: 128, initial_window_size: 524288, max_frame_size: 16777215 }
TRACE recv SETTINGS frame=Settings { flags: (0x0), max_concurrent_streams: 128, initial_window_size: 524288, max_frame_size: 16777215 }
DEBUG send frame=Settings { flags: (0x1: ACK) }
TRACE encoding SETTINGS; len=0
TRACE encoded settings rem=42
TRACE ACK sent; applying settings
TRACE poll
TRACE read.bytes=13
TRACE decoding frame from 13B
TRACE frame.kind=WindowUpdate
DEBUG received frame=WindowUpdate { stream_id: StreamId(0), size_increment: 2147418112 }
TRACE recv WINDOW_UPDATE frame=WindowUpdate { stream_id: StreamId(0), size_increment: 2147418112 }
TRACE inc_window; sz=2147418112; old=65535; new=2147483647
TRACE poll
DEBUG send frame=WindowUpdate { stream_id: StreamId(0), size_increment: 5177345 }
TRACE encoding WINDOW_UPDATE; id=StreamId(0)
TRACE encoded window_update rem=55
TRACE inc_window; sz=5177345; old=65535; new=5242880
TRACE poll_complete
TRACE schedule_pending_open
TRACE queued_data_frame=false
TRACE flushing buffer
TRACE inc_window; sz=65535; old=0; new=65535
TRACE inc_window; sz=524288; old=0; new=524288
TRACE send_headers; frame=Headers { stream_id: StreamId(1), flags: (0x5: END_HEADERS | END_STREAM) }; init_window=524288
TRACE Queue::push_back
TRACE  -> first entry
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: HalfClosedLocal(AwaitingHeaders) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: Some(Indices { head: 0, tail: 0 }) }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: true, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(65535), available: Window(65535) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Omitted }
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(AwaitingHeaders) }; is_closed=false; pending_send_empty=false; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE schedule_pending_open; stream=StreamId(1)
TRACE Queue::push_front
TRACE  -> first entry
TRACE requested=0 additional=0 buffered=0 window=524288 conn=2147483647
TRACE is_pending_reset=false
TRACE pop_frame; frame=Headers { stream_id: StreamId(1), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(AwaitingHeaders) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE writing frame=Headers { stream_id: StreamId(1), flags: (0x5: END_HEADERS | END_STREAM) }
DEBUG send frame=Headers { stream_id: StreamId(1), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE schedule_pending_open
TRACE queued_data_frame=false
TRACE flushing buffer
TRACE idle interval checking for expired
⠹ alphamodel==0.1.0                                                                                                                                                                                                                 TRACE connection.state=Open
TRACE poll
TRACE read.bytes=9
TRACE decoding frame from 9B
TRACE frame.kind=Settings
DEBUG received frame=Settings { flags: (0x1: ACK) }
TRACE recv SETTINGS frame=Settings { flags: (0x1: ACK) }
DEBUG received settings ACK; applying Settings { flags: (0x0), enable_push: 0, initial_window_size: 2097152, max_frame_size: 16384, max_header_list_size: 16384 }
TRACE update_initial_window_size; new=2097152; old=65535
TRACE incrementing all windows; inc=2031617
TRACE inc_window; sz=2031617; old=65535; new=2097152
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=267
TRACE decoding frame from 267B
TRACE frame.kind=Headers
TRACE loading headers; flags=(0x4: END_HEADERS)
TRACE decode
TRACE rem=258 kind=Indexed
TRACE rem=257 kind=LiteralWithIndexing
TRACE rem=233 kind=LiteralWithIndexing
TRACE rem=224 kind=LiteralWithIndexing
TRACE rem=211 kind=LiteralWithoutIndexing
TRACE rem=159 kind=LiteralWithoutIndexing
TRACE rem=96 kind=LiteralWithoutIndexing
TRACE rem=39 kind=LiteralWithoutIndexing
TRACE rem=17 kind=LiteralWithoutIndexing
DEBUG received frame=Headers { stream_id: StreamId(1), flags: (0x4: END_HEADERS) }
TRACE recv HEADERS frame=Headers { stream_id: StreamId(1), flags: (0x4: END_HEADERS) }
TRACE recv_headers; stream=StreamId(1); state=State { inner: HalfClosedLocal(AwaitingHeaders) }
TRACE opening stream; init_window=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=211
TRACE decoding frame from 211B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=202; connection=5242880; stream=2097152
TRACE send_data; sz=202; window=5242880; available=5242880
TRACE send_data; sz=202; window=2097152; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=19
TRACE decoding frame from 19B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1), flags: (0x1: END_STREAM) }
TRACE recv DATA frame=Data { stream_id: StreamId(1), flags: (0x1: END_STREAM) }
TRACE recv_data; size=10; connection=5242678; stream=2096950
TRACE send_data; sz=10; window=5242678; available=5242678
TRACE recv_close: HalfClosedLocal => Closed
TRACE send_data; sz=10; window=2096950; available=2096950
TRACE transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(1)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2096940), available: Window(2096940) }, in_flight_recv_data: 212, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: Some(Indices { head: 1, tail: 2 }) }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Omitted }
TRACE transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Updating cached credentials for https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ to Credentials { username: Username(Some("passwd")), password: Some("w6_username") }
TRACE is modified because status is 200 and not 304
DEBUG Found modified response for: https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/
TRACE cached request https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ is storable because its response has a heuristically cacheable status code 200
TRACE release_capacity; size=202
TRACE release_connection_capacity; size=202, connection in_flight_data=212
TRACE release_capacity; size=10
TRACE release_connection_capacity; size=10, connection in_flight_data=10
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 1, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2096940), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: false, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Omitted }
TRACE transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE Received package metadata for: datahub
DEBUG Searching for a compatible version of datahub (*)
TRACE Selecting candidate for datahub with range * with 1 remote versions
TRACE Found candidate for package datahub with range * after 1 steps: 0.1.4 version
TRACE Returning candidate for package datahub with range * after 1 steps
DEBUG Selecting: datahub==0.1.4 [compatible] (datahub-0.1.4-py3-none-any.whl)
⠹ datahub==0.1.4                                                                                                                                                                                                                    TRACE No cache entry exists for /root/.cache/uv/wheels-v3/index/5fad140c42accf15/datahub/datahub-0.1.4-py3-none-any.msgpack
DEBUG No cache entry for: https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl
TRACE Sending fresh HEAD request for https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl
TRACE Handling request for https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl
TRACE Request for https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl is unauthenticated, checking cache
TRACE Found cached credentials for URL https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl
TRACE Request for https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl is fully authenticated
TRACE take? ("https", packages.aliyun.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", packages.aliyun.com)
TRACE inc_window; sz=2097152; old=0; new=2097152
TRACE inc_window; sz=524288; old=0; new=524288
TRACE send_headers; frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }; init_window=524288
TRACE Queue::push_back
TRACE  -> first entry
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: State { inner: HalfClosedLocal(AwaitingHeaders) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: Some(Indices { head: 0, tail: 0 }) }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: true, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=State { inner: HalfClosedLocal(AwaitingHeaders) }; is_closed=false; pending_send_empty=false; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE schedule_pending_open; stream=StreamId(3)
TRACE Queue::push_front
TRACE  -> first entry
TRACE requested=0 additional=0 buffered=0 window=524288 conn=2147483647
TRACE is_pending_reset=false
TRACE pop_frame; frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE transition_after; stream=StreamId(3); state=State { inner: HalfClosedLocal(AwaitingHeaders) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE writing frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
DEBUG send frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE schedule_pending_open
TRACE queued_data_frame=false
TRACE flushing buffer
⠸ datahub==0.1.4                                                                                                                                                                                                                    TRACE connection.state=Open
TRACE poll
TRACE read.bytes=254
TRACE decoding frame from 254B
TRACE frame.kind=Headers
TRACE loading headers; flags=(0x5: END_HEADERS | END_STREAM)
TRACE decode
TRACE rem=245 kind=Indexed
TRACE rem=244 kind=LiteralWithIndexing
TRACE rem=220 kind=LiteralWithIndexing
TRACE rem=216 kind=LiteralWithoutIndexing
TRACE rem=165 kind=LiteralWithoutIndexing
TRACE rem=102 kind=LiteralWithoutIndexing
TRACE rem=45 kind=LiteralWithoutIndexing
TRACE rem=22 kind=LiteralWithoutIndexing
DEBUG received frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv HEADERS frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv_headers; stream=StreamId(3); state=State { inner: HalfClosedLocal(AwaitingHeaders) }
TRACE opening stream; init_window=2097152
TRACE transition_after; stream=StreamId(3); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(3)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 1, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: false, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("packages.aliyun.com")), port: None, path: "/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(400), url: "https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl" }))) }
TRACE Cannot retry error: not an IO error
TRACE Streams::recv_eof
error: Failed to fetch: `https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl`
  Caused by: HTTP status client error (400 Bad Request) for url (https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl)
```

---

_Comment by @zanieb on 2025-02-14 15:23_

The only relevant change looks like https://github.com/astral-sh/uv/pull/10227 here's the full enumeration of changes https://github.com/astral-sh/uv/compare/0.5.14...0.5.15

---

_Comment by @zanieb on 2025-02-14 15:41_

@seanmonstar sorry to ping, but it _seems_ like this was a change from reqwest 0.12.9 -> 0.12.12 — perhaps something will stand out to you?



---

_Comment by @zanieb on 2025-02-14 15:49_

Sort of suspicious of https://github.com/seanmonstar/reqwest/pull/2503 (there aren't many changes to pick from).

@Remalloc could you test one of the binaries from https://github.com/astral-sh/uv/actions/runs/13332583535?pr=11512 (at the bottom) and see if it resolves your problem?

---

_Comment by @Remalloc on 2025-02-15 03:52_

> Sort of suspicious of [seanmonstar/reqwest#2503](https://github.com/seanmonstar/reqwest/pull/2503) (there aren't many changes to pick from).
> 
> [@Remalloc](https://github.com/Remalloc) could you test one of the binaries from https://github.com/astral-sh/uv/actions/runs/13332583535?pr=11512 (at the bottom) and see if it resolves your problem?

```
md5sum ./uv
e02ab926f11cfbedf7319c547b3f6a80  ./uv

./uv -V
uv 0.5.31 (4ad7cbacd 2025-02-14)

RUST_LOG=trace ./uv sync
DEBUG received frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv HEADERS frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv_headers; stream=StreamId(5); state=State { inner: HalfClosedLocal(AwaitingHeaders) }
TRACE opening stream; init_window=2097152
TRACE transition_after; stream=StreamId(5); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(5)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE drop_stream_ref; stream=Stream { id: StreamId(5), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Head }
TRACE transition_after; stream=StreamId(5); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE drop_stream_ref; stream=Stream { id: StreamId(5), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 1, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(524288), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: false, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Head }
TRACE transition_after; stream=StreamId(5); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("packages.aliyun.com")), port: None, path: "/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(400), url: "https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl" }))) }
TRACE Cannot retry error: not an IO error
TRACE Streams::recv_eof
error: Failed to fetch: `https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl`
  Caused by: HTTP status client error (400 Bad Request) for url (https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.4-py3-none-any.whl)
```

Still have problem.

---

_Comment by @zanieb on 2025-02-15 04:09_

Interesting, thanks for checking — that would rule out the reqwest upgrade.

My next best guess was https://github.com/astral-sh/uv/pull/10310 though it seems dubious cc @BurntSushi 

This would be via 

https://github.com/astral-sh/uv/blob/5c0fdfd7ce15726e5b2c25309873329084dbc3a0/crates/uv-client/src/httpcache/mod.rs#L521-L539

https://github.com/astral-sh/uv/blob/5c0fdfd7ce15726e5b2c25309873329084dbc3a0/crates/uv-client/src/httpcache/mod.rs#L1377-L1393

which I think are ruled out by this being a "fresh" request (`Sending fresh HEAD request`) so we're in this code path

https://github.com/astral-sh/uv/blob/a1a4d820d54607613bceb4973789b67c8c1c1232/crates/uv-client/src/cached_client.rs#L276-L281

instead of 

https://github.com/astral-sh/uv/blob/a1a4d820d54607613bceb4973789b67c8c1c1232/crates/uv-client/src/cached_client.rs#L272-L274

which would invoke the referenced jiff code.

Perhaps there's something I'm missing related to the range request path? I didn't dig into that yet.

Unfortunately, I'm at a bit of a loss. We'd need to add more verbose request logs to understand what we're sending.

What kind of PyPI server are you running? Can you get server-side logs about why the request was invalid or about the content of the requests?

---

_Comment by @seanmonstar on 2025-02-15 10:49_

Just poking in here, sometimes the response body will include more details about why the request was bad. I didn't see it in any of the traces.

---

_Comment by @Remalloc on 2025-02-15 16:24_

I can't check service log because cloud service not provide, but I used Fiddler to capture all requests from uv and found a difference:


The latest version(two requests): 
```
GET https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/ HTTP/1.1
Authorization: Basic xxx
Accept-Encoding: gzip
Accept: application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01
User-Agent: uv/0.6.0 {"installer":{"name":"uv","version":"0.6.0"},"python":"3.12.7","implementation":{"name":"CPython","version":"3.12.7"},"distro":null,"system":{"name":"Windows","release":"11"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":null}
Host: packages.aliyun.com

Response:
<!DOCTYPE html>
 <html>
        <head>
            <meta name="pypi:repository-version" content="1.0">
<title>Links for datahub/</title>
        </head>
        <body>
            <h1>Links for datahub/</h1><a href="datahub-0.1.4-py3-none-any.whl" rel="internal">datahub-0.1.4-py3-none-any.whl</a><br/>
<a href="datahub-0.1.5-py3-none-any.whl" rel="internal">datahub-0.1.5-py3-none-any.whl</a><br/>
        </body>
</html>
```

```
HEAD https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.5-py3-none-any.whl HTTP/1.1
Accept-Encoding: identity
Authorization: Basic xxx==
Accept: */*
User-Agent: uv/0.6.0 {"installer":{"name":"uv","version":"0.6.0"},"python":"3.12.7","implementation":{"name":"CPython","version":"3.12.7"},"distro":null,"system":{"name":"Windows","release":"11"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":null}
Host: packages.aliyun.com

Response:
HTTP/1.1 400
```

uv 0.15.4(only one request)
```
GET https://packages.aliyun.com/67ac370259270b500346f530/pypi/repo-eclfx/datahub/ HTTP/1.1
Authorization: Basic NjdhYzNhMmIwMjI2MGEzYmM0ZmNmYWZhOnc2X2VlSDVEN1pSUQ==
Accept-Encoding: gzip
Accept: application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01
User-Agent: uv/0.5.14 {"installer":{"name":"uv","version":"0.5.14"},"python":"3.12.7","implementation":{"name":"CPython","version":"3.12.7"},"distro":null,"system":{"name":"Windows","release":"11"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":null}
Host: packages.aliyun.com

Response:
<!DOCTYPE html>
 <html>
        <head>
            <meta name="pypi:repository-version" content="1.0">
<title>Links for datahub/</title>
        </head>
        <body>
            <h1>Links for datahub/</h1><a href="datahub-0.1.4-py3-none-any.whl" rel="internal">datahub-0.1.4-py3-none-any.whl</a><br/>
<a href="datahub-0.1.5-py3-none-any.whl" rel="internal">datahub-0.1.5-py3-none-any.whl</a><br/>
        </body>
</html>
```

So, the problem seems to be caused by the second HEAD request.
@zanieb @seanmonstar 


---

_Comment by @zanieb on 2025-02-15 16:30_

Thanks for giving it a look Sean. I think we'll want to add logging of response bodies on failure — I'm not sure if we do that. cc @konstin 

Ah that's really helpful. In the previous version of uv we aren't downloading the wheel? That seems surprising.

Does this reproduce if you use `--no-cache --reinstall-package datahub` flag on 0.5.14?

---

_Comment by @Remalloc on 2025-02-15 16:51_

There is no problem in version 0.5.14. (--no-cache --reinstall-package datahub)

```
GET https://packages.aliyun.com/xxx/pypi/repo-eclfx/datahub/datahub-0.1.5-py3-none-any.whl HTTP/1.1
Accept-Encoding: identity
Authorization: Basic xxx
Accept: */*
User-Agent: uv/0.5.14 {"installer":{"name":"uv","version":"0.5.14"},"python":"3.12.7","implementation":{"name":"CPython","version":"3.12.7"},"distro":null,"system":{"name":"Windows","release":"11"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":null}
Host: packages.aliyun.com

Response:
HTTP/1.1 307

GET http://yunxiao-devops-packages-storage.oss-accelerate.aliyuncs.com/db/xxx?Expires=1739641452&OSSAccessKeyId=xxx&Signature=xxx%3D&response-content-disposition=attachment%3B%20filename%3D%22datahub%22 HTTP/1.1
Accept-Encoding: identity
User-Agent: uv/0.5.14 {"installer":{"name":"uv","version":"0.5.14"},"python":"3.12.7","implementation":{"name":"CPython","version":"3.12.7"},"distro":null,"system":{"name":"Windows","release":"11"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":null}
Accept: */*
Host: yunxiao-devops-packages-storage.oss-accelerate.aliyuncs.com

Response:
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Sat, 15 Feb 2025 16:44:12 GMT
Content-Type: application/octet-stream
Content-Length: 23360
Connection: keep-alive
x-oss-request-id: 67B0C45C0B9F106B550C1971
Accept-Ranges: bytes
ETag: "588657F5647570566A042CA79E8D32D6"
Last-Modified: Sat, 15 Feb 2025 04:45:29 GMT
x-oss-object-type: Normal
x-oss-hash-crc64ecma: 9377635836412253760
x-oss-storage-class: Standard
Content-Disposition: attachment; filename="datahub"
x-oss-version-id: xxx--
Content-MD5: WIZX9WR1cFZqBCynno0y1g==
x-oss-server-time: 38

```

---

_Comment by @zanieb on 2025-02-15 17:05_

Interesting. That suggests the change is that in 0.5.15 we are using `HEAD` but in 0.5.14 we know the index does not support range requests for some reason and are using `GET` per

https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/registry_client.rs#L710

Is this log present in 0.5.14?

https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/registry_client.rs#L772

Here's another pull request we can try https://github.com/astral-sh/uv/pull/11539 — here we should fallback to `GET` when we encounter a HTTP 400. The binaries will be [here](https://github.com/astral-sh/uv/actions/runs/13346996919?pr=11539).

Can you confirm what kind of PyPI server you're using? Is it provided by Alibaba Clloud?

---

_Comment by @Remalloc on 2025-02-16 02:47_

> Interesting. That suggests the change is that in 0.5.15 we are using `HEAD` but in 0.5.14 we know the index does not support range requests for some reason and are using `GET` per
> 
> [uv/crates/uv-client/src/registry_client.rs](https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/registry_client.rs#L710)
> 
> Line 710 in [748582e](/astral-sh/uv/commit/748582ee6f3ff1007692f131fa4b59d7c100ec66)
> 
>  if index.map_or(true, |index| capabilities.supports_range_requests(index)) { 
> Is this log present in 0.5.14?
> 
> [uv/crates/uv-client/src/registry_client.rs](https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/registry_client.rs#L772)
> 
> Line 772 in [748582e](/astral-sh/uv/commit/748582ee6f3ff1007692f131fa4b59d7c100ec66)
> 
>  warn!("Range requests not supported for {filename}; streaming wheel"); 
> Here's another pull request we can try [#11539](https://github.com/astral-sh/uv/pull/11539) — here we should fallback to `GET` when we encounter a HTTP 400. The binaries will be [here](https://github.com/astral-sh/uv/actions/runs/13346996919?pr=11539).
> 
> Can you confirm what kind of PyPI server you're using? Is it provided by Alibaba Clloud?

Yes, it is provided by Alibaba Cloud, but I don't know what type of PyPI server it is. #11539  new binary works now 😀

---

_Comment by @charliermarsh on 2025-02-16 03:14_

Nice, thanks for trying it.

---

_Comment by @Remalloc on 2025-02-16 03:21_

Thank you to all the devs for your efforts. I have also reported this issue to Alibaba Cloud, hoping they will support HEAD requests in the future.


---

_Closed by @Remalloc on 2025-02-16 03:21_

---

_Comment by @zanieb on 2025-02-16 04:02_

Thanks for your help!

Still absolutely no clue how this _regressed_ for you at some point. We must have changed the order of requests or something.

---

_Comment by @Remalloc on 2025-02-19 13:12_

Hello, I cloned this repository and found that the commit https://github.com/astral-sh/uv/commit/7182a34aa414f788d1a76a15690f2cd14e398e77 caused the issue. Do you have any ideas? 
@zanieb @charliermarsh 


---
