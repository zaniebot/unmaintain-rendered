---
number: 12730
title: "`uv add` fails with multiple errors"
type: issue
state: closed
author: sreekarchigurupati
labels:
  - bug
assignees: []
created_at: 2025-04-07T20:45:09Z
updated_at: 2025-04-16T03:09:31Z
url: https://github.com/astral-sh/uv/issues/12730
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv add` fails with multiple errors

---

_Issue opened by @sreekarchigurupati on 2025-04-07 20:45_

### Summary

I am trying to add a package using `uv add <package name>`

It fails with the following output. I have uninstalled and reinstalled uv to no avail.

```
Resolved 15 packages in 0.28ms
  × Failed to download `dipy==1.11.0`
  ├─▶ Failed to write to the distribution cache
  ├─▶ error decoding response body
  ├─▶ request or response body error
  ├─▶ error reading a body from connection
  ╰─▶ connection reset
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

### Platform

Pop!_OS 22.04 LTS (Linux 6.9.3-76060903-generic x86_64 GNU/Linux)

### Version

uv 0.6.13

### Python version

Python 3.12.9

---

_Label `bug` added by @sreekarchigurupati on 2025-04-07 20:45_

---

_Comment by @zanieb on 2025-04-07 20:58_

Sounds like a network problem? Can you share verbose logs? (`-v`)

---

_Comment by @sreekarchigurupati on 2025-04-07 21:09_

