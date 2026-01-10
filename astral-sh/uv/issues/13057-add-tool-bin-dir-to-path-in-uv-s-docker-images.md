```yaml
number: 13057
title: "Add tool bin dir to PATH in uv's docker images"
type: issue
state: closed
author: RazerM
labels:
  - enhancement
assignees: []
created_at: 2025-04-22T17:28:13Z
updated_at: 2025-06-23T22:03:11Z
url: https://github.com/astral-sh/uv/issues/13057
synced_at: 2026-01-10T03:32:45Z
```

# Add tool bin dir to PATH in uv's docker images

---

_Issue opened by @RazerM on 2025-04-22 17:28_

### Summary

If using a uv docker image such as `ghcr.io/astral-sh/uv:python3.13-bookworm`, installed tools are not on the `PATH`, which means tools cannot be used out of the box. E.g. in a GitLab CI job:

```yaml
job:
  stage: deploy
  image: ghcr.io/astral-sh/uv:python3.13-bookworm
  before_script:
    - uv tool install ...
    - <try to use tool>
```

```
warning: `/root/.local/bin` is not on your PATH. To use installed tools, add the directory to your PATH.
```

It would be nice if the docker images included `ENV PATH=/root/.local/bin:$PATH` so that this worked.

### Example

_No response_

---

_Label `enhancement` added by @RazerM on 2025-04-22 17:28_

---

_Assigned to @zanieb by @zanieb on 2025-04-25 02:05_

---

_Comment by @samypr100 on 2025-04-26 02:28_

@RazerM this is generally a nice shortcut, although it comes with risks given this is a breaking change which may effectively break downstream consumers.

Few immediate examples of this:

1. There is no easy way to opt-out if this behavior on builds that inherit from this image besides re-overriding PATH at build time or patching it at runtime each time.
2. Putting root in the path can be problematic for anyone that inherits from this image and uses a different user, likely causing permission issues. Using a different user will change what the tool path should be ultimately be. This is something that should be determined by the consumer not producer in our case.
3. You'd introduce different behaviors across images as  /root is not portable. There may be no equivalent across all images. Since Docker does not expand env vars dynamically from a runtime shell-specific output, you'd still have a build limitation on achieving the proposed behavior.

On your example, I would instead prefix PATH which what you need to accomplish the desired behavior.

```yaml
before_script:
    - uv tool install ...
    - PATH=/root/.local/bin:$PATH  <try to use tool>
```

edit: changed `permanently` to `effectively` to clarify intent and fixed wording on example 3 which was confusing.

---

_Comment by @RazerM on 2025-04-26 21:28_

@samypr100 

> this is a breaking change which may permanently break downstream consumers.

I think that may be a bit dramatic 😅

To answer your examples point by point:

1. I'm not sure why anyone would need an opt-out here. A user of the image is in control of what gets installed. Anyone running the image as root can do whatever they want, and anyone running as non-root isn't affected by it.
2. Inaccessible directories on PATH are ignored, they wouldn't cause "permission issues".

   If I build on top of these images and use a different user, it's also on me to set up `PATH` if I want `uv tool install` to work for that user.
3. Obviously the correct directory would be used for each image that is built...
4. I'm not sure what you're trying to say here. `ENV` does expand `$PATH` at build time. You can have a look at `docker image inspect` on built images if you want to check this.

> On your example, I would instead prefix PATH which what you need to accomplish the desired behavior.

I'm using `UV_TOOL_BIN_DIR`, since I can set it globally:

```yaml
variables:
  UV_TOOL_BIN_DIR: /usr/local/bin

job:
  stage: deploy
  image: ghcr.io/astral-sh/uv:python3.13-bookworm
  before_script:
    - uv tool install ...
    - <try to use tool>
```

I just thought it would be nice if `uv tool install` worked out of the box...

---

_Comment by @samypr100 on 2025-04-26 21:59_

> I'm using UV_TOOL_BIN_DIR, since I can set it globally

Indeed, that's a good approach and something that seems quite reasonable to do on uv images by default over modifying PATH.

---

_Closed by @RazerM on 2025-06-23 22:03_

---
