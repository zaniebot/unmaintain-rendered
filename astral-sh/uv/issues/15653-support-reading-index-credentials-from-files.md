```yaml
number: 15653
title: Support reading index credentials from files.
type: issue
state: open
author: peter-facko
labels:
  - enhancement
assignees: []
created_at: 2025-09-03T10:31:41Z
updated_at: 2025-12-08T12:16:18Z
url: https://github.com/astral-sh/uv/issues/15653
synced_at: 2026-01-12T16:02:14Z
```

# Support reading index credentials from files.

---

_@peter-facko_

### Summary

Please implement a way to provide index credentials in a file. Buildah can only provide build secrets as files.

### Example

export UV_INDEX_INTERNAL_PROXY_USERNAME=file=/run/secrets/username
export UV_INDEX_INTERNAL_PROXY_PASSWORD=file=/run/secrets/password

---

_Label `enhancement` added by @peter-facko on 2025-09-03 10:31_

---

_Comment by @konstin on 2025-09-05 09:47_

We aren't likely to implement support for arbitrary files in uv, but we're adding a (preview) keyring provider that uses a file-based based storage for credentials. Another alternative is using the existing stable keyring support and implement a keyring plugin that reads from your preferred file.

---

_Comment by @peter-facko on 2025-09-05 11:58_

How do I use the file-based keyring provider?

I have, I think, a common use case, where I:

- use buildah
- run `uv sync` in the Containerfile
- use a private repository/index

---

_Comment by @zanieb on 2025-09-05 14:11_

I think it is reasonable to read secrets from files for Docker images, I'm just not sure how we'd best expose it. `file=...` is not unreasonable 🤔 

> we're adding a (preview) keyring provider that uses a file-based based storage for credentials

The `uv auth` CLI is persisting secrets to a plaintext store, but that's different than mounting secrets into individual files. The preview portion of `uv auth` is the system-native storage backend. 

---

_Closed by @zanieb on 2025-09-05 14:11_

---

_Reopened by @zanieb on 2025-09-05 14:11_

---

_Comment by @zanieb on 2025-09-05 14:11_

(bumped the button)

---

_Comment by @zanieb on 2025-09-05 14:14_

To implement your own file based keyring provider, you'd install a package like https://github.com/astral-sh/uv/blob/de1479c4ef41106d5e87f5ad6ba27b9f94c46b0c/scripts/packages/keyring_test_plugin but read from files. It sounds like a pain, but might be a viable workaround.

---

_Comment by @zanieb on 2025-09-05 14:15_

Does Buildah not support environment variable secrets? It seems like Docker does?

---

_Comment by @konstin on 2025-09-05 14:17_

For docker specifically, something like https://docs.docker.com/build/building/secrets/ may be the better choice.

---

_Comment by @peter-facko on 2025-09-06 12:07_

No, buildah doesn't support it. Buildkit supports it since Dockerfile version 1.10.0 https://docs.docker.com/build/buildkit/dockerfile-release-notes/#1100.

---

_Comment by @peter-facko on 2025-12-04 16:21_

Is there any update on this? I think that buildah supports providing secrets only as files, because in general, passing secrets through files is considered safer than passing environmental variables.

---

_Comment by @woodruffw on 2025-12-04 17:46_

> because in general, passing secrets through files is considered safer than passing environmental variables.

I'm curious, could you share a resource that claimed this? In general I'd expect the opposite to be true: a secret in a file is *de facto* globally readable to all other processes on the system (including anything that might have a global-read primitive, e.g. a path traversal bug in a web server), whereas environment variables are resident in process memory and can't be accessed without direct process memory read access. This is why the typical assumption for secret materials is that process memory is safe; if an attacker can read e.g. TLS session keys from memory, then most other security properties for a system are moot.

(I say "de facto" because in principle you could use user/group management to isolate files, but in practice most machines are single-user machines with a single relevant "persona" for credential management purposes.)

---

_Comment by @peter-facko on 2025-12-04 20:04_

Environmental variables can apparently leak accidentally. After a quick search, I found two relevant sources:

- https://stackoverflow.com/questions/37049439/is-passing-application-configuration-in-stdin-a-secure-alternative-to-environmen - explains some cases
- https://secrets-store-csi-driver.sigs.k8s.io/introduction - this whole security solution revolves around bypassing k8s secrets using volumes, which mount files

I think that it is important to note that I am using `uv` from a container. I didn't even think of a single-user machine and storing a static file with secrets to my index.

So I would correct my statement to "files are more secure than environmental secrets *in a containerized environment*".

---

_Comment by @konstin on 2025-12-08 12:16_

We had e.g. a case where a build backend dumped all env vars on failure (because you need them for debugging), leaking credentials in their setup (no CI credential redaction). That's unlike files, which generally don't get dumped and where access needs to be intentional (unlike using `os.environ`).

In my experience, files are easier to reason about because they have clear access rights and access is clearly defined e.g. for containers, while env vars generally often get propagated implicitly, often without documentation about it.

It would be ideal to have credentials in the system credential storage which can only be accessed by a dedicated API for secrets, offering good protections against for env var dumping and path traversal bugs, but these are more often than not available in environments such as docker. (This is not a statement about the security about system credential managers, but about the ergonomics of handling credentials as a user.)

---
