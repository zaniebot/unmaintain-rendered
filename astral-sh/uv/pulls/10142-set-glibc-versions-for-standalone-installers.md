```yaml
number: 10142
title: Set glibc versions for standalone installers
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/glibc
created_at: 2024-12-24T13:16:20Z
updated_at: 2024-12-24T13:35:30Z
url: https://github.com/astral-sh/uv/pull/10142
synced_at: 2026-01-12T16:09:09Z
```

# Set glibc versions for standalone installers

---

_@charliermarsh_

## Summary

Per Discord, it sounds like `cargo-dist` will assume that 2.31 is our minimum glibc version, since we're building our own binaries. (You can confirm this by looking at [uv-installer.sh](https://github.com/astral-sh/uv/releases/download/0.5.11/uv-installer.sh).)

`cargo-dist` now supports specifying a glibc override for each target: https://opensource.axo.dev/cargo-dist/book/reference/config.html#min-glibc-version. This is great, since we use 2.17 everywhere, but 2.28 for ARM.


---

_Label `releases` added by @charliermarsh on 2024-12-24 13:16_

---

_Comment by @charliermarsh on 2024-12-24 13:35_

Ran `dist build -a global --tag=v0.5.11` locally, and verified that `target/distrib/uv-installer.sh` contains the right versions, e.g.:

```
"armv7-unknown-linux-gnueabihf")
    _archive="uv-armv7-unknown-linux-gnueabihf.tar.gz"
    if ! check_glibc "2" "17"; then
        _archive=""
    fi
    if [ -n "$_archive" ]; then
        echo "$_archive"
        return 0
    fi
    _archive="uv-armv7-unknown-linux-musleabihf.tar.gz"
    if [ -n "$_archive" ]; then
        echo "$_archive"
        return 0
    fi
    ;;
...
"aarch64-unknown-linux-gnu")
    _archive="uv-aarch64-unknown-linux-gnu.tar.gz"
    if ! check_glibc "2" "28"; then
        _archive=""
    fi
    if [ -n "$_archive" ]; then
        echo "$_archive"
        return 0
    fi
    _archive="uv-aarch64-unknown-linux-musl.tar.gz"
    if [ -n "$_archive" ]; then
        echo "$_archive"
        return 0
    fi
    ;;
```

---

_Merged by @charliermarsh on 2024-12-24 13:35_

---

_Closed by @charliermarsh on 2024-12-24 13:35_

---

_Branch deleted on 2024-12-24 13:35_

---
