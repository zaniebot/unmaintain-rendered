```yaml
number: 17513
title: "Add UV_INDEX_{name}_URL and UV_INDEX_{name}_PUBLISH_URL ?"
type: issue
state: open
author: mil-ad
labels:
  - enhancement
assignees: []
created_at: 2026-01-16T09:46:38Z
updated_at: 2026-01-16T09:48:08Z
url: https://github.com/astral-sh/uv/issues/17513
synced_at: 2026-01-16T09:55:15Z
```

# Add UV_INDEX_{name}_URL and UV_INDEX_{name}_PUBLISH_URL ?

---

_@mil-ad_

### Summary

I have a private index for some packages:

```
[[tool.uv.index]]
name = "blahblah"
url = "https://oauth2accesstoken@us-central1-python.pkg.dev/foo/bar/simple/"
publish-url = "https://oauth2accesstoken@us-central1-python.pkg.dev/foo/"
explicit = true
```

is there a way to add this via environment variables?

I guess `oauth2accesstoken` would go into `UV_INDEX_blahblah_USERNAME` but there are no `UV_INDEX_{name}_URL` or `UV_INDEX_{name}_PUBLISH_URL`

### Example

_No response_

---

_Label `enhancement` added by @mil-ad on 2026-01-16 09:46_

---
