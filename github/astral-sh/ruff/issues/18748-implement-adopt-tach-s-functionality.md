---
number: 18748
title: "Implement/adopt `tach`s functionality?"
type: issue
state: open
author: gwdekker
labels:
  - plugin
  - wish
  - needs-design
assignees: []
created_at: 2025-06-18T13:03:13Z
updated_at: 2025-06-19T14:47:36Z
url: https://github.com/astral-sh/ruff/issues/18748
synced_at: 2026-01-07T13:12:16-06:00
---

# Implement/adopt `tach`s functionality?

---

_Issue opened by @gwdekker on 2025-06-18 13:03_

### Summary

I have a large request. In my company we are using [tach](https://github.com/gauge-sh/tach) which has been extremely helpful to control and manage dependencies in our monorepo. I also remember vaguely @charliermarsh one time posting a demo of I believe an output of `ruff` that was able to show/map all interdependencies in the code base (which I would guess you are building up internally anyway to be able to power `ruff`). I imagine going from there to what `tach` offers might not be a very large stretch. Now, it appears that the startup behind tach has pivoted and tach is no longer maintained. 

The scope of tach seems to my eyes to fit very well with astral's scope/ambition. I am very curious how you guys feel about this, and whether this is something you would consider at all?

Disclaimer: I am just a happy user of tach (and ruff, and uv), not affiliated with them in any way.

---

_Label `wish` added by @MichaReiser on 2025-06-18 13:57_

---

_Label `type-inference` added by @MichaReiser on 2025-06-18 13:57_

---

_Comment by @MichaReiser on 2025-06-18 14:00_

Hi. Thanks for proposing this idea.

Ruff has an `analyze` command that allows you to extract some of that information into a JSON. But it currently doesn't provide a way to enforce this in any way. Some form of report would definetely be nice so that not every user has to build their own visualization.

I'm not quiet sure I fully understand what kind of errors are that tach checks for but it doesn't sound unreasonable to add similar checks to ruff (optional). 





---

_Label `type-inference` removed by @MichaReiser on 2025-06-18 14:00_

---

_Label `plugin` added by @MichaReiser on 2025-06-18 14:00_

---

_Label `needs-design` added by @MichaReiser on 2025-06-18 14:00_

---

_Comment by @gwdekker on 2025-06-19 12:03_

Great! If it would help in any way I would be very much willing to give a demo or have a discussion on what I consider the key features of this tool and how we use it!

---

_Comment by @MichaReiser on 2025-06-19 14:26_

Thank you. That's very kind. It will probably be a while before we have the capacity to increase our scope. Building a type checker is surprisingly a lot of work (okay, we sort of knew what we signed up for but it's still a lot of work 😅 )

---

_Comment by @gwdekker on 2025-06-19 14:47_

Of course! Rooting for you guys, can’t wait for ty!

On Thu, 19 Jun 2025 at 16:27, Micha Reiser ***@***.***> wrote:

> *MichaReiser* left a comment (astral-sh/ruff#18748)
> <https://github.com/astral-sh/ruff/issues/18748#issuecomment-2988292213>
>
> Thank you. That's very kind. It will probably be a while before we have
> the capacity to increase our scope. Building a type checker is surprisingly
> a lot of work (okay, we sort of knew what we signed up for but it's still a
> lot of work 😅 )
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/18748#issuecomment-2988292213>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AI3UPMOD4AUU4ZBLVZPCODT3ELCDJAVCNFSM6AAAAAB7TBZ532VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDSOBYGI4TEMRRGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---
