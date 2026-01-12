```yaml
number: 14171
title: Python installation fails during streamed unpack with connection reset error
type: issue
state: closed
author: zanieb
labels:
  - bug
  - ci-flake
assignees: []
created_at: 2025-06-20T20:29:16Z
updated_at: 2025-09-04T12:45:02Z
url: https://github.com/astral-sh/uv/issues/14171
synced_at: 2026-01-12T16:01:44Z
```

# Python installation fails during streamed unpack with connection reset error

---

_@zanieb_

```
    error: Failed to install cpython-3.10.17-windows-x86_64-none
      Caused by: Failed to extract archive: cpython-3.10.17-20250529-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
      Caused by: failed to unpack `\\?\V:\uv-tmp\.tmp43ZfS8\temp\managed\.temp\.tmpUUY8ru\python\Lib\zoneinfo\_zoneinfo.py`
      Caused by: failed to unpack `python/Lib/zoneinfo/_zoneinfo.py` into `\\?\V:\uv-tmp\.tmp43ZfS8\temp\managed\.temp\.tmpUUY8ru\python\Lib\zoneinfo\_zoneinfo.py`
      Caused by: error decoding response body
      Caused by: request or response body error
      Caused by: error reading a body from connection
      Caused by: connection reset
```

---

_Label `bug` added by @zanieb on 2025-06-20 20:29_

---

_Comment by @zanieb on 2025-06-20 20:29_

Perhaps related to https://github.com/astral-sh/uv/issues/14069

---

_Label `ci-flake` added by @zanieb on 2025-06-20 20:29_

---

_Comment by @zanieb on 2025-06-20 20:30_

I presume this is both a CI stability problem, and a user-facing issue.

---

_Comment by @konstin on 2025-06-27 17:51_

The problem is that we don't retry once we turns the response into a stream. To fix this, we need to wrap the whole streaming logic into a retry loop.

Note that the request itself already has retries, but we need a custom outside loop as we do with some of the `is_extended_transient_error`, e.g. below, except for the whole stream-and-unpack range: https://github.com/astral-sh/uv/blob/62365d4ec86c146fc1d2050bff3fa70e53abfa08/crates/uv-client/src/cached_client.rs#L632-L670

See https://github.com/astral-sh/uv/issues/14069

---

_Assigned to @oconnor663 by @zanieb on 2025-06-27 17:52_

---

_Comment by @oconnor663 on 2025-06-27 19:02_

Correct me if I'm wrong, the relevant code that converts the request to a stream is here: https://github.com/astral-sh/uv/blob/6a5d2f1ec4b8a43a38fb29cf31d57fd9f2bc362f/crates/uv-python/src/downloads.rs#L1292-L1295

If this were going to implement retrying mid-stream, it would need to transparently start a fresh request, convert that new request to a stream, and then skip the number of bytes in that new stream that the client has already read from the old one. That gets more complicated in a couple ways:
- We probably want to try to set a `Range: bytes=...` header and handle `206 Partial Content` from servers that are smart enough to support it, so that we don't unnecessarily re-fetch the same bytes? But we don't want to assume that the server supports that header?
- Do we care about checking whether the skipped bytes are the same as the ones we originally received, and failing if not? I guess we can't do that if we support partial content, so maybe we ignore this problem and hope for the best?

Does it sound like I'm understanding this right?

---

_Comment by @oconnor663 on 2025-06-27 19:12_

Or maybe I shouldn't be trying to do this transparently, and instead I should just modify the caller to retry at a higher level? Though then you want to carefully distinguish network errors from downstream errors (like a corrupt ZIP file or whatever), which might be tricky?

Edit: Ok no actually we already do some of this in `is_extended_transient_error`. Probably I'll just follow that approach.

---

_Comment by @oconnor663 on 2025-06-27 20:29_

My current theory is that this [should already be retrying](https://github.com/astral-sh/uv/blob/6a5d2f1ec4b8a43a38fb29cf31d57fd9f2bc362f/crates/uv-client/src/base_client.rs#L900), but the bug is that `#[error(transparent)]` interferes with the `downcast_ref` chaining that we're doing in `find_source`/`find_sources`. I need to play with it more to be sure.

Specifically this annotation right here: https://github.com/astral-sh/uv/blob/6a5d2f1ec4b8a43a38fb29cf31d57fd9f2bc362f/crates/uv-extract/src/error.rs#L9-L10

I think the problem is that the type isn't `io::Error` (it's `uv_extract::Error`), but also the `.source()` is `None`. It's like we "skip over" the actual `io::Error` in source chaining.

---

_Comment by @oconnor663 on 2025-06-27 22:58_

Ok this is weird. When I inject a bare `io::ErrorKind::ConnectionReset` error like this, it _doesn't_ get retried (I'm pretty sure because of the `downcast_ref` issue mentioned above):

