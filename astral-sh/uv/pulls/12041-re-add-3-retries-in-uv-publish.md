```yaml
number: 12041
title: "Re-add 3 retries in `uv publish`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-upload-retry-policy
created_at: 2025-03-07T12:56:03Z
updated_at: 2025-03-10T11:38:10Z
url: https://github.com/astral-sh/uv/pull/12041
synced_at: 2026-01-12T16:10:06Z
```

# Re-add 3 retries in `uv publish`

---

_@konstin_

In the publish client, we have to set the client retries to 0 as the retry middleware is incompatible with upload bodies. This however also sets `client.retry_policy()` to a zero-retry policy, so we need to construct our own policy.

Fixes #12027

---

_Label `bug` added by @konstin on 2025-03-07 12:56_

---

_Review requested from @charliermarsh by @konstin on 2025-03-07 12:56_

---

_Review requested from @zanieb by @konstin on 2025-03-07 12:56_

---

_@zanieb reviewed on 2025-03-07 14:03_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:385 on 2025-03-07 14:03_

Could you add a comment here, like

```suggestion
    // N.B. We cannot use the client policy here because it is set to zero retries
    let retry_policy = ExponentialBackoff::builder().build_with_max_retries(DEFAULT_RETRIES);
```

I guess alternatively we could add another method to the client like `retry_policy_builder`?

---

_@zanieb approved on 2025-03-07 14:04_

---

_Comment by @charliermarsh on 2025-03-07 14:54_

Can you add a test plan that we can use to reproduce and test in the future?

---

_Comment by @charliermarsh on 2025-03-07 14:54_

Even if it’s not an automated test, it’d be great to be able to look back here and know how to test this.

---

_Comment by @konstin on 2025-03-07 16:17_

I've used this script:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "flask>=3.1.0,<4",
# ]
# ///

from argparse import ArgumentParser
from flask import Flask, app

app = Flask(__name__)


@app.route("/", defaults={"path": ""}, methods=["GET", "POST"])
@app.route("/<path:path>", methods=["GET", "POST"])
def always_error(path):
    return "Internal Server Error", 500


def main():
    app.run(debug=True, host="0.0.0.0", port=5000)


if __name__ == "__main__":
    main()
```

With released uv:

```
$ uv publish -v --publish-url http://127.0.0.1:5000 -u ferris -p f3rr1s scripts/packages/built-by-uv/dist/*
DEBUG uv 0.6.5
Publishing 2 files http://127.0.0.1:5000/
DEBUG Using request timeout of 900s
Uploading built_by_uv-0.1.0-py3-none-any.whl (10.3KiB)
DEBUG Hashing scripts/packages/built-by-uv/dist/built_by_uv-0.1.0-py3-none-any.whl
DEBUG Using username/password basic auth
DEBUG Transient request failure for: http://127.0.0.1:5000/
DEBUG Response code for http://127.0.0.1:5000/: 500 Internal Server Error
DEBUG Upload error response: Internal Server Error
error: Failed to publish `scripts/packages/built-by-uv/dist/built_by_uv-0.1.0-py3-none-any.whl` to http://127.0.0.1:5000/
  Caused by: Upload failed with status code 500 Internal Server Error. Server says: Internal Server Error
```

With this branch:

```
$ uv-debug publish -v --publish-url http://127.0.0.1:5000 -u ferris -p f3rr1s scripts/packages/built-by-uv/dist/*
DEBUG uv 0.6.5+7 (7d065acc2 2025-03-07)
Publishing 2 files http://127.0.0.1:5000/
DEBUG Using request timeout of 900s
Uploading built_by_uv-0.1.0-py3-none-any.whl (10.3KiB)
DEBUG Hashing scripts/packages/built-by-uv/dist/built_by_uv-0.1.0-py3-none-any.whl
DEBUG Using username/password basic auth
DEBUG Transient request failure for: http://127.0.0.1:5000/
warning: Transient failure while handling response for http://127.0.0.1:5000/; retrying...
DEBUG Using username/password basic auth
DEBUG Transient request failure for: http://127.0.0.1:5000/
warning: Transient failure while handling response for http://127.0.0.1:5000/; retrying...
DEBUG Using username/password basic auth
DEBUG Transient request failure for: http://127.0.0.1:5000/
warning: Transient failure while handling response for http://127.0.0.1:5000/; retrying...
DEBUG Using username/password basic auth
DEBUG Transient request failure for: http://127.0.0.1:5000/
DEBUG Response code for http://127.0.0.1:5000/: 500 Internal Server Error
DEBUG Upload error response: Internal Server Error
error: Failed to publish `scripts/packages/built-by-uv/dist/built_by_uv-0.1.0-py3-none-any.whl` to http://127.0.0.1:5000/
  Caused by: Upload failed with status code 500 Internal Server Error. Server says: Internal Server Error
```

---

_Comment by @konstin on 2025-03-10 11:38_

Going ahead with merging this, if we need a better integration test I can follow up

---

_Merged by @konstin on 2025-03-10 11:38_

---

_Closed by @konstin on 2025-03-10 11:38_

---

_Branch deleted on 2025-03-10 11:38_

---
