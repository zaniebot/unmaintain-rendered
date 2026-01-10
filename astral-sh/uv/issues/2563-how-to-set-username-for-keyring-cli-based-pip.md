---
number: 2563
title: How to set username for keyring cli based pip authorization (Azure artifacts) 
type: issue
state: closed
author: jenshnielsen
labels:
  - enhancement
assignees: []
created_at: 2024-03-20T13:20:33Z
updated_at: 2024-04-16T16:49:15Z
url: https://github.com/astral-sh/uv/issues/2563
synced_at: 2026-01-10T01:23:19Z
---

# How to set username for keyring cli based pip authorization (Azure artifacts) 

---

_Issue opened by @jenshnielsen on 2024-03-20 13:20_

For keyring authorization to work with Azure artifacts and the keyring CLI I have to supply a default username that must be 
`VssSessionToken`
 
Trying to set this for UV in the same way as I would do for[ pip](https://pip.pypa.io/en/stable/topics/authentication/#basic-http-authentication) e.g.
```
uv pip install --keyring-provider subprocess --index-url https://VssSessionToken@pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/ nameofpackage --verbose
```
results in no keyring operation probably because UV identifies the username as a password/token
 
```
DEBUG No cache entry for: https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/
DEBUG Request already has an authorization header: https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/
error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/)
```
It seems that pip supports that the form where only one part is given could be both username and a usernameless token but UV does not.
 
Using the default password on the other hand results in no response from keyring (can be confirmed by calling keyring directly)
 
```
uv pip install --keyring-provider subprocess --index-url https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/ nameofpackage --verbose
```
 
```
DEBUG No cache entry for: https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/
DEBUG Running `keyring get` for `[https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/`](https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/%60) with username `oauth2accesstoken`
DEBUG No keyring credentials found for https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/
DEBUG No credentials found for: https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/
error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/nameofpackage/)
```
 
If I manually rebuild uv with the default username changed from `oauth2accesstoken` to `VssSessionToken` everything works correctly.
 
Is there a different way I am supposed to embed the username?
 
System:
Windows 11
Python 3.12 conda env
uv 1.22


---

_Comment by @zanieb on 2024-03-20 14:02_

I think we just don't support this yet although we probably can!

Can you share the keyring invocation we are performing and the other you would prefer?

---

_Label `enhancement` added by @zanieb on 2024-03-20 14:02_

---

_Assigned to @zanieb by @zanieb on 2024-03-20 14:02_

---

_Comment by @zanieb on 2024-03-20 14:14_

Okay thanks I can take a look into this unless @BakerNet is interested

---

_Comment by @jenshnielsen on 2024-03-20 14:15_

Thanks @zanieb
In the first example with the username given as part of the url (but no password in the url)
no keyring invocation is triggered since UV seems to assume that the username provided in the url is a token.

In the second example you call keyring something like this (which would return None)
```
keyring get https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/ oauth2accesstoken
```
but I would need.
```
keyring get https://pkgs.dev.azure.com/nameoforg/_packaging/nameoffeed/pypi/simple/ VssSessionToken 
```
which correctly returns a token when I call it from the commandline



---

_Comment by @BakerNet on 2024-03-20 14:29_

> Okay thanks I can take a look into this unless @BakerNet is interested

I know why this is happening and can work on it.  It's because of the early return in the middleware when the header is already added from the URL.  If there is a username in the URL, but no password, it should be checking keyring and replacing the header if found.

`pip` only relies on URL encoded auth if it includes a password, but `uv` is currently relying on it even if it only contains username.

---

_Assigned to @BakerNet by @zanieb on 2024-03-20 14:32_

---

_Referenced in [astral-sh/uv#2570](../../astral-sh/uv/pulls/2570.md) on 2024-03-20 18:55_

---

_Comment by @BakerNet on 2024-03-25 17:42_

The PR for this has been ready for a few days (#2570 )

I don't mean to rush, just an FYI in case it wasn't clear it's ready for review.

---

_Referenced in [astral-sh/uv#2984](../../astral-sh/uv/pulls/2984.md) on 2024-04-11 20:24_

---

_Referenced in [astral-sh/uv#2976](../../astral-sh/uv/pulls/2976.md) on 2024-04-15 19:55_

---

_Comment by @zanieb on 2024-04-16 16:49_

Should be resolved by https://github.com/astral-sh/uv/pull/2976 and available in the next release.

---

_Closed by @zanieb on 2024-04-16 16:49_

---

_Referenced in [Homebrew/homebrew-core#169338](../../Homebrew/homebrew-core/pulls/169338.md) on 2024-04-17 18:52_

---
