```yaml
number: 16359
title: Show build output when building (local?) packages
type: issue
state: open
author: davidhewitt
labels:
  - enhancement
assignees: []
created_at: 2025-10-19T12:10:26Z
updated_at: 2025-10-19T12:10:26Z
url: https://github.com/astral-sh/uv/issues/16359
synced_at: 2026-01-12T16:02:29Z
```

# Show build output when building (local?) packages

---

_@davidhewitt_

### Summary

(I'm experimenting with having `pydantic-core` as an in-tree subpackage of `pydantic`, but this could equally apply for any native package being built with `uv`.)

At the moment the build output when compiling a package doesn't show any current progress:
```
$ uv sync --group testing-extra
Resolved 144 packages in 21ms
   Building pydantic-core @ file:///Users/david/Dev/pydantic/pydantic/pydantic-core
⠼ Preparing packages... (0/1)              
```

... this step can take quite a while for complex Rust release builds.

It would be a _really nice_ improvement (in my opinion) if the build output is visible. I suspect having something like `docker build`'s scrolling output would be a good way to do this. Would you be open to supporting this?

### Example

See e.g. the docker build output ([sourced from this stack overflow question](https://stackoverflow.com/questions/75763024/how-to-see-all-output-when-executing-docker-build)):

![](https://i.sstatic.net/s65cL.png)

---

_Label `enhancement` added by @davidhewitt on 2025-10-19 12:10_

---