```diff
diff --git a/crates/uv-python/src/downloads.rs b/crates/uv-python/src/downloads.rs
index 51f6f1d45..0fa70813f 100644
--- a/crates/uv-python/src/downloads.rs
+++ b/crates/uv-python/src/downloads.rs
@@ -1254,6 +1256,21 @@ where
     }
 }
 
+struct BadReader {}
+
+impl AsyncRead for BadReader {
+    fn poll_read(
+        self: Pin<&mut Self>,
+        _cx: &mut Context<'_>,
+        _buf: &mut ReadBuf<'_>,
+    ) -> Poll<io::Result<()>> {
+        return Poll::Ready(Err(io::Error::new(
+            io::ErrorKind::ConnectionReset,
+            "Jack reset woo",
+        )));
+    }
+}
+
 /// Convert a [`Url`] into an [`AsyncRead`] stream.
 async fn read_url(
     url: &Url,
@@ -1289,11 +1308,11 @@ async fn read_url(
             .map_err(|err| Error::from_reqwest(url, err, retry_count))?;
 
         let size = response.content_length();
-        let stream = response
+        let _stream = response
             .bytes_stream()
             .map_err(io::Error::other)
             .into_async_read();
 
-        Ok((Either::Right(stream.compat()), size))
+        Ok((Either::Right(BadReader {}), size))
     }
 }

```

If I `dbg!` the error in that case, it looks like this:

```
[crates/uv-python/src/downloads.rs:716:64] err = ExtractError(
    "cpython-3.8.20-20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz",
    Io(
        Custom {
            kind: ConnectionReset,
            error: "Jack reset woo",
        },
    ),
)
```

However, when I try to reproduce this over the actual network, it _does_ get retried. Here's the server I'm using:

```rust
use std::io::BufReader;
use std::io::prelude::*;

const BAD_RESPONSES: u64 = 2;

const ZIP_PATH: &str =
    "/tmp/cpython-3.8.20+20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz";

fn main() -> anyhow::Result<()> {
    let zip_bytes = std::fs::read(ZIP_PATH)?;
    let listener = std::net::TcpListener::bind("127.0.0.1:8000")?;
    let mut bad_responses_so_far = 0u64;
    loop {
        let (mut stream, _) = listener.accept()?;

        // Read and print the (presumably) GET headers. NB: If we close the connection before
        // reading all of these, the sender gets BrokenPipe or ConnectionReset on their *write*,
        // which isn't what we want.
        let mut reader = BufReader::new(&stream);
        let mut line = String::new();
        while reader.read_line(&mut line)? != 0 {
            println!("{}", line.trim());
            if line == "\r\n" {
                break; // end of headers
            }
            line.clear();
        }

        let response_start = "HTTP/1.1 200 OK\r\n\r\n";
        stream.write_all(response_start.as_bytes())?;

        // Write the first half of the ZIP bytes.
        let half = zip_bytes.len() / 2;
        stream.write_all(&zip_bytes[..half])?;

        if bad_responses_so_far < BAD_RESPONSES {
            let socket = socket2::Socket::from(stream);
            socket.set_linger(Some(std::time::Duration::from_secs(0)))?;
            bad_responses_so_far += 1;
            println!("RESET THE CONNECTION 👹👹👹\n");
        } else {
            // Write the second half of the ZIP bytes.
            stream.write_all(&zip_bytes[half..])?;
            println!("good response\n");
        }
    }
}
```

When I `dbg!` the error in that case it looks like this:

```
[crates/uv-python/src/downloads.rs:975:74] err = Io(
    Custom {
        kind: Other,
        error: TarError {
            desc: "failed to unpack `/home/jacko/.local/share/uv/python/.temp/.tmpTd0oSW/python/lib/libpython3.8.so.1.0`",
            io: Custom {
                kind: Other,
                error: TarError {
                    desc: "failed to unpack `python/lib/libpython3.8.so.1.0` into `/home/jacko/.local/share/uv/python/.temp/.tmpTd0oSW/python/lib/libpython3.8.so.1.0`",
                    io: Custom {
                        kind: Other,
                        error: reqwest::Error {
                            kind: Decode,
                            source: reqwest::Error {
                                kind: Body,
                                source: hyper::Error(
                                    Body,
                                    Os {
                                        code: 104,
                                        kind: ConnectionReset,
                                        message: "Connection reset by peer",
                                    },
                                ),
                            },
                        },
                    },
                },
            },
        },
    },
)
```

Repro-ing this bug seems very sensitive to exactly where the connection reset error bubbles up from.

---

_Comment by @oconnor663 on 2025-06-27 23:10_

Another possibility is that the client _is_ retrying in this case, and it's exhausting all of its retries. When I set that server above to always reset the connection, and then run `uv python install 3.8.20 --mirror http://localhost:8000 2>&1 | cat`, the result looks like this:

