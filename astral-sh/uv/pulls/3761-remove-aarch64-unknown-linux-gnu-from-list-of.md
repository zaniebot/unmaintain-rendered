```yaml
number: 3761
title: "Remove `aarch64-unknown-linux-gnu` from list of expected binaries"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - releases
assignees: []
merged: true
base: main
head: charlie/gnu
created_at: 2024-05-22T19:32:48Z
updated_at: 2024-05-23T07:15:58Z
url: https://github.com/astral-sh/uv/pull/3761
synced_at: 2026-01-10T14:32:20Z
```

# Remove `aarch64-unknown-linux-gnu` from list of expected binaries

---

_Pull request opened by @charliermarsh on 2024-05-22 19:32_

## Summary

We now only ship the static `aarch64-unknown-linux-musl` binary here.

Closes #3760.


---

_Label `bug` added by @charliermarsh on 2024-05-22 19:32_

---

_Label `release` added by @charliermarsh on 2024-05-22 19:32_

---

_@zanieb approved on 2024-05-22 19:34_

👀 👀 👀 👀 

---

_Comment by @charliermarsh on 2024-05-22 23:53_

Ok, confirmed that the installer does fall back:

```sh
    # Lookup what to download/unpack based on platform
    case "$_arch" in 
        "aarch64-apple-darwin")
            _artifact_name="uv-aarch64-apple-darwin.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            _bins_js_array='"uv"'
            _updater_name=""
            _updater_bin=""
            ;;
        "aarch64-unknown-linux-gnu")
            _artifact_name="uv-aarch64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            _bins_js_array='"uv"'
            _updater_name=""
            _updater_bin=""
            ;;
        "aarch64-unknown-linux-musl-dynamic")
            _artifact_name="uv-aarch64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            _bins_js_array='"uv"'
            _updater_name=""
            _updater_bin=""
            ;;
        "aarch64-unknown-linux-musl-static")
            _artifact_name="uv-aarch64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            _bins_js_array='"uv"'
            _updater_name=""
            _updater_bin=""
            ;;
```

By running `cargo dist build -aglobal`.

---

_Merged by @charliermarsh on 2024-05-22 23:53_

---

_Closed by @charliermarsh on 2024-05-22 23:53_

---

_Branch deleted on 2024-05-22 23:53_

---

_Comment by @konstin on 2024-05-23 07:15_

Thanks for checking & fixing!

---
