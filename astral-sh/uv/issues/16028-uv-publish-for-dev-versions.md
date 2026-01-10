```yaml
number: 16028
title: uv publish for dev versions
type: issue
state: closed
author: berkorbay
labels:
  - question
assignees: []
created_at: 2025-09-25T17:39:27Z
updated_at: 2025-09-28T09:21:23Z
url: https://github.com/astral-sh/uv/issues/16028
synced_at: 2026-01-10T03:23:54Z
```

# uv publish for dev versions

---

_Issue opened by @berkorbay on 2025-09-25 17:39_

### Question

When I have a pre-release with a dev versioning (e.g. 1.3.1.dev0) and I try `uv publish` I get the error that version 1.3.0 is already uploaded ("Caused by: Upload failed with status code 400 Bad Request. Server says: 400 File already exists"). 

Though I can upload it with poetry easily.

### Platform

macOS

### Version

uv 0.7.2 (481d05d8d 2025-04-30)

---

_Label `question` added by @berkorbay on 2025-09-25 17:39_

---

_Comment by @konstin on 2025-09-25 17:40_

Can you share verbose logs?


---

_Comment by @berkorbay on 2025-09-25 18:36_

Just realized, I have multiple versions under dist/ and uv gets the first one. 

```
DEBUG uv 0.7.2 (481d05d8d 2025-04-30)
DEBUG Not a distribution filename: `.gitignore`
Publishing 114 files https://upload.pypi.org/legacy/
DEBUG Using request timeout of 900s
Uploading packagex-0.1.0-py3-none-any.whl (11.5KiB)
DEBUG Hashing dist/packagex-0.1.0-py3-none-any.whl
DEBUG Using HTTP Basic authentication
DEBUG Response code for https://upload.pypi.org/legacy/: 400 Bad Request
DEBUG Upload error response: {"message": "The server could not comply with the request since it is either malformed or otherwise incorrect.\n\n\nFile already exists ('packagex-0.1.0-py3-none-any.whl', with blake2_256 hash 'cc9732fd69049c8c3cc0a233cec84604d81fa08a4df5a111febb3b91ce692a42'). See https://pypi.org/help/#file-name-reuse for more information.\n\n", "code": "400 File already exists ('packagex-0.1.0-py3-none-any.whl', with blake2_256 hash 'cc9732fd69049c8c3cc0a233cec84604d81fa08a4df5a111febb3b91ce692a42'). See https://pypi.org/help/#file-name-reuse for more information.", "title": "Bad Request"}
error: Failed to publish `dist/packagex-0.1.0-py3-none-any.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 File already exists ('packagex-0.1.0-py3-none-any.whl', with blake2_256 hash 'cc9732fd69049c8c3cc0a233cec84604d81fa08a4df5a111febb3b91ce692a42'). See https://pypi.org/help/#file-name-reuse for more information.
```

---

_Comment by @zanieb on 2025-09-25 18:51_

Sounds like you should either remove the old distributions or pass the specific distributions you want to upload to `uv publish`.

---

_Comment by @berkorbay on 2025-09-25 19:34_

> Sounds like you should either remove the old distributions or pass the specific distributions you want to upload to `uv publish`.

That's one way to go. Instead I successfully tried and succeeded in `poetry publish` :) I recently transitioned this package from poetry to uv so poetry still lingers around. Next time I will try to specify the file. 

Weakly irrelevant but I need to use --token every time I use `uv publish` (ps. I did not add token to zsh environment but it is in .env of my project) but it is not the case with Poetry. It makes the publishing much more seamless. 

Don't get me wrong, I love using uv. Though Poetry in this case has the better practice for my use case. Perhaps a little peek 👀 at Poetry process would help as I understand you both use twine under the hood?

---

_Comment by @zanieb on 2025-09-25 19:37_

We don't use twine under the hood, we've implement publishing ourselves. We support persistent credentials via the `uv auth login` interface, but it's fairly new so it's not discussed often in the documentation yet.

---

_Comment by @zanieb on 2025-09-25 19:37_

Presumably Poetry is just skipping the existing artifact with a different SHA? We don't really want to hide that from you.

---

_Comment by @zanieb on 2025-09-25 19:38_

This does remind me of https://github.com/astral-sh/uv/issues/14010 though

---

_Closed by @berkorbay on 2025-09-28 09:21_

---
