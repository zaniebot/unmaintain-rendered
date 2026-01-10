```yaml
number: 2220
title: "Small reads in uv_distribution::distribution_database::download producing v. slow download performance when installing torch"
type: issue
state: closed
author: thundergolfer
labels:
  - performance
  - great writeup
assignees: []
created_at: 2024-03-05T20:44:12Z
updated_at: 2024-03-10T14:52:19Z
url: https://github.com/astral-sh/uv/issues/2220
synced_at: 2026-01-10T05:40:32Z
```

# Small reads in uv_distribution::distribution_database::download producing v. slow download performance when installing torch

---

_Issue opened by @thundergolfer on 2024-03-05 20:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey, this is a successor to https://github.com/astral-sh/uv/pull/1978 as I'm still trying to integrate `uv` with Modal's internal mirror. But this time I don't yet have a solution to this problem 🙂. 

I'm observed extremely degraded install performance when installing pytorch against our mirror. The install command is `uv pip install torch`. 

On my test machine, installing **_without_** our mirror is fast, taking around 7 seconds.

<img width="691" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/fd7a6c9b-e2c6-45e6-8d5d-eeadafc6eaaa">

But when I get our mirror involved, performance tanks and an install takes over 5 minutes. The command I'm running is this:

```bash
RUST_LOG="uv=trace,uv_extrace=trace,async_zip=debug" \
UV_NO_CACHE=true UV_INDEX_URL="http://172.21.0.1:5555/simple" \
uv --verbose pip install torch
```

### Investigating...

Using debug logs on our mirror I see that there is a vast (~64x) difference in read sizes between `uv pip install` and `pip install`. 

Our mirror serves `.whl` files off disk like this: 

```rust
use hyper::{Body, Response};
use tokio_util::codec::{BytesCodec, FramedRead};

// ... 

let file tokio::fs::File::open(&filepath).await?;
debug!("cache hit: serving {} from disk cache", filepath.display());
let size = file.metadata().await.map(|m| Some(m.len())).unwrap_or(None);
let stream = FramedRead::new(file, BytesCodec::new());
let body = Body::wrap_stream(stream);
let mut response = Response::new(body);
```
By enabling debug logs (`RUST_LOG=debug`) I can see that when using **`pip install`** read chunks are large:

<kbd>
<img width="1331" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/8760e92d-e054-48ad-8746-4e5192931aa7">
</kbd>

We can see in the screenshot of the debug logs that reads up to 128KiB are happening. Performance of the `pip install` command is good, the whl download takes around 3 seconds. 

The `pip` command here was 

```
PIP_TRUSTED_HOST="172.21.0.1:5555" \
PIP_NO_CACHE_DIR=off PIP_INDEX_URL="http://172.21.0.1:5555/simple" \
pip install torch
``` 

and `pip --version` is `pip 23.3.1 from /home/ubuntu/modal/venv/lib/python3.11/site-packages/pip (python 3.11)`. 

The read sizes of `pip` contrast strongly with **`uv pip install`**, where I'm seeing read sizes between `11` and `1120` bytes. 

<img width="1823" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/622b3f85-cb44-4a4c-a501-9d73c83f0038">

I believe these relatively tiny read sizes when downloading are contributing to the slow download performance on the 720.55MiB torch `.whl` file.

Looking at the `trace` logs this is the region of code I'm in: https://github.com/astral-sh/uv/blob/043d72646d37a5d740edb2c68ebd26a78dc5eb08/crates/uv-distribution/src/distribution_database.rs#L161

----

**`uv` version**

```
uv --version
uv 0.1.14
```

**OS**

