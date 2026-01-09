---
number: 13608
title: Recognize an S3 store as valid for packages
type: issue
state: open
author: phreed
labels:
  - enhancement
assignees: []
created_at: 2025-05-22T23:49:32Z
updated_at: 2025-05-22T23:56:38Z
url: https://github.com/astral-sh/uv/issues/13608
synced_at: 2026-01-07T13:12:18-06:00
---

# Recognize an S3 store as valid for packages

---

_Issue opened by @phreed on 2025-05-22 23:49_

### Summary

There are techniques for making an S3 store act as a pypi repository:
* https://github.com/devpi/devpi
* https://github.com/stevearc/pypicloud
The `pixi` project can retrieve packages from S3 `https://pixi.sh/latest/deployment/s3/`.
`pixi` uses `uv` for retrieving pypi packages.

Discussions related to providing S3 access in Rust:
* https://github.com/prefix-dev/pixi/blob/v0.47.0/crates/pixi_manifest/src/s3.rs
* https://github.com/conda/rattler/issues/960
* https://crates.io/crates/aws-sdk-s3 (instead of https://github.com/durch/rust-s3)
* https://github.com/minio/minio-rs



### Example

When doing deployments in a off-line environment a standalone S3 server (e.g., MinIO) would serve as a repository without adding an additional server to translate into pypi form.

AWS may be more scalable than `pypi.org`.



---

_Label `enhancement` added by @phreed on 2025-05-22 23:49_

---

_Comment by @phreed on 2025-05-22 23:54_

This seems to be the primary change for rattler:  https://github.com/conda/rattler/pull/1008/files

---
