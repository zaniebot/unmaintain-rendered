---
number: 9998
title: fork-strategy requires-python produces unexpected results with repeated dependencies
type: issue
state: closed
author: alex
labels:
  - bug
assignees: []
created_at: 2024-12-18T13:28:48Z
updated_at: 2024-12-18T21:54:57Z
url: https://github.com/astral-sh/uv/issues/9998
synced_at: 2026-01-10T01:24:49Z
---

# fork-strategy requires-python produces unexpected results with repeated dependencies

---

_Issue opened by @alex on 2024-12-18 13:28_

Hello!

I'm playing with using `fork-strategy` to replace our current `tool.uv.environments`, however it doesn't seem to be working due to some dependencies being repeated.

To reproduce:

```
$ echo -e "nox >=2024.04.15\nnox[uv] >=2024.03.02; python_version >= '3.8'" | uv pip compile --universal -p 3.7 -
```

If you look at the result, you'll see that there is a single version of `argcomplete==3.1.2`. However, what I'd expect to see is:

```
argcomplete==3.1.2 ; python_full_version < '3.8'
    # via nox
argcomplete==3.5.2 ; python_full_version >= '3.8'
    # via nox
```

When I do a more minimal test (`echo 'nox' | uv pip compile --universal -p 3.7 -`), I see the expected behavior.

---

_Assigned to @charliermarsh by @zanieb on 2024-12-18 15:20_

---

_Label `bug` added by @charliermarsh on 2024-12-18 15:26_

---

_Comment by @charliermarsh on 2024-12-18 15:34_

Thanks yeah, I see the issue here. It should be an easy fix, I'm just evaluating the fixture changes to make sure it's correct.

---

_Comment by @alex on 2024-12-18 16:08_

Great, thanks!

On Wed, Dec 18, 2024, 10:35 AM Charlie Marsh ***@***.***>
wrote:

> Thanks yeah, I see the issue here. It should be an easy fix, I'm just
> evaluating the fixture changes to make sure it's correct.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/9998#issuecomment-2551634752>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBGIXUJOIE22AFQ7TZT2GGI3RAVCNFSM6AAAAABT2WX35WVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDKNJRGYZTINZVGI>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Referenced in [astral-sh/uv#10007](../../astral-sh/uv/pulls/10007.md) on 2024-12-18 16:31_

---

_Comment by @notatallshaw-gts on 2024-12-18 20:13_

FWIW, I was also going to report this, I was surprised to see no resolution difference between the two fork strategies when I threw very complex resolutions at them, but my resolutions were too complex to produce a useful MRE 😰

---

_Closed by @charliermarsh on 2024-12-18 21:54_

---
