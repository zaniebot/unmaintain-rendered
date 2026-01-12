```yaml
number: 9997
title: uv publish does not read environment variables
type: issue
state: closed
author: fleetingbytes
labels: []
assignees: []
created_at: 2024-12-18T13:21:09Z
updated_at: 2024-12-18T15:25:03Z
url: https://github.com/astral-sh/uv/issues/9997
synced_at: 2026-01-12T16:00:04Z
```

# uv publish does not read environment variables

---

_@fleetingbytes_

I have the environment variables `UV_PUBLISH_URL` and `UV_PUBLISH_TOKEN` set to my corporate PyPI package registry and my authentication token. When I run `uv publish dist/*`, uv tries to upload it to pypi.org:

```
$ uv publish dist/*
warning: `uv publish` is experimental and may change without warning
Publishing 2 files https://upload.pypi.org/legacy/
Enter username ('__token__' if using a token):
```

When I run `uv publish --publish-url=$UV_PUBLISH_URL --token=$UV_PUBLISH_TOKEN dist/*` then everything is fine and the package gets published in my corporate package registry (output redacted).

```
$ uv publish --publish-url=$UV_PUBLISH_URL --token=$UV_PUBLISH_TOKEN dist/*
warning: `uv publish` is experimental and may change without warning
Publishing 2 files https://<corporate-gitlab-host>/api/v4/projects/<project-id>/packages/pypi
Uploading <file-name>
```

I can't imagine that there would be such bug, but maybe I missed something when reading the uv documentation. 

I use uv 0.5.10 (294da5261 2024-12-17) on Linux 6.1.0-28-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.119-1 (2024-11-22) x86_64 GNU/Linux

---

_Comment by @nathanscain on 2024-12-18 14:01_

Are you exporting the env vars? In bash, saying `ABC=XYZ` only sets `$ABC` for the shell. You have to do `export ABC=XYZ` for subprocesses like uv to see it. 

---

_Comment by @fleetingbytes on 2024-12-18 14:39_

Thank you, that was exactly the issue, they were only set for the shell. After export it works as expected. 🙏🏻 

---

_Closed by @fleetingbytes on 2024-12-18 14:39_

---

_Comment by @nathanscain on 2024-12-18 14:40_

Awesome - happy to help 🤙

---

_Comment by @zanieb on 2024-12-18 15:25_

Thanks @nathanscain !

---
