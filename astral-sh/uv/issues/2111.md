```yaml
number: 2111
title: "Feature request: show available package versions (equivalent of `pip index versions`)"
type: issue
state: open
author: tlambert03
labels:
  - wish
assignees: []
created_at: 2024-03-01T14:03:01Z
updated_at: 2025-07-09T21:36:26Z
url: https://github.com/astral-sh/uv/issues/2111
synced_at: 2026-01-10T03:32:43Z
```

# Feature request: show available package versions (equivalent of `pip index versions`)

---

_Issue opened by @tlambert03 on 2024-03-01 14:03_

Apologies if in my search I've missed an open issue...

I'm hoping for a feature to list/search available versions of a package.  The equivalent of 


```shell
# for pip >= 21.2
pip index versions <some-package>

# or, the old hack:
pip install <some-package>==
```

Is this currently possible, or in the future scope of the project?

Thanks!


---

_Label `wish` added by @konstin on 2024-04-29 14:00_

---

_Comment by @wimglenn on 2024-05-28 20:46_

Example output:

```
$ podman run --rm -it python:3.12 bash
root@ce153fd14fb0:/# pip index versions uv
WARNING: pip index is currently an experimental command. It may be removed/changed in a future release without prior warning.
uv (0.2.5)
Available versions: 0.2.5, 0.2.4, 0.2.3, 0.2.2, 0.2.1, 0.2.0, 0.1.45, 0.1.44, 0.1.43, 0.1.42, 0.1.41, 0.1.40, 0.1.39, 0.1.38, 0.1.37, 0.1.36, 0.1.35, 0.1.34, 0.1.33, 0.1.32, 0.1.31, 0.1.30, 0.1.29, 0.1.28, 0.1.27, 0.1.26, 0.1.25, 0.1.24, 0.1.23, 0.1.22, 0.1.21, 0.1.20, 0.1.19, 0.1.18, 0.1.17, 0.1.16, 0.1.15, 0.1.14, 0.1.13, 0.1.12, 0.1.11, 0.1.10, 0.1.9, 0.1.8, 0.1.7, 0.1.6, 0.1.5, 0.1.4, 0.1.3, 0.1.2, 0.1.1, 0.1.0, 0.0.5
```

Old hack (~still works~ stopped working in pip 24.0):

```
root@ce153fd14fb0:/# pip install uv==
ERROR: Could not find a version that satisfies the requirement uv== (from versions: 0.0.5, 0.1.0, 0.1.1, 0.1.2, 0.1.3, 0.1.4, 0.1.5, 0.1.6, 0.1.7, 0.1.8, 0.1.9, 0.1.10, 0.1.11, 0.1.12, 0.1.13, 0.1.14, 0.1.15, 0.1.16, 0.1.17, 0.1.18, 0.1.19, 0.1.20, 0.1.21, 0.1.22, 0.1.23, 0.1.24, 0.1.25, 0.1.26, 0.1.27, 0.1.28, 0.1.29, 0.1.30, 0.1.31, 0.1.32, 0.1.33, 0.1.34, 0.1.35, 0.1.36, 0.1.37, 0.1.38, 0.1.39, 0.1.40, 0.1.41, 0.1.42, 0.1.43, 0.1.44, 0.1.45, 0.2.0, 0.2.1, 0.2.2, 0.2.3, 0.2.4, 0.2.5)
ERROR: No matching distribution found for uv==
```

---

_Comment by @per11235813 on 2024-09-01 06:26_

It would be very useful to have something like "pip index versions ..."

Compared to pip "uv pip" have changed the semantics of extra-index-url, so it is useful in handling internal and external packages in a simple and safe way. With this change it would be very nice to check available version.

Btw, I just started using uv. The speed, the "extra-index-url"  and tools handling are utterly amazing. Thanks!



---

_Comment by @nrontsis on 2024-12-19 14:46_

+1

---

_Comment by @ion-elgreco on 2025-01-17 12:57_

This would be very useful! :)

---

_Comment by @zanieb on 2025-01-17 18:42_

Please see #9452 — do not post +1 comments use 👍  instead.

---

_Comment by @per11235813 on 2025-02-06 05:41_

Just realized the lack `pip index versions ... ` can be mitigated with uv:  `uvx pip index versions ...`

I just love `uv`!

---

_Comment by @per11235813 on 2025-02-19 13:45_

I use it with a private index. You must configure the index url for
index explicitly, like this

[install]
> index-url = https://...
>
> [download]
> index-url = https://...
>
> [index]
> index-url = https://...
>


Br,
Per

On Wed, Feb 19, 2025 at 9:37 AM Scott ***@***.***> wrote:

> Just realized the lack pip index versions ... can be mitigated with uv: uvx
> pip index versions ...
>
> I just love uv!
>
> Sadly this doesn't work if you're searching for a package which uses a
> private pypi server. the pip search defaults to only the default pypi
> distribution.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/2111#issuecomment-2667917946>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AGMXSU5VESULVV3P2F5DZWL2QQ7ELAVCNFSM6AAAAABEB2TBGGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDMNRXHEYTOOJUGY>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
> [image: ScottWilliamAnderson]*ScottWilliamAnderson* left a comment
> (astral-sh/uv#2111)
> <https://github.com/astral-sh/uv/issues/2111#issuecomment-2667917946>
>
> Just realized the lack pip index versions ... can be mitigated with uv: uvx
> pip index versions ...
>
> I just love uv!
>
> Sadly this doesn't work if you're searching for a package which uses a
> private pypi server. the pip search defaults to only the default pypi
> distribution.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/2111#issuecomment-2667917946>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AGMXSU5VESULVV3P2F5DZWL2QQ7ELAVCNFSM6AAAAABEB2TBGGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDMNRXHEYTOOJUGY>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


-- 

Med venlig hilsen

Per Stoffer Jensen
Kæragervej 14
2720 Vanløse
22406349


---

_Comment by @eegli on 2025-04-23 07:29_

While the following does not show _all_ package versions, it allows you to easily display available updates:

```sh
uv tree --depth 1 --outdated
```

With `--depth 1`, only the project dependencies specified in `pyproject.toml` files but not the transitive dependencies are shown.


---

_Comment by @kwaegel on 2025-07-09 21:36_

For what it's worth, I've been using `pip index versions <private-package>` to check if authentication credentials for a private repo are valid. It's a slight abuse of the command, but useful.

---
