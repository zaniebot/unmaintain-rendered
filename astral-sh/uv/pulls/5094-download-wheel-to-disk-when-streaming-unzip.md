```yaml
number: 5094
title: Download wheel to disk when streaming unzip failed with HTTP streaming error
type: pull_request
state: merged
author: messense
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: retry-http-streaming-error
created_at: 2024-07-16T03:11:00Z
updated_at: 2024-07-16T13:03:13Z
url: https://github.com/astral-sh/uv/pull/5094
synced_at: 2026-01-12T16:06:38Z
```

# Download wheel to disk when streaming unzip failed with HTTP streaming error

---

_@messense_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Workaround the `stream_wheel` not retry issue [found](https://github.com/astral-sh/uv/issues/3514#issuecomment-2229820667) in #3514, it's not a perfect solution but I think it's acceptable because the error should not occur frequently.

## Test Plan

Manually using `iptables -A OUTPUT -p tcp -dport 3128 -j REJECT --reject-with tcp-reset` to inject connection reset error to the HTTP proxy that proxies PyPI requests.

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: piqp==0.4.1
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://files.pythonhosted.org/packages/94/4d/09ade94dfda5b57c1ca43564541871bd1a0d89dfd3c368ac505b6ca09831/piqp-0.4.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
  Caused by: client error (Connect)
  Caused by: tcp connect error: Connection refused (os error 111)
  Caused by: Connection refused (os error 111)
```


---

_@messense reviewed on 2024-07-16 03:14_

---

_Review comment by @messense on `crates/uv-extract/src/error.rs`:36 on 2024-07-16 03:14_

The `Io` error case is for https://github.com/astral-sh/uv/blob/545bbf92863f4109070ebeb922c837b430441b76/crates/uv-extract/src/stream.rs#L53

> Io(Custom { kind: Other, error: reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Os { code: 104, kind: ConnectionReset, message: "Connection reset by peer" }) } } })

we can check for a `reqwest::Error` inside the io error to avoid retry on writer side error if deemed useful.

Edit: done in d808e1c

---

_@konstin approved on 2024-07-16 09:03_

---

_Review requested from @charliermarsh by @konstin on 2024-07-16 09:03_

---

_Review requested from @zanieb by @konstin on 2024-07-16 09:03_

---

_Label `bug` added by @konstin on 2024-07-16 09:03_

---

_Label `network` added by @konstin on 2024-07-16 09:03_

---

_@charliermarsh approved on 2024-07-16 13:00_

---

_Merged by @charliermarsh on 2024-07-16 13:00_

---

_Closed by @charliermarsh on 2024-07-16 13:00_

---

_Branch deleted on 2024-07-16 13:03_

---