```
Downloading cpython-3.8.20-linux-x86_64-gnu (download)
Downloading cpython-3.8.20-linux-x86_64-gnu (download)
Downloading cpython-3.8.20-linux-x86_64-gnu (download)
Downloading cpython-3.8.20-linux-x86_64-gnu (download)
error: Failed to install cpython-3.8.20-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.8.20-20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/jacko/.local/share/uv/python/.temp/.tmpvnPRu8/python/lib/libpython3.8.so.1.0`
  Caused by: failed to unpack `python/lib/libpython3.8.so.1.0` into `/home/jacko/.local/share/uv/python/.temp/.tmpvnPRu8/python/lib/libpython3.8.so.1.0`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
```

The only difference between that and [the observed failure here](https://github.com/astral-sh/uv/actions/runs/15929622464/job/44935399219) is that I don't see multiple `Downloading...` messages. However, I don't see _any_ messages like that, and I might've expected to see one, so maybe they're getting filtered out?

---

_Comment by @zanieb on 2025-06-27 23:21_

I think `Downloading...` is a progress reporter message which might be turned off during tests, e.g., from `UV_TEST_NO_CLI_PROGRESS`? or something?

---

_Comment by @mjcarter on 2025-06-28 14:40_

I'm having this issue with a poor connection trying to downloading torch, if I use PIP (without UV) the --timeout parameter prevents the connection being lost if set with a high value, but it seems like the --timeout parameter isn't supported in UV?

---

_Comment by @zanieb on 2025-06-28 14:50_

@mjcarter can you open a new issue please? This one is about Python installation downloads.

---

_Comment by @oconnor663 on 2025-07-01 16:53_

https://github.com/astral-sh/uv/pull/14378 is merged, so the next time this flake happens we'll be looking to see whether it says it retried or not.

---

_Unassigned @oconnor663 by @oconnor663 on 2025-07-01 16:55_

---

_Comment by @zanieb on 2025-08-22 19:34_

Still seeing this

```
              5 │+error: Failed to install cpython-3.10.18-[PLATFORM]
              6 │+  Caused by: Failed to extract archive: cpython-3.10.18-[PLATFORM]-linux-gnu-install_only_stripped.tar.gz
              7 │+  Caused by: Invalid tar file
              8 │+  Caused by: failed to unpack `[TEMP_DIR]/managed/.temp/[TMP]/python3.10`
              9 │+  Caused by: failed to unpack `python/bin/python3.10` into `[TEMP_DIR]/managed/.temp/[TMP]/python3.10`
             10 │+  Caused by: error decoding response body
             11 │+  Caused by: request or response body error
             12 │+  Caused by: error reading a body from connection
             13 │+  Caused by: connection reset
```

---

_Closed by @charliermarsh on 2025-08-31 15:07_

---

_Comment by @zanieb on 2025-09-02 21:58_

I think we're still seeing this

```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: python_install_no_cache
    Source: V:\uv:2270
    ───────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ───────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬──────────────────────────────────────────────────────────────────
        0       │-success: true
        1       │-exit_code: 0
              0 │+success: false
              1 │+exit_code: 1
        2     2 │ ----- stdout -----
        3     3 │ 
        4     4 │ ----- stderr -----
        5       │-Installed Python 3.13.7 in [TIME]
        6       │- + cpython-3.13.7-[PLATFORM] (python3.13)
              5 │+error: Failed to install cpython-3.13.7-[PLATFORM]
              6 │+  Caused by: Failed to extract archive: cpython-3.13.7-[PLATFORM]-windows-msvc-install_only_stripped.tar.gz
              7 │+  Caused by: Invalid tar file
              8 │+  Caused by: failed to unpack `[TEMP_DIR]/managed/.temp/[TMP]/entities.cpython-313.pyc`
              9 │+  Caused by: failed to unpack `python/[PYTHON-LIB]/html/__pycache__/entities.cpython-313.pyc` into `[TEMP_DIR]/managed/.temp/[TMP]/entities.cpython-313.pyc`
             10 │+  Caused by: error decoding response body
             11 │+  Caused by: request or response body error
             12 │+  Caused by: error reading a body from connection
             13 │+  Caused by: connection reset
    ────────────┴──────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test python_install::python_install_no_cache ... FAILED

```

https://github.com/astral-sh/uv/actions/runs/17413253772/attempts/1?pr=15638

---

_Reopened by @zanieb on 2025-09-02 21:58_

---

_Comment by @zanieb on 2025-09-03 14:13_

And again

https://github.com/astral-sh/uv/actions/runs/17435853668/job/49505424731?pr=15663

---

_Comment by @konstin on 2025-09-03 14:18_

Puzzled why our logic isn't catching this, connection reset are a kind of IO error we catch and this error can only come from the `fetch` which is inside of our loop.

---

_Comment by @konstin on 2025-09-04 11:28_

It was h2 shadowing the `std::io::Error`.

---

_Closed by @konstin on 2025-09-04 12:45_

---
