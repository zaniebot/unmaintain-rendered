```yaml
number: 721
title: Add support for relative URLs in simple metadata responses
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/base-url
created_at: 2023-12-24T20:44:59Z
updated_at: 2023-12-27T13:53:22Z
url: https://github.com/astral-sh/uv/pull/721
synced_at: 2026-01-12T16:04:09Z
```

# Add support for relative URLs in simple metadata responses

---

_@charliermarsh_

## Summary

This PR adds support for relative URLs in the simple JSON responses. We already support relative URLs for HTML responses, but the handling has been consolidated between the two. Similar to index URLs, we now store the base alongside the metadata, and use the base when resolving the URL.

Closes #455.

## Test Plan

`cargo test` (to test HTML indexes). Separately, I also ran `cargo run -p puffin-cli -- pip-compile requirements.in -n --index-url=http://localhost:3141/packages/pypi/+simple` on the `zb/relative` branch with `packse` running, and forced both HTML and JSON by limiting the `accept` header.


---

_Label `enhancement` added by @charliermarsh on 2023-12-24 20:45_

---

_Comment by @charliermarsh on 2023-12-24 20:45_

@zanieb - Any instructions for testing on `devpi`?

---

_Comment by @zanieb on 2023-12-24 20:53_

You could use https://github.com/zanieb/packse/compare/zb/relative

Then `packse index up` and `http://localhost:3141/packages/pypi/+simple` as the index URL.

I _presume_ this will use relative URLs everywhere but I'm not sure.

---

_Comment by @charliermarsh on 2023-12-24 20:55_

I shall try, thank you!

---

_Comment by @charliermarsh on 2023-12-24 21:39_

Worked great, thanks. Interestingly, if I send up `application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html`, `devpi` returns `HTML` by default. I wonder if we should "prefer" JSON?

---

_Comment by @charliermarsh on 2023-12-24 21:41_

Ah nevermind, I'm missing a "quality" parameter to indicate that we prefer JSON. Easy fix.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-24 21:43_

---

_Review requested from @konstin by @charliermarsh on 2023-12-26 13:58_

---

_@konstin approved on 2023-12-26 14:56_

---

_Merged by @charliermarsh on 2023-12-27 13:53_

---

_Closed by @charliermarsh on 2023-12-27 13:53_

---

_Branch deleted on 2023-12-27 13:53_

---
