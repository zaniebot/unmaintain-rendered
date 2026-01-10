---
number: 17047
title: uv - incompatibility with repoflow
type: issue
state: open
author: Poil
labels:
  - bug
  - external
assignees: []
created_at: 2025-12-09T14:07:00Z
updated_at: 2025-12-11T10:55:09Z
url: https://github.com/astral-sh/uv/issues/17047
synced_at: 2026-01-10T01:26:13Z
---

# uv - incompatibility with repoflow

---

_Issue opened by @Poil on 2025-12-09 14:07_

### Summary

Hi,

It looks like uv is not working with repoflow (acting as proxy cache for pypi.org)
Everything works fine with pip

```
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: Closed(EndStream), is_counted: false, ref_count: 1, send_flow: FlowControl { window_size: Window(65535), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(1933098), available: Window(2097152) }, in_flight_recv_data: 0, content_length: Remaining(0) }
TRACE transition_after; stream=StreamId(1); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE Received package metadata for: torch
TRACE Selecting candidate for torch with range * with 44 remote versions
DEBUG Searching for a compatible version of torch (*)
TRACE Found candidate for package torch with range * after 1 steps: 2.9.1 version
TRACE Selecting candidate for torch with range * with 44 remote versions
TRACE Returning candidate for package torch with range * after 1 steps
TRACE Found candidate for package torch with range * after 1 steps: 2.9.1 version
TRACE Returning candidate for package torch with range * after 1 steps
DEBUG Selecting: torch==2.9.1 [compatible] (torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl)
TRACE No cache entry exists for /home/bdupuis/.cache/uv/wheels-v5/index/90a994867a8cd17e/torch/2.9.1-cp314-cp314-manylinux_2_28_x86_64.msgpack
DEBUG No cache entry for: https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE Sending fresh HEAD request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE Handling request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl with authentication policy auto
TRACE Request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE Attempting unauthenticated request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE take? ("https", repoflow.tools.dentalmind.ai): expiration = Some(90s)
DEBUG reuse idle connection for ("https", repoflow.tools.dentalmind.ai)
TRACE inc_window; sz=2097152; old=0; new=2097152
TRACE inc_window; sz=65536; old=0; new=65536
TRACE send_headers; frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }; init_window=65536
TRACE Queue::push_back
TRACE  -> first entry
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: HalfClosedLocal(AwaitingHeaders), is_counted: false, ref_count: 2, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, pending_send: Deque { indices: Some(Indices { head: 0, tail: 0 }) }, is_pending_open: true, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, is_recv: true, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=HalfClosedLocal(AwaitingHeaders); is_closed=false; pending_send_empty=false; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE schedule_pending_open; stream=StreamId(3)
TRACE Queue::push_front
TRACE  -> first entry
TRACE requested=0 additional=0 buffered=0 window=65536 conn=2147483647
TRACE is_pending_reset=false
TRACE pop_frame; frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE transition_after; stream=StreamId(3); state=HalfClosedLocal(AwaitingHeaders); is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE writing frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
DEBUG send frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE schedule_pending_open
TRACE queued_data_frame=false
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=267
TRACE decoding frame from 267B
TRACE frame.kind=Headers
TRACE loading headers; flags=(0x5: END_HEADERS | END_STREAM)
TRACE decode
TRACE rem=258 kind=Indexed
TRACE rem=257 kind=LiteralWithIndexing
TRACE rem=233 kind=LiteralWithIndexing
TRACE rem=224 kind=LiteralWithIndexing
TRACE rem=218 kind=LiteralWithoutIndexing
TRACE rem=202 kind=LiteralWithoutIndexing
TRACE rem=168 kind=LiteralWithoutIndexing
TRACE rem=151 kind=LiteralWithoutIndexing
TRACE rem=133 kind=LiteralWithoutIndexing
TRACE rem=96 kind=LiteralWithoutIndexing
TRACE rem=80 kind=LiteralWithoutIndexing
TRACE rem=62 kind=LiteralWithoutIndexing
TRACE rem=25 kind=LiteralWithoutIndexing
DEBUG received frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv HEADERS frame=Headers { stream_id: StreamId(3), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE recv_headers; stream=StreamId(3); state=HalfClosedLocal(AwaitingHeaders)
TRACE opening stream; init_window=2097152
TRACE transition_after; stream=StreamId(3); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(3)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: Closed(EndStream), is_counted: false, ref_count: 2, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, is_recv: true, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Response from https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl is storable because it has a heuristically cacheable status code 200
TRACE drop_stream_ref; stream=Stream { id: StreamId(3), state: Closed(EndStream), is_counted: false, ref_count: 1, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, content_length: Head }
TRACE transition_after; stream=StreamId(3); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Getting metadata for torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl by range request
TRACE Handling request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl with authentication policy auto
TRACE Request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE Attempting unauthenticated request for https://repoflow.tools.dentalmind.ai/pypi/dentalmonitoring/pypi/torch/torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
TRACE take? ("https", repoflow.tools.dentalmind.ai): expiration = Some(90s)
DEBUG reuse idle connection for ("https", repoflow.tools.dentalmind.ai)
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE inc_window; sz=2097152; old=0; new=2097152
TRACE inc_window; sz=65536; old=0; new=65536
TRACE send_headers; frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }; init_window=65536
TRACE Queue::push_back
TRACE  -> first entry
TRACE drop_stream_ref; stream=Stream { id: StreamId(5), state: HalfClosedLocal(AwaitingHeaders), is_counted: false, ref_count: 2, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, pending_send: Deque { indices: Some(Indices { head: 0, tail: 0 }) }, is_pending_open: true, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, is_recv: true, content_length: Omitted }
TRACE transition_after; stream=StreamId(5); state=HalfClosedLocal(AwaitingHeaders); is_closed=false; pending_send_empty=false; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE schedule_pending_open; stream=StreamId(5)
TRACE Queue::push_front
TRACE  -> first entry
TRACE requested=0 additional=0 buffered=0 window=65536 conn=2147483647
TRACE is_pending_reset=false
TRACE pop_frame; frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE transition_after; stream=StreamId(5); state=HalfClosedLocal(AwaitingHeaders); is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE writing frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }
DEBUG send frame=Headers { stream_id: StreamId(5), flags: (0x5: END_HEADERS | END_STREAM) }
TRACE schedule_pending_open
TRACE queued_data_frame=false
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=276
TRACE decoding frame from 276B
TRACE frame.kind=Headers
TRACE loading headers; flags=(0x4: END_HEADERS)
TRACE decode
TRACE rem=267 kind=Indexed
TRACE rem=266 kind=LiteralWithIndexing
TRACE rem=242 kind=LiteralWithIndexing
TRACE rem=233 kind=LiteralWithIndexing
TRACE rem=227 kind=LiteralWithoutIndexing
TRACE rem=211 kind=LiteralWithoutIndexing
TRACE rem=177 kind=LiteralWithoutIndexing
TRACE rem=160 kind=LiteralWithoutIndexing
TRACE rem=142 kind=LiteralWithoutIndexing
TRACE rem=105 kind=LiteralWithoutIndexing
TRACE rem=80 kind=LiteralWithoutIndexing
TRACE rem=62 kind=LiteralWithoutIndexing
TRACE rem=25 kind=LiteralWithoutIndexing
DEBUG received frame=Headers { stream_id: StreamId(5), flags: (0x4: END_HEADERS) }
TRACE recv HEADERS frame=Headers { stream_id: StreamId(5), flags: (0x4: END_HEADERS) }
TRACE recv_headers; stream=StreamId(5); state=HalfClosedLocal(AwaitingHeaders)
TRACE opening stream; init_window=2097152
TRACE transition_after; stream=StreamId(5); state=HalfClosedLocal(Streaming); is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=4077
TRACE decoding frame from 4077B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(5) }
TRACE recv DATA frame=Data { stream_id: StreamId(5) }
TRACE recv_data; size=4068; connection=5078826; stream=2097152
TRACE send_data; sz=4068; window=5078826; available=5242880
TRACE send_data; sz=4068; window=2097152; available=2097152
TRACE transition_after; stream=StreamId(5); state=HalfClosedLocal(Streaming); is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=9
TRACE decoding frame from 9B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(5), flags: (0x1: END_STREAM) }
TRACE recv DATA frame=Data { stream_id: StreamId(5), flags: (0x1: END_STREAM) }
TRACE recv_data; size=0; connection=5074758; stream=2093084
TRACE send_data; sz=0; window=5074758; available=5238812
TRACE recv_close: HalfClosedLocal => Closed
TRACE send_data; sz=0; window=2093084; available=2093084
TRACE transition_after; stream=StreamId(5); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(5)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer

TRACE drop_stream_ref; stream=Stream { id: StreamId(5), state: Closed(EndStream), is_counted: false, ref_count: 2, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(2093084), available: Window(2093084) }, in_flight_recv_data: 4068, pending_recv: Deque { indices: Some(Indices { head: 0, tail: 3 }) }, is_recv: true, content_length: Remaining(0) }
TRACE transition_after; stream=StreamId(5); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE release_capacity; size=4068
TRACE release_connection_capacity; size=4068, connection in_flight_data=4068
TRACE release_capacity; size=0
TRACE release_connection_capacity; size=0, connection in_flight_data=0
TRACE drop_stream_ref; stream=Stream { id: StreamId(5), state: Closed(EndStream), is_counted: false, ref_count: 1, send_flow: FlowControl { window_size: Window(65536), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(2093084), available: Window(2097152) }, in_flight_recv_data: 0, content_length: Remaining(0) }
TRACE transition_after; stream=StreamId(5); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE Considering retry of error: Error { kind: Zip(WheelFilename { name: PackageName("torch"), version: "2.9.1", tags: Small { small: WheelTagSmall { python_tag: CPython { python_version: (3, 14) }, abi_tag: CPython { gil_disabled: false, python_version: (3, 14) }, platform_tag: Manylinux { major: 2, minor: 28, arch: X86_64 } } } }, UnableToLocateEOCDR), retries: 0 }
TRACE Cannot retry error: Neither an IO error nor a reqwest error
DEBUG Released lock at `/home/xxx/blabla/.venv/.lock`
DEBUG Released lock at `/home/xxx/.cache/uv/.lock`
TRACE Streams::recv_eof
TRACE Error trace: Failed to unzip wheel: torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl

Caused by:
    unable to locate the end of central directory record
error: Failed to unzip wheel: torch-2.9.1-cp314-cp314-manylinux_2_28_x86_64.whl
  Caused by: unable to locate the end of central directory record
```

Regards

### Platform

Ubuntu 24.04

### Version

uv 0.9.16

### Python version

Python 3.14.2

---

_Label `bug` added by @Poil on 2025-12-09 14:07_

---

_Label `external` added by @konstin on 2025-12-11 10:48_

---

_Comment by @konstin on 2025-12-11 10:55_

This looks like a bug in repoflow, are they forwarding PyPI's `accept-ranges: bytes` header without implementing support for range requests? It's strange that we need to use range requests at all, is repoflow stripping PyPI's PEP 658 support when proxying?

---
