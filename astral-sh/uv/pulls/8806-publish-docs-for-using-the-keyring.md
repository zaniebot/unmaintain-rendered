```yaml
number: 8806
title: "Publish: Docs for using the keyring"
type: pull_request
state: closed
author: konstin
labels:
  - documentation
  - preview
assignees: []
base: main
head: konsti/document-the-keyring
created_at: 2024-11-04T10:16:49Z
updated_at: 2025-04-28T12:50:30Z
url: https://github.com/astral-sh/uv/pull/8806
synced_at: 2026-01-12T16:08:30Z
```

# Publish: Docs for using the keyring

---

_@konstin_

While publishing from CI is clearly the better choice, for smaller projects it may just not be worth the effort, so we also support publishing from a local machine.

We don't support `.pypirc`, the non-standard file that saves password as plaintext to the user home. Instead, we recommend using the keyring for local publishing, using the operating system's much more secure, mostly standardized and better queryable credential store as backend. This comes with the catch that the keyring keys by URL+username, so we need to use the horrible query string attach to tag per-project scoped tokens to a more specific URL (https://github.com/pypa/twine/issues/565).

Fixes #7963 


---

_Label `documentation` added by @konstin on 2024-11-04 10:16_

---

_Review requested from @zanieb by @konstin on 2024-11-04 10:16_

---

_Label `preview` added by @konstin on 2024-11-04 10:16_

---

_Review comment by @j178 on `docs/guides/publish.md`:94 on 2024-11-04 10:22_

```suggestion
keyring set 'https://upload.pypi.org/legacy/?PACKAGE_NAME' __token__
```


---

_Review comment by @j178 on `docs/guides/publish.md`:100 on 2024-11-04 10:22_

```suggestion
uv publish --username __token__ --keyring-provider subprocess --publish-url 'https://upload.pypi.org/legacy/?PACKAGE_NAME'
```


---

_@j178 reviewed on 2024-11-04 10:22_

---

_@konstin reviewed on 2024-11-04 10:27_

---

_Review comment by @konstin on `docs/guides/publish.md`:94 on 2024-11-04 10:27_

thanks!

---

_@cthoyt requested changes on 2024-11-04 14:04_

Given that the `?PACKAGE_NAME` suffix is a hack, I think it would also be beneficial to include in this guide the fact that you can have a single token for all publish URLs, and adding the hack is only an optional security precaution that allows you to have different tokens for different packages. I would suggest also including a section that says something like:

> If you don't need the added security of scoping tokens to single packages, and instead use one token for all packages, you can use the following:
>
> ```shell
> keyring set 'https://upload.pypi.org/legacy/' __token__
> ```
> 
> Then you can publish with:
> 
> ```shell
> uv publish --username __token__ --keyring-provider subprocess --publish-url 'https://upload.pypi.org/legacy/'
> ```

---

_Comment by @cthoyt on 2024-11-04 14:04_

Not to open up another can of worms, but what makes something standard or not? the `.pypirc` file is well-documented by the PyPA under its specifications list at https://packaging.python.org/en/latest/specifications/pypirc/

---

_Comment by @zanieb on 2024-11-04 20:21_

`.pypirc` is documented by the PyPA but isn't really a standard specification (i.e., no PEP) and the PyPA is warning people not to use it. Please use https://github.com/astral-sh/uv/issues/7676 for discussion on that topic.

---

_Comment by @konstin on 2024-11-05 00:15_

Personally, I find it misleading that `.pypirc` is in the specifications section of packaging.python.org next to the actual authoritative specs. As zanie said it has no PEP, it was added https://github.com/pypa/packaging.python.org/pull/734 and it's a user guide and not a specification. The only mention in a PEP is a 2002(!) extension to distutils: https://github.com/python/peps/blob/0bb65f91d0eb1b087eee1c61d2bc446f421ab032/peps/pep-0301.rst?plain=1#L233.

I'm focussing the guide on security best practices, such as using the keyring with least-privileged tokens.

---

_Closed by @konstin on 2025-04-28 12:50_

---
