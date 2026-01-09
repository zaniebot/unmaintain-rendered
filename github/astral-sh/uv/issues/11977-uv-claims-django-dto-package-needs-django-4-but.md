---
number: 11977
title: uv claims django-dto package needs django <4, but thats not what the dependency declaration says
type: issue
state: closed
author: sean-abbott
labels:
  - question
assignees: []
created_at: 2025-03-05T14:54:13Z
updated_at: 2025-03-05T16:10:28Z
url: https://github.com/astral-sh/uv/issues/11977
synced_at: 2026-01-07T13:12:18-06:00
---

# uv claims django-dto package needs django <4, but thats not what the dependency declaration says

---

_Issue opened by @sean-abbott on 2025-03-05 14:54_

### Question

Why does uv think that django-dto depends on django<4? 

When I attempt to add [django-dto](https://github.com/nicogall/django-dto) to my project, I get:

```
❯ uv add django-dto
  × No solution found when resolving dependencies:
  ╰─▶ Because only django-dto==0.1.3 is available and django-dto==0.1.3 depends on django>=3.0,<4.0, we can
      conclude that all versions of django-dto depend on django>=3.0,<4.0.
      And because my-package depends on django>=5.1.6, we can conclude that my-package and all
      versions of django-dto are incompatible.
      And because cl-config-data depends on django-dto and your workspace requires my-package, we can
      conclude that your workspace's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to
        skip locking and syncing.
```

but django-dto declares django > 3 (with no <4):
```
classifiers = [
    'Development Status :: 4 - Beta',
    'Environment :: Web Environment',
    'Intended Audience :: Developers',
    'License :: OSI Approved :: MIT License',
    'Operating System :: OS Independent',
    'Programming Language :: Python',
    'Programming Language :: Python :: 3',
    'Programming Language :: Python :: 3.9',
    'Programming Language :: Python :: 3.10',
    'Programming Language :: Python :: 3.11',
    'Programming Language :: Python :: 3.12',
    'Framework :: Django',
    'Framework :: Django :: 3.2',
    'Framework :: Django :: 4.0',
    'Framework :: Django :: 4.1',
    'Framework :: Django :: 4.2',
    'Framework :: Django :: 5.0',
]

[tool.poetry.dependencies]
python = "^3.9"
django = "^3.0"
```

Cloning locally and bumping python and django to >3.12 and > 5.0, respectively, allows me to install via path, but I'd rather understand why uv believes it requies django < 4 so I can submit a PR to the maintainer and be able to use the package directly.

### Platform

Linux 6.8.0-54-generic x86_64 GNU/Linux (ubuntu 24.04)

### Version

uv 0.5.29

---

_Label `question` added by @sean-abbott on 2025-03-05 14:54_

---

_Comment by @charliermarsh on 2025-03-05 14:54_

I believe that `django = "^3.0"` means `django = ">=3.0, <4"`.

---

_Referenced in [nicogall/django-dto#2](../../nicogall/django-dto/issues/2.md) on 2025-03-05 14:54_

---

_Comment by @charliermarsh on 2025-03-05 14:55_

(That's non-standard syntax that is specific to Poetry.)

---

_Comment by @notatallshaw-gts on 2025-03-05 15:04_

FYI, the only way to _really_ confirm the dependency metadata is to check it directly:

1. Go to the pypi page: https://pypi.org/project/django-dto/
2. Go to download files: https://pypi.org/project/django-dto/#files
3. Right click on the ".whl" file and copy link address
4. Paste it into the address bar and add ".metadata" on the end: https://files.pythonhosted.org/packages/2a/69/142740f5ece5b286e254720b10b61151a91a0bd1518ebc93939632c56179/django_dto-0.1.3-py3-none-any.whl.metadata
5. Open the downloaded file as a text file and look for lines that say `Requires-Dist:`

Here are the requirements for this package:

> Requires-Dist: django (>=3.0,<4.0)

This is a classic example of why defaulting to upper bounds is bad for a library, future users could use a library fine, but  an author has asserted things about future to protect a against a hypothetical failure.


**Edit:** Seems like I accidentally posted twice, not sure what happened, deleted the other post.

---

_Comment by @sean-abbott on 2025-03-05 15:13_

Ah, ok, thank you so much for the incredibly quick response!

That gives me a pr I can send, so perfect!  

---

_Closed by @sean-abbott on 2025-03-05 15:13_

---

_Referenced in [nicogall/django-dto#3](../../nicogall/django-dto/pulls/3.md) on 2025-03-05 15:21_

---

_Comment by @notatallshaw-gts on 2025-03-05 15:24_

FYI, uv allows you to override your dependencies metadata: https://docs.astral.sh/uv/reference/settings/#dependency-metadata.

This can be useful for such situations if a library author is unresponsive, can save having to fork the project to check if it works with different dependencies.

---

_Comment by @sean-abbott on 2025-03-05 15:33_

Oh that's awesome, thank you!

On Wed, Mar 5, 2025 at 10:24 AM Damian Shaw ***@***.***>
wrote:

> FYI, uv allows you to override your dependencies metadata:
> https://docs.astral.sh/uv/reference/settings/#dependency-metadata.
>
> This can be useful for such situations if a library author is
> unresponsive, can save having to fork the project to check if it works with
> different dependencies.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701263775>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAKWJVZARM3YQA6WSPHXWBT2S4JMRAVCNFSM6AAAAABYMJVDIGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMBRGI3DGNZXGU>
> .
> You are receiving this because you modified the open/close state.Message
> ID: ***@***.***>
> [image: notatallshaw-gts]*notatallshaw-gts* left a comment
> (astral-sh/uv#11977)
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701263775>
>
> FYI, uv allows you to override your dependencies metadata:
> https://docs.astral.sh/uv/reference/settings/#dependency-metadata.
>
> This can be useful for such situations if a library author is
> unresponsive, can save having to fork the project to check if it works with
> different dependencies.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701263775>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAKWJVZARM3YQA6WSPHXWBT2S4JMRAVCNFSM6AAAAABYMJVDIGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMBRGI3DGNZXGU>
> .
> You are receiving this because you modified the open/close state.Message
> ID: ***@***.***>
>


---

_Comment by @sean-abbott on 2025-03-05 15:46_

Ok, how do I use this appropriately, though?

Relevant pyproject snippet:
```
requires-python = ">=3.12"
dependencies = [
    "django>=5.1.6",
    "django-dto",
]

[tool.uv]
dependency-metadata = [
    { name = "django-dto", requires-dist = ["django>=5.0"] }
]
```

Returns the same dependency resolution error. Do I need to remove it from dependencies?

---

_Comment by @notatallshaw-gts on 2025-03-05 15:53_

I'm not that familiar with exactly how it works, but try clearing the cache?

But perhaps someone else can help if you provide the command you ran and output that you got.

---

_Comment by @charliermarsh on 2025-03-05 15:55_

This worked just fine for me:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django>=5.1.6",
    "django-dto",
]

[tool.uv]
dependency-metadata = [
    { name = "django-dto", requires-dist = ["django>=5.0"] }
]
```


---

_Comment by @sean-abbott on 2025-03-05 16:03_

I tried cleaning the cache for that package, and got the same result as the original. My pyproject is basically the same as yours
```
[project]
name = "my-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "seanabbott", email = "xxx" }
]
requires-python = ">=3.12"
dependencies = [
    "django>=5.1.6",
    "django-dto",
]

[tool.uv]
dependency-metadata = [
    { name = "django-dto", requires-dist = ["django>=5.0"] }
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

and I still get:
```
❯ uv sync
  × No solution found when resolving dependencies:
  ╰─▶ Because only django-dto==0.1.3 is available and django-dto==0.1.3 depends on django>=3.0,<4.0, we can
      conclude that all versions of django-dto depend on django>=3.0,<4.0.
      And because my-project depends on django>=5.1.6, we can conclude that my-project and all
      versions of django-dto are incompatible.
      And because my-project depends on django-dto and your workspace requires my-project we can
      conclude that your workspace's requirements are unsatisfiable.
```

The difference may be that this is one package that is part of a larger workspace, or the hatch build system? I do not use django-dto anywhere else in the workspace, though. 

I updated uv, so now I'm on uv 0.6.4

---

_Comment by @charliermarsh on 2025-03-05 16:05_

These need to be defined in the root of the workspace.

---

_Comment by @sean-abbott on 2025-03-05 16:10_

Ah, that would do it, thank you!

On Wed, Mar 5, 2025 at 11:05 AM Charlie Marsh ***@***.***>
wrote:

> These need to be defined in the root of the workspace.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701392267>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAKWJV4ZTNRM2CGIBX6BMND2S4OFJAVCNFSM6AAAAABYMJVDIGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMBRGM4TEMRWG4>
> .
> You are receiving this because you modified the open/close state.Message
> ID: ***@***.***>
> [image: charliermarsh]*charliermarsh* left a comment (astral-sh/uv#11977)
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701392267>
>
> These need to be defined in the root of the workspace.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/11977#issuecomment-2701392267>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAKWJV4ZTNRM2CGIBX6BMND2S4OFJAVCNFSM6AAAAABYMJVDIGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMBRGM4TEMRWG4>
> .
> You are receiving this because you modified the open/close state.Message
> ID: ***@***.***>
>


---
