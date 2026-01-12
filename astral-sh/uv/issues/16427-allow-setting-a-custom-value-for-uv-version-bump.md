```yaml
number: 16427
title: "Allow setting a custom value for `uv version --bump`"
type: issue
state: closed
author: jvacek
labels:
  - enhancement
assignees: []
created_at: 2025-10-23T19:07:57Z
updated_at: 2025-11-11T17:27:33Z
url: https://github.com/astral-sh/uv/issues/16427
synced_at: 2026-01-12T16:02:31Z
```

# Allow setting a custom value for `uv version --bump`

---

_@jvacek_

### Summary

Would be nice if we can set the version to bump the specific "section" up to, instead of always having to increment by one.

### Example

We want to automate the process of building and publishing our packages in CI, and part of that is making dev versions from a manual action in the CI.

The intent is that when ran, the package is bumped by a patch, and also a dev version, which has the value of the pipeline ID that ran it (an integer). This means that for example `0.0.1` → `0.0.2.dev66463664`. This avoids collisions when multiple dev versions are being worked on.

It would be nice if one could run `uv version --bump patch --bump dev 66463664`.

I would also expect that if the version is `0.0.12`, then for example `uv version --bump patch 11` would fail, as that is not a bump.

----

Furthermore, the current docs are confusing for this as it kinda seems that `--bump` and `VALUE` can be used together? could this just be a bug?

```
.venv ❯ uv version --bump dev 23
error: the argument '--bump <BUMP>' cannot be used with '[VALUE]'

Usage: uv version --bump <BUMP> [VALUE]
```

---

_Label `enhancement` added by @jvacek on 2025-10-23 19:07_

---

_Comment by @zanieb on 2025-10-23 20:01_

I think this seems like a pretty reasonable use-case, though I'm not sure what the best way to expose the interface is 🤔 

---

_Comment by @jvacek on 2025-10-24 11:49_

I suppose if you have the following usecases already:
```
uv version [VALUE]  # uv version 1.2.3.dev456
uv version [--bump <BUMP>]...  # uv version --bump patch --bump dev
```

Thoughts on adding it like so?
```
uv version [--bump <BUMP> [<value>]]...  # uv version --bump patch --bump dev 456
```


---

_Comment by @zanieb on 2025-10-24 13:35_

We can't have optional values associated with flags, e.g., `--foo <bar> [baz]` because it makes parsing ambiguous w.r.t. positional arguments. (It could technically be possible in some cases, but we avoid it)

---

_Comment by @jvacek on 2025-10-29 12:22_

how about this? 

`uv version [--bump <BUMP>[=<value>]]...  # uv version --bump patch --bump dev=456`

also a bit more "intuitive" about the value being set imo?

---

_Comment by @zanieb on 2025-10-29 14:11_

I'm okay with that. cc @Gankra and @konstin 

---

_Closed by @Gankra on 2025-11-11 17:27_

---
