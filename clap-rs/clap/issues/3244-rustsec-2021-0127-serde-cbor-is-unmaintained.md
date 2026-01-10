---
number: 3244
title: "RUSTSEC-2021-0127: serde_cbor is unmaintained"
type: issue
state: closed
author: github-actions
labels: []
assignees: []
created_at: 2022-01-03T03:08:22Z
updated_at: 2022-01-04T01:34:22Z
url: https://github.com/clap-rs/clap/issues/3244
synced_at: 2026-01-10T01:27:36Z
---

# RUSTSEC-2021-0127: serde_cbor is unmaintained

---

_Issue opened by @github-actions on 2022-01-03 03:08_


> serde_cbor is unmaintained

| Details             |                                                |
| ------------------- | ---------------------------------------------- |
| Status              | unmaintained                |
| Package             | `serde_cbor`                      |
| Version             | `0.11.2`                   |
| URL                 | [https://github.com/pyfisch/cbor](https://github.com/pyfisch/cbor) |
| Date                | 2021-08-15                         |

The `serde_cbor` crate is unmaintained. The author has archived the github repository.

Alternatives proposed by the author:

 * [`ciborium`](https://crates.io/crates/ciborium)
 * [`minicbor`](https://crates.io/crates/minicbor)

See [advisory page](https://rustsec.org/advisories/RUSTSEC-2021-0127.html) for additional details.


---

_Comment by @epage on 2022-01-04 01:34_

This is because of criterion which is a dev-dependency.

---

_Closed by @epage on 2022-01-04 01:34_

---
