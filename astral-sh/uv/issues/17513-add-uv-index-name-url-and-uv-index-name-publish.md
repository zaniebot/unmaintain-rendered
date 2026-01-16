```yaml
number: 17513
title: "Add UV_INDEX_{name}_URL and UV_INDEX_{name}_PUBLISH_URL ?"
type: issue
state: open
author: mil-ad
labels:
  - question
assignees: []
created_at: 2026-01-16T09:46:38Z
updated_at: 2026-01-16T21:26:36Z
url: https://github.com/astral-sh/uv/issues/17513
synced_at: 2026-01-16T22:15:06Z
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

_Comment by @woodruffw on 2026-01-16 21:26_

I think that kind of dynamism in environment variables might be a little tricky; uv _generally_ tries to only use environment variables that it knows AOT. There's also an encoding/ambiguity problem, e.g. `name = "foo-bar"` would need to be transformed into something like `UV_INDEX_FOO_BAR_...` but that would then overlap with `name = "foo_bar"`.

Does `UV_PUBLISH_INDEX=blahblah` work for your use case? That'll ensure that the matching index configuration's `url` and `publish-url` are selected as needed 🙂 

---

_Label `question` added by @woodruffw on 2026-01-16 21:26_

---

_Label `enhancement` removed by @woodruffw on 2026-01-16 21:26_

---
