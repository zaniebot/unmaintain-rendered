```yaml
number: 18278
title: Is it possible to not highlight things that can be autofixed
type: issue
state: open
author: tolomea
labels:
  - wish
  - server
  - needs-design
assignees: []
created_at: 2025-05-23T14:29:26Z
updated_at: 2025-05-23T21:08:25Z
url: https://github.com/astral-sh/ruff/issues/18278
synced_at: 2026-01-12T15:54:56Z
```

# Is it possible to not highlight things that can be autofixed

---

_@tolomea_

### Question

I'm not sure if this is the right place to ask this.

I recently setup Ruff in Sublime via LSP. And this has mostly been a very positive experience.

But there's one thing I can't work out how to do. I have got it set to auto fix on save. Which means highlights / actions for stuff that's going to be autofixed is just distracting visual noise that I'd rather not have.

Is it possible to disable the check highlights for anything that can be autofixed?

### Version

0.11.10

---

_Label `question` added by @tolomea on 2025-05-23 14:29_

---

_Comment by @MichaReiser on 2025-05-23 20:34_

Not today.

Would you expect that the errors are still listed under diagnostics? Do you only want autofixable squiggles to be hidden?

---

_Comment by @tolomea on 2025-05-23 20:55_

I'd like the squiggles too go away, I'm not sure what you are referring to
by diagnostics

On Fri, May 23, 2025, 21:35 Micha Reiser ***@***.***> wrote:

> *MichaReiser* left a comment (astral-sh/ruff#18278)
> <https://github.com/astral-sh/ruff/issues/18278#issuecomment-2905753903>
>
> Not today.
>
> Would you expect that the errors are still listed under diagnostics? Do
> you only want autofixable squiggles to be hidden?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/18278#issuecomment-2905753903>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAGERUBHPCX72A6M5CIDMHT276BARAVCNFSM6AAAAAB5Y46P2KVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDSMBVG42TGOJQGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @MichaReiser on 2025-05-23 20:58_

I don't know if that's something that sublime text has but VS code has a panel where it shows all diagnostics similar to the diagnostics panel in the playground https://play.ruff.rs/b4146679-8459-45a7-9b8e-19a10676b4a4

---

_Comment by @tolomea on 2025-05-23 21:03_

I'm not sure if sublime has it, I haven't noticed it.
It'd be most consistent with my intent for them to not show there either.
Because they will be fixed on save I do not care about those warnings.
But them being there distracts from the ones I do need to care about.

On Fri, May 23, 2025, 21:59 Micha Reiser ***@***.***> wrote:

> *MichaReiser* left a comment (astral-sh/ruff#18278)
> <https://github.com/astral-sh/ruff/issues/18278#issuecomment-2905798437>
>
> I don't know if that's something that sublime text has but VS code has a
> panel where it shows all diagnostics similar to the diagnostics panel in
> the playground https://play.ruff.rs/b4146679-8459-45a7-9b8e-19a10676b4a4
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/18278#issuecomment-2905798437>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAGERUDDD6K6A2YCQUXETKL276D2FAVCNFSM6AAAAAB5Y46P2KVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDSMBVG44TQNBTG4>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Label `question` removed by @MichaReiser on 2025-05-23 21:08_

---

_Label `wish` added by @MichaReiser on 2025-05-23 21:08_

---

_Label `server` added by @MichaReiser on 2025-05-23 21:08_

---

_Label `needs-design` added by @MichaReiser on 2025-05-23 21:08_

---
