---
number: 16623
title: "Forbid credentials in URLs in `pyproject.toml` index definitions"
type: issue
state: open
author: woodruffw
labels:
  - security
  - needs-decision
assignees: []
created_at: 2025-11-06T21:54:41Z
updated_at: 2025-11-12T15:58:21Z
url: https://github.com/astral-sh/uv/issues/16623
synced_at: 2026-01-07T13:12:19-06:00
---

# Forbid credentials in URLs in `pyproject.toml` index definitions

---

_Issue opened by @woodruffw on 2025-11-06 21:54_

Right now, uv allows URLs with credentials in the `[[tools.uv.index]]` table. For example, this is permitted:

```toml
[[tool.uv.index]]
name = "super-secure"
url = "https://foo:hunter2@example.com/simple"
default = true
```

(Where `hunter2` is a presumably secret password.)

This is arguably not super ideal for a few reasons:

1. It's a plain text file, and credentials should *generally* not be in plain text files. The overall status quo is that credentials in plain text files are pretty common (netrc, pypirc, cargo credentials, etc.), so we're not "out of band" here, but it's still not ideal.
2. Validation of pyproject.toml files isn't redacted in any way, meaning that a syntax or semantic error in  a user's config could accidentally disclose their credentials to stdout/stderr. This may or may not be a security concern depending on the usage context, but it *is* inconsistent with efforts we take elsewhere in the codebase to avoid accidental credential display (like `DisplaySafeUrl`). An example of this can be seen in the snapshots in #16622.

So, maybe we should forbid these? On the other hand, doing so might be pretty disruptive, especially to users who are using credentials-in-index-urls-in-pyproject in settings where it's less of an issue (e.g. outside of source control). @zanieb points out that we could maybe fine-tune the check here to only fire when we detect that the `pyproject.toml` is in source control to somewhat mitigate this case.


---

_Label `needs-decision` added by @woodruffw on 2025-11-06 21:54_

---

_Referenced in [astral-sh/uv#16622](../../astral-sh/uv/pulls/16622.md) on 2025-11-06 22:17_

---

_Referenced in [astral-sh/uv#13668](../../astral-sh/uv/issues/13668.md) on 2025-11-07 15:29_

---

_Label `security` added by @woodruffw on 2025-11-07 15:30_

---

_Comment by @woutervh on 2025-11-11 17:55_

Is the username alone also not considered a credential?  
I do put `oauth2accesstoken` in the index-url, to avoid keyring requesting the username interactively.

If a password is found,  a big red warning could be shown.



---

_Comment by @woodruffw on 2025-11-12 15:58_

> Is the username alone also not considered a credential? I do put `oauth2accesstoken` in the index-url, to avoid keyring requesting the username interactively.
> 
> If a password is found, a big red warning could be shown.

I was referring specifically to passwords since usernames are *generally* non-sensitive information (although plenty of services violate this, e.g. GitHub historically allowed the username/password in basic authentication to be swapped. Not sure if they still do.)



---
