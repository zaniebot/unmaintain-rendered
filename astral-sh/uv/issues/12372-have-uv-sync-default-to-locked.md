```yaml
number: 12372
title: "Have `uv sync` default to `--locked`"
type: issue
state: open
author: johnthagen
labels:
  - enhancement
assignees: []
created_at: 2025-03-21T17:08:54Z
updated_at: 2025-03-21T17:13:49Z
url: https://github.com/astral-sh/uv/issues/12372
synced_at: 2026-01-12T16:01:01Z
```

# Have `uv sync` default to `--locked`

---

_@johnthagen_

### Summary

I was a little surprised/confused when migrating to `uv` that the default `uv sync` did not enforce that the project was synced exactly to `uv.lock` or else error if not.

When reading through the docs, I found some references to `--frozen`

- https://docs.astral.sh/uv/guides/integration/docker/#installing-a-project

And then other references that used CI that didn't include it:

- https://docs.astral.sh/uv/guides/integration/github/#syncing-and-running

I saw `--locked` and it wasn't immediately obvious the distinction between it and the closely related `--frozen`. Part of this stemmed from the expectation that `uv sync` would only _sync_ and not change the project environment at all.

I think I can see where the convenience motivation for `uv sync` working as it does could come from, but in my opinion, it would be a much safer default that `uv sync` tries to only install the environment as currently specifies, or errors if something is not correct. Whether it's a new developer, a Dockerfile, a CI configuration, I think the most desirable thing is to fail fast and give a useful error message rather than silently move on if the lock file is not up-to-date. Again, this is just for the `sync` command.

The alternative is that if developers want to be safe about this, they need to duplicate `--locked` everywhere in their READMEs, Dockerfiles, CI configurations etc to be sure that something unexpected doesn't silently happen when the lock file is out of date.

---

_Label `enhancement` added by @johnthagen on 2025-03-21 17:08_

---

_Comment by @johnthagen on 2025-03-21 17:13_

@zanieb Explained the current position well in

- https://github.com/astral-sh/uv/issues/10793#issuecomment-2743956736

I'm happy to close this issue if that's the direction `uv` is going.

---