```bash
uname -a
Linux ip-10-1-1-198 5.15.0-1052-aws #57~20.04.1-Ubuntu SMP Mon Jan 15 17:04:56 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

---

_Comment by @thundergolfer on 2024-03-05 20:45_

I'm happy to look more into what's going on later in the week 🙂. I've read through some of the code in `uv` and `async_zip` but haven't figured out what's going on. 

---

_Comment by @charliermarsh on 2024-03-05 20:48_

Cool, would definitely welcome more exploration / eyes on it and happy to support. We unzip the wheel to disk directly as we stream the download in-memory, it looks like we're making way too many small reads right now?

---

_Comment by @thundergolfer on 2024-03-05 20:55_

Yeah, I added `BufReader` in a couple places which was ineffectual, and see there's already `BufReader` used in a couple places (in uv and in async_zip) despite seeing reads << than the 8KiB default buffer size. 

It'd take me a decent bit more time to unpack why these small reads are being produced and  slap a buffered reader in the right place. 

---

_Comment by @charliermarsh on 2024-03-05 20:57_

Is there any way to reproduce this with publicly available indexes?

---

_Comment by @thundergolfer on 2024-03-05 20:59_

Yeh I think that's job 1 so that we can collab better on this! We won't open-source our mirror implementation just yet. 

---

_Renamed from "Small reads in uv_distribution::distribution_database::download producing v. slow download performance when install torch" to "Small reads in uv_distribution::distribution_database::download producing v. slow download performance when installing torch" by @thundergolfer on 2024-03-05 21:01_

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 22:45_

This behavior seems very similar to one I’m seeing as well. For our internal index, proxied public packages have good performance even when wheel is large (PyTorch), but internal packages with large wheels are much slower with uv then pip.

The only thing I can add is I know for our index we don’t support range requests/head so maybe the fallback path there is needed to trigger slow behavior here.

---

_Label `performance` added by @zanieb on 2024-03-06 00:03_

---

_Label `great writeup` added by @zanieb on 2024-03-06 00:03_

---

_Comment by @charliermarsh on 2024-03-06 06:03_

We don't use range requests for the wheel _download_ IIRC, just for fetching wheel metadata, so hopefully not a factor here.

---

_Comment by @thundergolfer on 2024-03-09 04:56_

Adding some details of attempting a repro with an open-source mirror, [proxpi](https://github.com/EpicWink/proxpi): `docker run -p 5000:5000 epicwink/proxpi`. 

```
time UV_NO_CACHE=true UV_INDEX_URL="http://127.0.0.1:5000/index" target/release/uv --verbose pip install torch
```

Has `proxpi` log a whl download time of around 7.4s. 

```
2024-03-09 04:37:34 [    INFO] gunicorn.access: 172.18.0.1 "GET /index/torch/torch-2.2.1-cp311-cp311-manylinux1_x86_64.whl HTTP/1.1" 200 0 7431ms
```

With just `pip`:

```bash
PIP_TRUSTED_HOST="127.0.0.1:5000" PIP_NO_CACHE_DIR=off PIP_INDEX_URL="http://127.0.0.1:5000/index" pip install torch
```

```bash
2024-03-09 04:39:12 [    INFO] gunicorn.access: 172.18.0.1 "GET /index/torch/ HTTP/1.1" 200 24056 7ms
2024-03-09 04:39:14 [    INFO] gunicorn.access: 172.18.0.1 "GET /index/torch/torch-2.2.1-cp311-cp311-manylinux1_x86_64.whl HTTP/1.1" 200 0 2080ms
```

I get around 2s. 

So there's a difference, but nothing like the ~5 minute slowdown seen in our internal mirror. So a negative result on providing an open-source reproduction. I'll keep digging. 

---

_Comment by @charliermarsh on 2024-03-09 13:16_

I'd really like to fix this. I'll try to play around with it today. The simplest thing we should try:

```diff
diff --git a/crates/uv-extract/src/stream.rs b/crates/uv-extract/src/stream.rs
index 8d8dfb11..b9272df5 100644
--- a/crates/uv-extract/src/stream.rs
+++ b/crates/uv-extract/src/stream.rs
@@ -18,7 +18,7 @@ pub async fn unzip<R: tokio::io::AsyncRead + Unpin>(
     target: impl AsRef<Path>,
 ) -> Result<(), Error> {
     let target = target.as_ref();
-    let mut reader = reader.compat();
+    let mut reader = futures::io::BufReader::new( reader.compat());
     let mut zip = async_zip::base::read::stream::ZipFileReader::new(&mut reader);
 
     let mut directories = FxHashSet::default();
```

(It looks like the async ZIP reader uses a BufReader internally, but only for the entry _bodies_, and not the headers.)

---

_Comment by @charliermarsh on 2024-03-09 15:47_

Not seeing any improvement from this change.

FWIW, on my machine, the difference is much smaller with the open-source proxy.

pip:

```
gunicorn.access: 172.17.0.1 "GET /index/torch/torch-2.2.1-cp312-none-macosx_11_0_arm64.whl HTTP/1.1" 200 0 888ms
```

uv:

```
gunicorn.access: 172.17.0.1 "GET /index/torch/torch-2.2.1-cp312-none-macosx_11_0_arm64.whl HTTP/1.1" 200 0 1374ms
```

Which is maybe plausible given that we're unzipping and writing to disk -- not sure.

---

_Comment by @charliermarsh on 2024-03-09 17:09_

We could also try increasing the buffer size.

---

_Comment by @thundergolfer on 2024-03-09 18:21_

Ok from looking at trace logs for our internal mirror I think the encoding strategy difference is key: 

**`pip`**

```
2024-03-09T17:41:23.715190Z DEBUG pypi_mirror: cache hit: serving /tmp/pypi-mirror-worker-cache/packages/2c/df/5810707da6f2fd4be57f0cc417987c0fa16a2eecf0b1b71f82ea555dc619/torch-2.2.1-cp311-cp311-manylinux1_x86_64.whl from disk cache
2024-03-09T17:41:23.715200Z TRACE hyper::proto::h1::conn: flushed({role=server}): State { reading: KeepAlive, writing: Init, keep_alive: Busy }
2024-03-09T17:41:23.715260Z TRACE encode_headers: hyper::proto::h1::role: Server::encode status=200, body=Some(Unknown), req_method=Some(GET)
2024-03-09T17:41:23.715329Z DEBUG hyper::proto::h1::io: flushed 83 bytes
2024-03-09T17:41:23.715336Z TRACE hyper::proto::h1::conn: flushed({role=server}): State { reading: KeepAlive, writing: Body(Encoder { kind: Length(755552143), is_last: false }), keep_alive: Busy }
2024-03-09T17:41:23.715355Z TRACE hyper::proto::h1::encode: sized write, len = 8192
2024-03-09T17:41:23.715362Z TRACE hyper::proto::h1::io: buffer.queue self.len=0 buf.len=8192
```

**`uv`**

```
2024-03-09T17:40:37.658227Z DEBUG pypi_mirror: cache hit: serving /tmp/pypi-mirror-worker-cache/packages/2c/df/5810707da6f2fd4be57f0cc417987c0fa16a2eecf0b1b71f82ea555dc619/torch-2.2.1-cp311-cp311-manylinux1_x86_64.whl from disk cache
2024-03-09T17:40:37.658414Z TRACE hyper::proto::h1::conn: flushed({role=server}): State { reading: KeepAlive, writing: Init, keep_alive: Busy }
2024-03-09T17:40:37.658530Z TRACE encode_headers: hyper::proto::h1::role: Server::encode status=200, body=Some(Unknown), req_method=Some(GET)
2024-03-09T17:40:37.658576Z DEBUG hyper::proto::h1::io: flushed 108 bytes
2024-03-09T17:40:37.658591Z TRACE hyper::proto::h1::conn: flushed({role=server}): State { reading: KeepAlive, writing: Body(Encoder { kind: Chunked, is_last: false }), keep_alive: Busy }
2024-03-09T17:40:37.658755Z TRACE hyper::proto::h1::encode: encoding chunked 10B
2024-03-09T17:40:37.658759Z TRACE hyper::proto::h1::io: buffer.queue self.len=0 buf.len=15
```

Will work out why this difference occurs.


---

_Comment by @charliermarsh on 2024-03-09 18:33_

Ohh nice!

---

_Comment by @thundergolfer on 2024-03-09 20:26_

I think this is another `content-encoding` related issue.

I added some extra `trace!` logs into hyper when it was running `encode_headers` and deciding on the encoding strategy.

**uv**

```
Server::encode head=MessageHead { version: HTTP/1.1, subject: 200, headers: {"content-encoding": "gzip"}, extensions: Extensions }, status=200, body=Some(Unknown), req_method=Some(GET)
```

**pip**

```
Server::encode head=MessageHead { version: HTTP/1.1, subject: 200, headers: {"content-length": "755552143"}, extensions: Extensions }, status=200, body=Some(Unknown), req_method=Some(GET)
```

Seeing `gzip` in the headers cued me to think this was just the same issue as https://github.com/astral-sh/uv/pull/1978 showing up in a different place.

By doing the same header change that's done in https://github.com/astral-sh/uv/pull/1978 torch installs much, much faster against the mirror!

<img width="1458" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/815fcc8a-624f-485a-9d18-bbf591e5d1ef">

A 15.64s second download against the mirror is still slower than what `uv` gets against PyPI (around 10s) so there may be some additional change I can make to get performance of our mirror to parity with PyPi. It really should be faster as it's serving over the local network. 

Checking the `pip` repository for its history of dealing with `content-encoding` I see this https://github.com/pypa/pip/pull/1688



---

_Comment by @thundergolfer on 2024-03-09 20:30_

Looking at the flushes in our mirror after adding the `identity` content-encoding header, I can now see they're averaging around 8KiB, much higher than the ~1KiB before: 

<kbd>
<img width="890" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/7b30f5b3-f14f-4822-852a-b642df097e14">
</kbd>

Still lower than what `pip` was producing in our mirror, which went up to 128KiB. 

---

_Comment by @charliermarsh on 2024-03-09 20:30_

Can you try tweaking the buffer size on the BufReader? I think 8KiB might be the exact default.

---

_Comment by @thundergolfer on 2024-03-09 20:31_

You mean doing this but with a non-default buffer size? https://github.com/astral-sh/uv/issues/2220#issuecomment-1986854785



---

_Comment by @charliermarsh on 2024-03-09 20:47_

Yeah. Alternatively I can change the buffer used inside async-zip-rs, we use a fork anyway.

---

_Comment by @charliermarsh on 2024-03-09 20:54_

(I don’t have much intuition for how nested BufReaders interact.)

---

_Comment by @thundergolfer on 2024-03-09 22:34_

Yep with this diff I get comparable performance to `pip` 🙂 

```diff
diff --git a/crates/uv-extract/src/stream.rs b/crates/uv-extract/src/stream.rs
index 8d8dfb11..db3c42fb 100644
--- a/crates/uv-extract/src/stream.rs
+++ b/crates/uv-extract/src/stream.rs
@@ -18,7 +18,7 @@ pub async fn unzip<R: tokio::io::AsyncRead + Unpin>(
     target: impl AsRef<Path>,
 ) -> Result<(), Error> {
     let target = target.as_ref();
-    let mut reader = reader.compat();
+    let mut reader = futures::io::BufReader::with_capacity( 128 * 1024, reader.compat());
     let mut zip = async_zip::base::read::stream::ZipFileReader::new(&mut reader);

     let mut directories = FxHashSet::default();
```

Debug logging on `hyper` shows flushes up-to 128KiB.

----

<kbd>
<img width="1458" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/69fefa38-a7c9-442c-89c5-d6565c275d48">
</kbd>


---

_Comment by @thundergolfer on 2024-03-09 22:37_

Think this is a enough to make a PR. It'd be two things:

1. setting `identity`, in the spirit of https://github.com/pypa/pip/pull/1688
2. increasing buffer from 8KiB to 128KiB. 

---

_Comment by @charliermarsh on 2024-03-09 22:42_

Sounds great, thank you! The only thing I’ll want to test before merging is whether the buffer size changes benchmarks much / at all against PyPI.

---

_Comment by @thundergolfer on 2024-03-09 22:44_

Makes sense. Looks like I could even check this myself by reading `BENCHMARKS.md` and going from there? 

---

_Comment by @charliermarsh on 2024-03-09 22:47_

Totally, up to you! I'd just do a release build on main, move it to `uv-main`, then do a release build on your branch, then run something ike:

```
python -m scripts.bench \
        --uv-path ./target/release/uv \
        --uv-path ./target/release/uv-main \
        ./scripts/requirements/compiled/trio.txt --benchmark install-cold
```

---

_Comment by @charliermarsh on 2024-03-09 22:48_

By the way, I can get this released pretty quickly.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 03:28_

I tested the fix on my slow large wheel and it works great. Installation time is now fast for that 1 wheel and I guess my index server had same issue here. Thanks to both of you for continuing to improve uv's performance.

---

_Comment by @charliermarsh on 2024-03-10 03:30_

All @thundergolfer!

---

_Comment by @charliermarsh on 2024-03-10 03:30_

I'll probably cut a release tomorrow with this in it.

---

_Closed by @zanieb on 2024-03-10 14:52_

---
