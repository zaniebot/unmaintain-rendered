---
number: 1710
title: "`Failed to download distributions` with `unexpected BufError` errors"
type: issue
state: open
author: humanzz
labels:
  - bug
  - error messages
  - network
assignees: []
created_at: 2024-02-19T17:11:46Z
updated_at: 2024-08-12T12:18:18Z
url: https://github.com/astral-sh/uv/issues/1710
synced_at: 2026-01-10T01:23:08Z
---

# `Failed to download distributions` with `unexpected BufError` errors

---

_Issue opened by @humanzz on 2024-02-19 17:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello,

My team develops locally mostly using MacOS, but we use a CI/CD (Amazon Linux 2) system as part of our development process.

We have a python package, with `pyproject.toml` and we use https://github.com/pyinvoke/invoke to manage our build scripts.

The main task in question now in `tasks.py`, uses `uv pip install` and looks as follows

```
UV_ENV = "UV_CACHE_DIR=<path> UV_INDEX_URL=<a non default index> "

@task
def develop(ctx):
    ctx.run(UV_ENV + "uv pip install --verbose -e '.[dev,testing]'", echo=True)
```

We use `uv==0.1.5`.

We're trying to move to using `uv` for its speed gains.
On MacOS, everything works as expected.
On the CI/CD host, we sometimes - more often than not - run into an error that originates from the `uv pip install` call in the `develop` task.

The error consistently looks as follows. The error sometimes changes the dependency it's failing to fetch.

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: numpy==1.26.4
  Caused by: Failed to extract source distribution
  Caused by: unexpected BufError
```

I'm attaching the `--verbose` output for a failed run and its successful retry. Note that I've slightly modified this to drop index urls, and package name, but the full list of dependencies should be there at the end of 2nd successful attempt.

[uv_attempt1_fail.log](https://github.com/astral-sh/uv/files/14335151/uv_attempt1_fail.log)
[uv_attempt2_success.log](https://github.com/astral-sh/uv/files/14335152/uv_attempt2_success.log)

---

_Comment by @zanieb on 2024-02-19 17:19_

Interesting thanks for the thorough report!

Related:
- #1534 

This looks like an issue with our `uv-extract` crate. At the very least we should provide more information about what's gone wrong.

---

_Label `bug` added by @zanieb on 2024-02-19 17:19_

---

_Comment by @humanzz on 2024-02-19 18:23_

Yeah. It's very tough to know what's going wrong here with such short message.

Another point on `pip install` and its output: `verbose` is too chatty, but omitting `verbose` is also too silent. `pip` on the other hand does print out some useful things like what's being downloaded, what's already satisfied, what index was used, etc. which are useful bits of info, that are much shorter than the chatty verbose output. It'd be nice if `uv` does a similar thing.

---

_Comment by @zanieb on 2024-02-19 18:25_

Yep agree see https://github.com/astral-sh/uv/issues/1569

---

_Comment by @humanzz on 2024-02-19 18:32_

Awesome, I'll follow there for progress on that!

---

_Comment by @humanzz on 2024-02-26 23:31_

I've started seeing a similar error on Mac (Apple Silicon), this time when using `pip compile` and `uv==0.1.11` e.g.

```
UV_CACHE_DIR=<a cache dir> UV_INDEX_URL=<a custom index> uv pip compile pyproject.toml --all-extras --upgrade -o requirements.lock

error: Failed to download: ipython==8.22.1
  Caused by: The wheel ipython-8.22.1-py3-none-any.whl is not a valid zip file
  Caused by: an upstream reader returned an error: unexpected BufError
  Caused by: unexpected BufError
```

---

_Comment by @zanieb on 2024-02-27 04:54_

It looks like maybe this comes from [`flate2`](https://docs.rs/flate2/latest/flate2/enum.Status.html#variant.BufError) via [`async-compression`](https://github.com/Nullus157/async-compression/blob/bd46497b98117775d8921f9eb72ede84f179548f/src/codec/flate/decoder.rs#L54C21-L54C29) via [`rs-async-zip`](https://github.com/Majored/rs-async-zip) which I think we call at https://github.com/astral-sh/uv/blob/8d721830db8ad75b8b7ef38edc0e346696c52e3d/crates/uv-extract/src/stream.rs#L22

> For decompression this means that more input is needed to continue or the output buffer isn’t large enough to contain the result. The function can be called again after fixing both.

It seems like either:

- Your custom index is returning truncated output
- Our output buffer is overflowing somehow — I haven't looked into if this is even possible yet

---

_Comment by @humanzz on 2024-02-28 19:38_

So, a bit more information - though likely a bit vague.
The setup I have, essentially runs a local proxy on my machine. I checked the logs of that proxy, to check entries when using `pip` and when using `uv pip`.

When using `pip`, logs look fine, no exceptions/errors.
When using `uv pip`, I did see some errors along the lines of 

```
ERROR org.glassfish.jersey.server.ServerRuntime$Responder - An I/O error has occurred while writing a response message entity to the container output stream. - org.glassfish.jersey.server.internal.process.MappableException: java.io.IOException: Buffer overflow
...
...
Caused by: java.io.IOException: Buffer overflow.
	at org.glassfish.jersey.netty.connector.internal.JerseyChunkedInput.write(JerseyChunkedInput.java:211) ~[jersey-netty-connector-2.31.jar:?]
	at org.glassfish.jersey.netty.connector.internal.JerseyChunkedInput.write(JerseyChunkedInput.java:191) ~[jersey-netty-connector-2.31.jar:?]
	at org.glassfish.jersey.netty.connector.internal.JerseyChunkedInput.write(JerseyChunkedInput.java:182) ~[jersey-netty-connector-2.31.jar:?]
	at org.glassfish.jersey.message.internal.CommittingOutputStream.write(CommittingOutputStream.java:185) ~[jersey-common-2.35.jar:?]
```

I feel like I've read somewhere, on some of the issues, that `uv`, as an optimization, can stream files to read specific parts it might need. I wonder if maybe there's some weird interaction here, `uv` is reading the part and closing the connection, and that proxy is not properly handling that.

I'm not sure, really, but trying to hypothesize what might be wrong, given I don't have access to that proxy code, and I'm Rust-ignorant :(

---

_Comment by @charliermarsh on 2024-02-28 19:57_

It's reasonable that we should just fall back to non-range requests when we see a `BufError`.

---

_Referenced in [astral-sh/uv#2053](../../astral-sh/uv/pulls/2053.md) on 2024-02-28 20:34_

---

_Comment by @zanieb on 2024-03-01 15:52_

I would recommend reporting that buffer overflow as a bug to the proxy you're using (if you can), it seems like the proxy is failing to buffer the streamed data correctly sometimes.

---

_Label `network` added by @konstin on 2024-07-02 09:35_

---

_Comment by @chrisirhc on 2024-08-12 08:44_

In case it'd help someone else, I got this `BufError` error when the wheel file itself was corrupted.
In our case, the cache was serving a corrupted file (2.5MB), whereas the same file on public pypi is 9.6 MB, and uv was correctly failing when trying to use it. 

It was just a bit difficult to understand what was the nature of the failure based on the error `BufError`.

---

_Label `error messages` added by @zanieb on 2024-08-12 12:18_

---

_Referenced in [astral-sh/uv#6349](../../astral-sh/uv/issues/6349.md) on 2024-08-21 16:10_

---
