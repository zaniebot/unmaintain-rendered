---
number: 13311
title: "[FR] Add config option to emit pre-0.5 condensed output of `ruff check`"
type: issue
state: closed
author: sterliakov
labels:
  - question
assignees: []
created_at: 2024-09-10T16:50:40Z
updated_at: 2024-09-10T20:54:26Z
url: https://github.com/astral-sh/ruff/issues/13311
synced_at: 2026-01-07T13:12:15-06:00
---

# [FR] Add config option to emit pre-0.5 condensed output of `ruff check`

---

_Issue opened by @sterliakov on 2024-09-10 16:50_

In 0.5.0 the format of ruff's output became much more verbose. It may be useful in some cases, but I use ruff mostly in pre-commit and will navigate to all these files anyway. I don't want to see the big code snippet and would rather see just code, file+lineno and short description.

I do not propose reverting the design change, I only would like to have e.g. `--condensed` flag available.

This will also make adding `ruff` to new projects more trivial: I finally went to ask for this feature after trying to survive scrolling through 204 diagnostics.

Compare the following (keeping in mind that I see 200 such messages).

```
rest_framework_simplejwt/views.py:42:47: ARG002 Unused method argument: `kwargs`
   |
40 |         )
41 | 
42 |     def post(self, request: Request, *args, **kwargs) -> Response:
   |                                               ^^^^^^ ARG002
43 |         serializer = self.get_serializer(data=request.data)
   |
```

vs

```
rest_framework_simplejwt/views.py:42:47: ARG002 Unused method argument: `kwargs`
``` 

Keywords: verbose, CLI, output, silent, condensed

I'm running `ruff check some_project/ --select ALL --isolated` (for example).

Ad a workaround, I pretend to be a TTY and `grep` a bit, obtaining the desired output:

```bash
unbuffer -p ruff check some_project/ --select ALL | grep -v -P '^(\e\[1;38| |\s*$)'
```

Ruff 0.6.4 (starting from 0.5.0).

---

_Comment by @MichaReiser on 2024-09-10 17:09_

Hi @sterliakov 

That makes sense to me. Does `ruff check --output-format=concise` work for you ([docs](https://docs.astral.sh/ruff/settings/#output-format))?

---

_Label `question` added by @MichaReiser on 2024-09-10 17:09_

---

_Comment by @sterliakov on 2024-09-10 17:17_

Sorry, I swear I looked through `ruff help format` several times... Works
for me, thanks!

Aside, can I ask `ruff` to avoid checking whether its stdout is attached to
a TTY and keep colorizing the output (e.g. `--color=always` or
`--force-color`)? Would be nice to `tail`/`head`/`grep` its output without
the `unbuffer` trick. This is something I expect from any unix CLI tool, to
be honest - perhaps I'm also just unable to find that in the docs?

On Tue, Sep 10, 2024, 19:09 Micha Reiser ***@***.***> wrote:

> Hi @sterliakov <https://github.com/sterliakov>
>
> That makes sense to me. Does ruff check --output-format=concise work for
> you (docs <https://docs.astral.sh/ruff/settings/#output-format>)?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/13311#issuecomment-2341512090>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AMBQIRD2ATKYZY3BLDPJBHLZV4RVTAVCNFSM6AAAAABN7FVL42VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGNBRGUYTEMBZGA>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @MichaReiser on 2024-09-10 17:32_

There's no CLI flag for this but you can set an environment variable. The documentation for it is a bit hard to find but see Ruff's [FAQ](https://docs.astral.sh/ruff/faq/#how-can-i-disableforce-ruffs-color-output)

---

_Comment by @sterliakov on 2024-09-10 20:54_

Amazing, thanks, that's it!

---

_Closed by @sterliakov on 2024-09-10 20:54_

---
