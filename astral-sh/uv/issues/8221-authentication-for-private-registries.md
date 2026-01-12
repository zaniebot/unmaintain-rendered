```yaml
number: 8221
title: Authentication for private registries
type: issue
state: closed
author: juan-abia
labels:
  - registry
assignees: []
created_at: 2024-10-15T15:27:57Z
updated_at: 2024-10-17T13:15:31Z
url: https://github.com/astral-sh/uv/issues/8221
synced_at: 2026-01-12T15:59:22Z
```

# Authentication for private registries

---

_@juan-abia_

Hello! thanks of all, thanks for your awesome work at astral-sh!

I'm trying to use uv to publish to a private registry (cloudsmith). But I'm getting Unauthorized error. The same token is working using poetry. Here is the line I execute:

`uv publish --verbose --publish-url https://python.cloudsmith.io/<my_org>/pip/ --token <cloudsmith_api_key> <my_package>.tar.gz`

Here is the output:
```
DEBUG uv 0.4.20
warning: `uv publish` is experimental and may change without warning
Publishing 1 file to https://python.cloudsmith.io/<my_org>/pip/
DEBUG Using request timeout of 900s
Uploading <my_package>.tar.gz (279.2KiB)
DEBUG Using username/password basic auth
DEBUG Response code for https://python.cloudsmith.io/<my_org>/pip/: 401 Unauthorized
DEBUG Upload error response: {"detail": "Authentication credentials were not provided."}
error: Failed to publish `<my_package>.tar.gz` to https://python.cloudsmith.io/<my_org>/pip/
  Caused by: Upload failed with status code 401 Unauthorized. Server says: {"detail": "Authentication credentials were not provided."}
```

I've tried using the `UV_PUBLISH_TOKEN ` env var. And also using username as __token__ and the api key as the password.

Could you please add compatibility with cloudsmith? TY!


---

_Comment by @astrojuanlu on 2024-10-15 16:17_

@juan-abia Does it work if you configure a `.netrc` file?

```
$ cat ~/.netrc
machine python.cloudsmith.io
  login LOGIN
  password TOKEN
```

---

_Comment by @juan-abia on 2024-10-16 07:14_

Doesn't work @astrojuanlu :(
I also updated to verion 0.4.22 and no luck

---

_Comment by @lskillen on 2024-10-16 12:52_

Hey folks 👋 I'm representing the Cloudsmith team here, so I'm happy to help diagnose from our side, too.

We also utilize `uv` (and `ruff` too, actually) at Cloudsmith extensively, so we'd be delighted to offer support. :)

If you need a free account to test out, just let us know, too. Cheers!

---

_Comment by @charliermarsh on 2024-10-16 12:57_

Thank you so much @lskillen -- really appreciate that! A test account would be super useful. I'll let @konstin follow up.

---

_Comment by @konstin on 2024-10-16 12:58_

Hi @lskillen! We'd love to collaborate with you on this, and a test account would be great! You can reach me at konsti@astral.sh (or on discord as `konsti2849`) for coordinating the details.

---

_Comment by @lskillen on 2024-10-16 13:01_

Sounds good, I'll drop you an email and intro you to our team. :)

---

_Label `registry` added by @zanieb on 2024-10-16 14:21_

---

_Comment by @konstin on 2024-10-16 15:57_

@astrojuanlu For cloudsmith, you need to use username and password authentication: For the username, use your username or `token`, the password is your token. For `--token` we use the PyPI default of `__token__`, which mismatches with the cloudsmith `token`.

---

_Comment by @juan-abia on 2024-10-16 16:30_

@konstin just confirmed that using `token` works

---

_Comment by @konstin on 2024-10-17 12:39_

Cloudsmith updated their backend to also allow `__token__`, so you can now publish with `--token`, too. Shoutout to the cloudsmith team for adding this so quickly!

---

_Comment by @lskillen on 2024-10-17 12:49_

Glad it worked. 😁 Happy to be included on a list if one ever exists of "Supported services that work". 😆 

---

_Comment by @juan-abia on 2024-10-17 13:15_

Thanks a lot team! closing the issue

---

_Closed by @juan-abia on 2024-10-17 13:15_

---
