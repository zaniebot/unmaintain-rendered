```yaml
number: 21099
title: "`ruff analyze graph` seems to skip notebooks"
type: issue
state: closed
author: gwdekker
labels:
  - wish
  - analyze
assignees: []
created_at: 2025-10-27T15:39:26Z
updated_at: 2025-10-31T21:47:02Z
url: https://github.com/astral-sh/ruff/issues/21099
synced_at: 2026-01-10T11:10:00Z
```

# `ruff analyze graph` seems to skip notebooks

---

_Issue opened by @gwdekker on 2025-10-27 15:39_

### Summary

It seems like `ruff analyze graph` only includes .py files but skips notebooks (.ipynb files) while plain ruff nowadays supports notebooks (https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery). 

### Version

_No response_

---

_Comment by @MichaReiser on 2025-10-27 16:03_

I've to jump into my next meeting so I can't try it right now but does ruff not list notebook files or are they listed but without any imports?

---

_Label `analyze` added by @MichaReiser on 2025-10-27 16:03_

---

_Comment by @gwdekker on 2025-10-27 16:11_

They are not listed at all.

On Mon, 27 Oct 2025 at 17:05, Micha Reiser ***@***.***> wrote:

> *MichaReiser* left a comment (astral-sh/ruff#21099)
> <https://github.com/astral-sh/ruff/issues/21099#issuecomment-3452085066>
>
> I've to jump into my next meeting so I can't try it right now but does
> ruff not list notebook files or are they listed but without any imports?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/21099#issuecomment-3452085066>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AI3UPMKJ7U62DQITLPVPIZ33ZYYB3AVCNFSM6AAAAACKK2SGWWVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTINJSGA4DKMBWGY>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @MichaReiser on 2025-10-27 16:42_

I was surprised this behavior because analyze uses the same logic to discover all python files as all other commands but it then also does this:

https://github.com/astral-sh/ruff/blob/d9cab4d242a18102617d2a9eea1fc10a70c67a50/crates/ruff/src/commands/analyze_graph.rs#L130-L133

I'm not sure why it's there. Do you want to try to remove it and see if it just works?

It seems reasonable to have a way to exclude notebooks for people who don't care about them but that's what `analyze.exclude` can be used for.

---

_Label `wish` added by @MichaReiser on 2025-10-27 16:42_

---

_Comment by @gwdekker on 2025-10-27 20:17_

I tried. As a result after commenting out the lines and running `cargo run -p ruff -- analyze graph --python ...`
shows that now the notebook files show up as keys in the resulting mapping - however, the values are all empty lists. So I think a bit more work is needed to make this actually work.

Disclaimer: this is the very first thing I ever did with rust and cargo so no guarantees i did everything right.

---

_Closed by @MichaReiser on 2025-10-31 21:47_

---
