---
number: 16410
title: pip download capabilities
type: issue
state: closed
author: lemming622
labels:
  - enhancement
assignees: []
created_at: 2025-10-22T14:50:49Z
updated_at: 2025-10-22T16:30:29Z
url: https://github.com/astral-sh/uv/issues/16410
synced_at: 2026-01-10T01:26:05Z
---

# pip download capabilities

---

_Issue opened by @lemming622 on 2025-10-22 14:50_

### Summary

I'm in an air-gapped network and currently spending 10+ hours running `pip download` for the multiple python versions and platforms my users are using.  Can uv add the same functionality that `pip download` does to help speed this up?

### Example

Current process trying to replicate in uv:
```shell
process_package() {
    platforms=("win32" "win_amd64" "manylinux_2_28_x86_64" "manylinux_2_12_x86_64" "manylinux_2_17")
    versions=("309" "310" "311" "312" "313")
    for platform in "${platforms[@]}"; do
        for version in "${versions[@]}"; do
            pip download --only-binary :all: \
                         --implementation cp \
                         --dest wheels \
                         --platform $platform \
                         --python-version $version \
                         $1
        done
    done
}
```
Where `$1` is a single package.  Trying to speed up a known list of 300+ packages with an unknown count of dependencies.

---

_Label `enhancement` added by @lemming622 on 2025-10-22 14:50_

---

_Comment by @my1e5 on 2025-10-22 14:55_

Duplicate of 
* https://github.com/astral-sh/uv/issues/3163

---

_Closed by @zanieb on 2025-10-22 16:30_

---
