---
number: 707
title: "Panic when `requires-python` is missing from JSON responses"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2023-12-19T18:52:37Z
updated_at: 2023-12-20T10:53:50Z
url: https://github.com/astral-sh/uv/issues/707
synced_at: 2026-01-10T01:23:04Z
---

# Panic when `requires-python` is missing from JSON responses

---

_Issue opened by @zanieb on 2023-12-19 18:52_

```
error: Received some unexpected JSON from http://localhost:3141/root/pypi/+simple/markupsafe/?format=application/vnd.pypi.simple.v1+json
  Caused by: missing field `requires-python` at line 1 column 271
```

Some versions include it while others do not, see this truncated response

```
curl -H 'Accept: application/vnd.pypi.simple.v1+json' 'http://localhost:3141/root/pypi/+simple/markupsafe/?format=application/vnd.pypi.simple.v1+json' | jq
```

```
{
  "meta": {
    "api-version": "1.0"
  },
  "name": "markupsafe",
  "files": [
    {
      "filename": "MarkupSafe-0.9.tar.gz",
      "url": "http://localhost:3141/root/pypi/+f/9d8/4526bcfb6435d/MarkupSafe-0.9.tar.gz",
      "hashes": {
        "sha256": "9d84526bcfb6435d30cdb3531e3c43170d77107531a6d695c12a9c126ac09766"
      }
    },
    {
      "filename": "MarkupSafe-1.1.0-cp27-cp27m-macosx_10_6_intel.whl",
      "url": "http://localhost:3141/root/pypi/+f/efd/c45ef1afc238d/MarkupSafe-1.1.0-cp27-cp27m-macosx_10_6_intel.whl",
      "hashes": {
        "sha256": "efdc45ef1afc238db84cb4963aa689c0408912a0239b0721cb172b4016eb31d6"
      },
      "requires-python": ">=2.7,!=3.0.*,!=3.1.*,!=3.2.*,!=3.3.*"
    }
  ]
}
```

---

_Comment by @zanieb on 2023-12-19 18:55_

Seems like `serde` should not be doing this by default https://github.com/serde-rs/serde/issues/2214

Perhaps it's our custom deserializer

https://github.com/astral-sh/puffin/blob/12eedb1c12acd3e1ea51cd71aaa7cca12b1d70ac/crates/pypi-types/src/simple_json.rs#L26

---

_Comment by @zanieb on 2023-12-19 18:58_

Can confirm this works with our custom deserializer disabled

---

_Referenced in [astral-sh/uv#708](../../astral-sh/uv/pulls/708.md) on 2023-12-19 19:07_

---

_Label `bug` added by @charliermarsh on 2023-12-19 20:57_

---

_Closed by @konstin on 2023-12-20 10:53_

---
