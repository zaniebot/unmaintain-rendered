```yaml
number: 1978
title: "fix 'uv pip install' handling of gzip'd response and PEP 691"
type: pull_request
state: merged
author: thundergolfer
labels:
  - performance
  - great writeup
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-26T03:48:06Z
updated_at: 2024-02-26T19:48:48Z
url: https://github.com/astral-sh/uv/pull/1978
synced_at: 2026-01-12T16:04:49Z
```

# fix 'uv pip install' handling of gzip'd response and PEP 691

---

_@thundergolfer_

Thank you for writing `uv`! We're already using it internally on some container image builds and finding that it's noticeably faster 💯 

## Summary

I was attempting to use `uv` alongside [modal](https://modal.com/)'s internal PyPi mirror and ran into some issues. The first issue was the following error:

```
error: Failed to download: nltk==3.8.1
  Caused by: content-length header is missing from response
```

This error was coming from within `RegistryClient::wheel_metadata_no_pep658`. By logging requests on the client (uv) and server (internal mirror) sides I've concluded that it's occurring because `uv` is sending a header suggesting that it can accept a gzip'd response, but decompressing the gzip'd response strips the `content-length` header: https://github.com/seanmonstar/reqwest/issues/294. 

**Logged request, client-side:**

```
0.981664s   0ms  INFO uv_client::registry_client JONO, REQ: Request { method: HEAD, url: Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Ipv4(172.21.0.1)), port: Some(5555), path: "/simple/joblib/joblib-1.3.2-py3-none-any.whl", query: None, fragment: None }, headers: {} }
```

No headers set explicitly by `uv`.

**Logged request, server-side:**

```
2024-02-26T03:45:08.598272Z DEBUG pypi_mirror: origin request = Request { method: HEAD, uri: /simple/joblib/joblib-1.3.2-py3-none-any.whl, version: HTTP/1.1, headers: {"accept": "*/*", "user-agent": "uv", "accept-encoding": "gzip, br", "host": "172.21.0.1:5555"}, body: Body(Empty) }
```

Server receives `"accept-encoding": "gzip, br",`. 

My change adding the header to the request fixed this issue. But our internal mirror is just passing through PyPI responses and PyPI responses do contain PEP 658 data, and so `wheel_metadata_no_pep658` shouldn't execute. 

The issue there is that the PyPi response field has _dashes_ not _underscores_ (https://peps.python.org/pep-0691/). 

<img width="1261" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/35230f27-441a-457a-827b-870a1a16c16a">

After changing the `alias` the PEP 658 codepath now runs correctly :)

## Test Plan

I tested by installing against both our mirror and against PyPi: 

```
RUST_LOG="uv=trace" UV_NO_CACHE=true UV_INDEX_URL="http://172.21.0.1:5555/simple" target/release/uv pip install -v nltk
RUST_LOG="uv=trace" UV_NO_CACHE=true UV_INDEX_URL="http://172.21.0.1:5555/simple" target/release/uv pip uninstall -v nltk
```

```
target/release/uv pip install -v nltk
target/release/uv pip uninstall -v nltk
```


---

_@charliermarsh reviewed on 2024-02-26 04:14_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:464 on 2024-02-26 04:14_

Iiinteresting, so when the server returned a gzipped response here, we effectively lost the headers that are required for the range request?

---

_@charliermarsh reviewed on 2024-02-26 04:19_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:41 on 2024-02-26 04:19_

Wow that's a huge oversight, thank you! This could make a big difference in cold resolution time actually.

---

_@charliermarsh reviewed on 2024-02-26 04:20_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:464 on 2024-02-26 04:20_

I wonder if this is related: https://github.com/astral-sh/uv/issues/1754.

---

_@charliermarsh reviewed on 2024-02-26 04:21_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:464 on 2024-02-26 04:21_

Eh, that issue suggests that `Content-Type` is preserved, nevermind.

---

_Comment by @charliermarsh on 2024-02-26 04:22_

I'm gonna benchmark this real quick.

---

_@zanieb reviewed on 2024-02-26 04:26_

---

_Review comment by @zanieb on `crates/pypi-types/src/simple_json.rs`:41 on 2024-02-26 04:26_

👀 

---

_Comment by @charliermarsh on 2024-02-26 04:28_

The bad news is that we missed that typo and it's embarrassing. The good news is that it makes cold resolution like 30-40% faster for free:

```
❯ python -m scripts.bench \
        --uv-path ./target/release/uv \
        --uv-path ./target/release/main \
        ./scripts/requirements/jupyter.in --benchmark resolve-cold --min-runs 20
Benchmark 1: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     627.2 ms ±  33.8 ms    [User: 215.1 ms, System: 97.3 ms]
  Range (min … max):   567.0 ms … 719.3 ms    20 runs

Benchmark 2: ./target/release/main (resolve-cold)
  Time (mean ± σ):     838.0 ms ±  38.9 ms    [User: 228.0 ms, System: 110.9 ms]
  Range (min … max):   775.9 ms … 897.9 ms    20 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.34 ± 0.10 times faster than './target/release/main (resolve-cold)'
```

---

_Merged by @charliermarsh on 2024-02-26 04:28_

---

_Closed by @charliermarsh on 2024-02-26 04:28_

---

_Comment by @charliermarsh on 2024-02-26 04:28_

Thanks for the great find! I'll release this in the morning.

---

_Label `performance` added by @charliermarsh on 2024-02-26 04:30_

---

_Label `great writeup` added by @charliermarsh on 2024-02-26 04:30_

---

_Comment by @zanieb on 2024-02-26 04:33_

Thank you! This is great work

---

_Review comment by @thundergolfer on `crates/uv-client/src/registry_client.rs`:464 on 2024-02-26 15:58_

Yep they get stripped because they're not accurate after decompression https://github.com/seanmonstar/reqwest/issues/294

---

_@thundergolfer reviewed on 2024-02-26 15:58_

---