Sharing verbose logs
```
DEBUG uv 0.6.13
DEBUG Found project root: `/home/sreekar/Projects/Distortions`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/sreekar/Projects/Distortions`
DEBUG Reading Python requests from version file at `/home/sreekar/Projects/Distortions/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `/tmp/uv-f47c9653cfcdcfec.lock`
DEBUG No changes to dependencies; skipping update
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: distortions @ file:///home/sreekar/Projects/Distortions
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 15 packages in 0.44ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: dipy==1.11.0
DEBUG Identified uncached distribution: h5py==3.13.0
DEBUG Registry requirement already cached: nibabel==5.3.2
DEBUG Identified uncached distribution: numpy==2.2.4
DEBUG Registry requirement already cached: packaging==24.2
DEBUG Identified uncached distribution: scipy==1.15.2
DEBUG Registry requirement already cached: tqdm==4.67.1
DEBUG Registry requirement already cached: trx-python==0.3
DEBUG Registry requirement already cached: typing-extensions==4.13.1
DEBUG Registry requirement already cached: deepdiff==8.4.2
DEBUG Registry requirement already cached: setuptools-scm==8.2.0
DEBUG Registry requirement already cached: orderly-set==5.3.0
DEBUG Registry requirement already cached: setuptools==78.1.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c0/53/eaada1a414c026673eb983f8b4a55fe5eb172725d33d62c1b21f63ff6ca4/scipy-1.15.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/02/e2/e2cbb8d634151aab9528ef7b8bab52ee4ab10e076509285602c2a3a686e0/numpy-2.2.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/27/96/469627001bdcaf847be2b2c2c895114ded31211e1f0503282f5d244916a0/dipy-1.11.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a7/da/3c137006ff5f0433f0fb076b1ebe4a7bf7b5ee1e8811b5486af98b500dd5/h5py-3.13.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Downloading numpy (15.4MiB)
Downloading scipy (35.6MiB)
Downloading dipy (9.1MiB)
Downloading h5py (4.7MiB)
WARN Streaming failed for numpy==2.2.4; downloading wheel to disk (error decoding response body)
WARN Streaming failed for h5py==3.13.0; downloading wheel to disk (error decoding response body)
WARN Streaming failed for dipy==1.11.0; downloading wheel to disk (error decoding response body)
WARN Streaming failed for scipy==1.15.2; downloading wheel to disk (error decoding response body)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/02/e2/e2cbb8d634151aab9528ef7b8bab52ee4ab10e076509285602c2a3a686e0/numpy-2.2.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a7/da/3c137006ff5f0433f0fb076b1ebe4a7bf7b5ee1e8811b5486af98b500dd5/h5py-3.13.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/27/96/469627001bdcaf847be2b2c2c895114ded31211e1f0503282f5d244916a0/dipy-1.11.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c0/53/eaada1a414c026673eb983f8b4a55fe5eb172725d33d62c1b21f63ff6ca4/scipy-1.15.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Downloading scipy (35.6MiB)
Downloading numpy (15.4MiB)
Downloading dipy (9.1MiB)
Downloading h5py (4.7MiB)
  × Failed to download `numpy==2.2.4`
  ├─▶ Failed to write to the distribution cache
  ├─▶ error decoding response body
  ├─▶ request or response body error
  ├─▶ error reading a body from connection
  ╰─▶ connection reset
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

---

_Comment by @zanieb on 2025-04-08 01:52_

Do you have a proxy or something? Some abnormal network setup of any sort?

---

_Comment by @zanieb on 2025-04-08 01:53_

(I've never seen this before)

---

_Comment by @sreekarchigurupati on 2025-04-08 23:11_

No proxy. Had tailscale running, disabled it. Still the same. Is there any way to debug futher from my side?

---

_Comment by @zanieb on 2025-04-08 23:24_

You could set `RUST_LOG=trace` and we'd get logs from the underlying network stack instead of just uv.

---

_Comment by @zanieb on 2025-04-08 23:25_

Can you reproduce it in a container?

---

_Comment by @sreekarchigurupati on 2025-04-09 04:30_

somehow using RUST_LOG=trace is successfully installing the packages 😅 . 

```
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5131826; stream=1953330
TRACE send_data; sz=16384; window=5131826; available=5242880
TRACE send_data; sz=16384; window=1953330; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5115442; stream=1936946
TRACE send_data; sz=16384; window=5115442; available=5226496
TRACE send_data; sz=16384; window=1936946; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5099058; stream=1920562
TRACE send_data; sz=16384; window=5099058; available=5242880
TRACE send_data; sz=16384; window=1920562; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5082674; stream=1904178
TRACE send_data; sz=16384; window=5082674; available=5242880
TRACE send_data; sz=16384; window=1904178; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5066290; stream=1887794
TRACE send_data; sz=16384; window=5066290; available=5242880
TRACE send_data; sz=16384; window=1887794; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5049906; stream=1871410
TRACE send_data; sz=16384; window=5049906; available=5226496
TRACE send_data; sz=16384; window=1871410; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5033522; stream=1855026
TRACE send_data; sz=16384; window=5033522; available=5242880
TRACE send_data; sz=16384; window=1855026; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5017138; stream=1838642
TRACE send_data; sz=16384; window=5017138; available=5242880
TRACE send_data; sz=16384; window=1838642; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=5000754; stream=1822258
TRACE send_data; sz=16384; window=5000754; available=5242880
TRACE send_data; sz=16384; window=1822258; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4984370; stream=1805874
TRACE send_data; sz=16384; window=4984370; available=5226496
TRACE send_data; sz=16384; window=1805874; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4967986; stream=1789490
TRACE send_data; sz=16384; window=4967986; available=5242880
TRACE send_data; sz=16384; window=1789490; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4951602; stream=1773106
TRACE send_data; sz=16384; window=4951602; available=5242880
TRACE send_data; sz=16384; window=1773106; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4935218; stream=1756722
TRACE send_data; sz=16384; window=4935218; available=5242880
TRACE send_data; sz=16384; window=1756722; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4918834; stream=1740338
TRACE send_data; sz=16384; window=4918834; available=5226496
TRACE send_data; sz=16384; window=1740338; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4902450; stream=1723954
TRACE send_data; sz=16384; window=4902450; available=5242880
TRACE send_data; sz=16384; window=1723954; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4886066; stream=1707570
TRACE send_data; sz=16384; window=4886066; available=5242880
TRACE send_data; sz=16384; window=1707570; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4869682; stream=1691186
TRACE send_data; sz=16384; window=4869682; available=5242880
TRACE send_data; sz=16384; window=1691186; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4853298; stream=1674802
TRACE send_data; sz=16384; window=4853298; available=5242880
TRACE send_data; sz=16384; window=1674802; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=12297
TRACE decoding frame from 12297B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=12288; connection=4836914; stream=1658418
TRACE send_data; sz=12288; window=4836914; available=5226496
TRACE send_data; sz=12288; window=1658418; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=28672
TRACE release_capacity; size=12288
TRACE release_connection_capacity; size=12288, connection in_flight_data=12288
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4824626; stream=1646130
TRACE send_data; sz=16384; window=4824626; available=5242880
TRACE send_data; sz=16384; window=1646130; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4808242; stream=1629746
TRACE send_data; sz=16384; window=4808242; available=5242880
TRACE send_data; sz=16384; window=1629746; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4791858; stream=1613362
TRACE send_data; sz=16384; window=4791858; available=5242880
TRACE send_data; sz=16384; window=1613362; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4775474; stream=1596978
TRACE send_data; sz=16384; window=4775474; available=5226496
TRACE send_data; sz=16384; window=1596978; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4759090; stream=1580594
TRACE send_data; sz=16384; window=4759090; available=5242880
TRACE send_data; sz=16384; window=1580594; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4742706; stream=1564210
TRACE send_data; sz=16384; window=4742706; available=5242880
TRACE send_data; sz=16384; window=1564210; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4726322; stream=1547826
TRACE send_data; sz=16384; window=4726322; available=5242880
TRACE send_data; sz=16384; window=1547826; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4709938; stream=1531442
TRACE send_data; sz=16384; window=4709938; available=5226496
TRACE send_data; sz=16384; window=1531442; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=32768
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4693554; stream=1515058
TRACE send_data; sz=16384; window=4693554; available=5242880
TRACE send_data; sz=16384; window=1515058; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4677170; stream=1498674
TRACE send_data; sz=16384; window=4677170; available=5242880
TRACE send_data; sz=16384; window=1498674; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=16384
TRACE connection.state=Open
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4660786; stream=1482290
TRACE send_data; sz=16384; window=4660786; available=5242880
TRACE send_data; sz=16384; window=1482290; available=2097152
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=16393
TRACE decoding frame from 16393B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1) }
TRACE recv DATA frame=Data { stream_id: StreamId(1) }
TRACE recv_data; size=16384; connection=4644402; stream=1465906
TRACE send_data; sz=16384; window=4644402; available=5226496
TRACE send_data; sz=16384; window=1465906; available=2080768
TRACE transition_after; stream=StreamId(1); state=State { inner: HalfClosedLocal(Streaming) }; is_closed=false; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE poll
TRACE read.bytes=9747
TRACE decoding frame from 9747B
TRACE frame.kind=Data
DEBUG received frame=Data { stream_id: StreamId(1), flags: (0x1: END_STREAM) }
TRACE recv DATA frame=Data { stream_id: StreamId(1), flags: (0x1: END_STREAM) }
TRACE recv_data; size=9738; connection=4628018; stream=1449522
TRACE send_data; sz=9738; window=4628018; available=5210112
TRACE recv_close: HalfClosedLocal => Closed
TRACE send_data; sz=9738; window=1449522; available=2064384
TRACE transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=1
TRACE dec_num_streams; stream=StreamId(1)
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=42506
TRACE release_capacity; size=16384
TRACE release_connection_capacity; size=16384, connection in_flight_data=26122
TRACE release_capacity; size=9738
TRACE release_connection_capacity; size=9738, connection in_flight_data=9738
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 1, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(65535), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(1439784), available: Window(2097152) }, in_flight_recv_data: 0, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: false, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Remaining(0) }
TRACE transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
 Downloaded scipy
