```yaml
number: 11943
title: Documentation needed for UV_GIT_LFS in dependency management and clear error message for using Git LFS
type: issue
state: open
author: kubotty
labels:
  - enhancement
assignees: []
created_at: 2025-03-04T09:19:25Z
updated_at: 2025-04-29T12:31:52Z
url: https://github.com/astral-sh/uv/issues/11943
synced_at: 2026-01-12T16:00:50Z
```

# Documentation needed for UV_GIT_LFS in dependency management and clear error message for using Git LFS

---

_@kubotty_

### Summary

Hi,

There is no documentation for environment variable `UV_GIT_LFS` on the *Managing dependencies* page, which made it difficult to resolve errors when adding repositories that apply Git LFS to dependencies. It would be helpful if you could add an explanation of this variable to the *Managing dependencies* page.

Additionally, the error messages when using this feature without configuring `UV_GIT_LFS` were unclear. Providing clear error messages would facilitate smoother problem-solving.

Furthermore, it would be great if this could be configured via command-line options or in the `pyproject.toml`. I would also like to request that you consider making the default setting `True`, similar to `pip`.

Thank you for your consideration!

### Example

_No response_

---

_Label `enhancement` added by @kubotty on 2025-03-04 09:19_

---

_Comment by @alexlorenzo on 2025-04-29 12:31_

You can try this, for my side it's working:
`UV_GIT_LFS=1 uv add git+ssh://git@github.com/...`

Verbose information : 
`UV_GIT_LFS=1 uv add -vv  git+ssh://git@github.com/...`



---