Prepared 1 package in 10.22s
TRACE Extracting file name=PackageName("scipy")
TRACE Extracting file name=PackageName("trx-python")
TRACE Extracting file name=PackageName("typing-extensions")
TRACE Extracting file name=PackageName("setuptools-scm")
TRACE Extracting file name=PackageName("deepdiff")
TRACE Extracting file name=PackageName("h5py")
TRACE Extracting file name=PackageName("tqdm")
TRACE Extracting file name=PackageName("orderly-set")
TRACE Extracting file name=PackageName("packaging")
TRACE Extracting file name=PackageName("nibabel")
TRACE Extracting file name=PackageName("setuptools")
TRACE Extracting file name=PackageName("dipy")
TRACE Extracting file name=PackageName("numpy")
TRACE Extracted 9 files name=PackageName("orderly-set")
TRACE Extracted 5 files name=PackageName("typing-extensions")
TRACE No entrypoints name=PackageName("orderly-set")
TRACE No entrypoints name=PackageName("typing-extensions")
TRACE No data name=PackageName("typing-extensions")
TRACE Writing installer metadata name=PackageName("typing-extensions")
TRACE No data name=PackageName("orderly-set")
TRACE Writing installer metadata name=PackageName("orderly-set")
TRACE Writing record name=PackageName("typing-extensions")
TRACE Extracted 23 files name=PackageName("packaging")
TRACE Writing record name=PackageName("orderly-set")
TRACE No entrypoints name=PackageName("packaging")
TRACE No data name=PackageName("packaging")
TRACE Writing installer metadata name=PackageName("packaging")
TRACE Extracted 40 files name=PackageName("tqdm")
TRACE Extracted 36 files name=PackageName("setuptools-scm")
TRACE Writing record name=PackageName("packaging")
TRACE Extracted 25 files name=PackageName("deepdiff")
TRACE No entrypoints name=PackageName("setuptools-scm")
TRACE No data name=PackageName("setuptools-scm")
TRACE Writing installer metadata name=PackageName("setuptools-scm")
TRACE Extracted 26 files name=PackageName("trx-python")
TRACE No entrypoints name=PackageName("trx-python")
TRACE Installing data/scripts dist_name=PackageName("trx-python")
TRACE Writing record name=PackageName("setuptools-scm")
TRACE Extracted 94 files name=PackageName("h5py")
TRACE No entrypoints name=PackageName("h5py")
TRACE No data name=PackageName("h5py")
TRACE Writing installer metadata name=PackageName("h5py")
TRACE Writing entrypoints name=PackageName("tqdm")
TRACE Writing record name=PackageName("h5py")
TRACE Writing entrypoints name=PackageName("deepdiff")
TRACE No data name=PackageName("tqdm")
TRACE Writing installer metadata name=PackageName("tqdm")
TRACE Writing record name=PackageName("tqdm")
TRACE No data name=PackageName("deepdiff")
TRACE Writing installer metadata name=PackageName("deepdiff")
TRACE Writing record name=PackageName("deepdiff")
TRACE Writing installer metadata name=PackageName("trx-python")
TRACE Writing record name=PackageName("trx-python")
TRACE Extracted 348 files name=PackageName("nibabel")
TRACE Writing entrypoints name=PackageName("nibabel")
TRACE No data name=PackageName("nibabel")
TRACE Writing installer metadata name=PackageName("nibabel")
TRACE Writing record name=PackageName("nibabel")
TRACE Extracted 568 files name=PackageName("dipy")
TRACE Writing entrypoints name=PackageName("dipy")
TRACE Extracted 514 files name=PackageName("setuptools")
TRACE No entrypoints name=PackageName("setuptools")
TRACE No data name=PackageName("setuptools")
TRACE Writing installer metadata name=PackageName("setuptools")
TRACE Writing record name=PackageName("setuptools")
TRACE Installing data/data dist_name=PackageName("dipy")
TRACE Extracted 1424 files name=PackageName("scipy")
TRACE No entrypoints name=PackageName("scipy")
TRACE No data name=PackageName("scipy")
TRACE Writing installer metadata name=PackageName("scipy")
TRACE Writing record name=PackageName("scipy")
TRACE Extracted 875 files name=PackageName("numpy")
TRACE Writing entrypoints name=PackageName("numpy")
TRACE No data name=PackageName("numpy")
TRACE Writing installer metadata name=PackageName("numpy")
TRACE Writing record name=PackageName("numpy")
TRACE Writing installer metadata name=PackageName("dipy")
TRACE Writing record name=PackageName("dipy")
Installed 13 packages in 30ms
 + deepdiff==8.4.2
 + dipy==1.11.0
 + h5py==3.13.0
 + nibabel==5.3.2
 + numpy==2.2.4
 + orderly-set==5.3.0
 + packaging==24.2
 + scipy==1.15.2
 + setuptools==78.1.0
 + setuptools-scm==8.2.0
 + tqdm==4.67.1
 + trx-python==0.3
 + typing-extensions==4.13.1
TRACE Streams::recv_eof
```

---

_Comment by @sreekarchigurupati on 2025-04-16 03:09_

I'm no longer facing this issue, could have been an intermittent network issue. I'm closing the issue

---

_Closed by @sreekarchigurupati on 2025-04-16 03:09_

---
