```yaml
number: 2044
title: "Consider behavior when `VIRTUAL_ENV` is set to a missing directory"
type: issue
state: open
author: zanieb
labels:
  - cli
  - needs-decision
assignees: []
created_at: 2025-12-17T23:37:33Z
updated_at: 2025-12-18T17:29:00Z
url: https://github.com/astral-sh/ty/issues/2044
synced_at: 2026-01-10T01:53:59Z
```

# Consider behavior when `VIRTUAL_ENV` is set to a missing directory

---

_Issue opened by @zanieb on 2025-12-17 23:37_

This is pulled out of https://github.com/astral-sh/ty/issues/2031 which I'd prefer to keep scoped to the panic in the language server. A couple thoughts were expressed there, e.g., https://github.com/astral-sh/ty/issues/2031#issuecomment-3667543854

Today, we error when `VIRTUAL_ENV` refers to a missing path. There's not a clear consensus on whether or not we should continue to other methods of Python environment discovery. If we ignore an invalid `VIRTUAL_ENV` value, we should probably warn?

I don't think we need to act on this urgently, but I'd like to have a central place for discussion.


---

_Label `cli` added by @zanieb on 2025-12-17 23:37_

---

_Label `needs-decision` added by @zanieb on 2025-12-17 23:37_

---

_Comment by @MichaReiser on 2025-12-18 07:24_

The reason why I think it's related to #2031 is that it would be the most straightforward fix

---

_Comment by @AlexWaygood on 2025-12-18 15:24_

Pasting my comment from #2031 (now closed):

> I think I'd much prefer a hard exit when invoked on the CLI. I don't see a use case for setting an invalid `VIRTUAL_ENV` from the CLI and I'd want to fix that immediately as a user, since the type-checking results could make very little sense in that situation.
>
> The LSP case seems different; I think logging a warning and continuing could make sense there

@zanieb responded:

> I'm not sure if I agree. `uv pip install` doesn't hard exit here — and from experience there it's unfortunately common for people to have invalid `VIRTUAL_ENV` variables.

I'm curious how so many people end up with invalid `VIRTUAL_ENV` variables 😆 And I'd prefer to avoid implicit behaviour where possible for ty, so that it's clear why we're producing the results that we are.

But even putting that aside, to me, the `uv pip` case still sort-of feels pretty different? If `VIRTUAL_ENV` points to a nonexistent directory, it still seems like there's a pretty good chance that they meant to just install the thing into their local virtual environment, so carrying on as normal seems rational there. But for the ty case, there's a good chance that we just won't find a local virtual environment at all, and carrying on in that case despite a broken `VIRTUAL_ENV` variable seems like it could be really confusing. You could end up with hundreds of `unresolved-import` diagnostics suddenly appearing (all of which would be "duplicates" if there's just a single cause), and the warning about the broken `VIRTUAL_ENV` variable would be miles higher in the terminal.

I still think it's just a much better UX if we have a hard early exit for the CLI case here.

---

_Comment by @zanieb on 2025-12-18 16:24_

> But for the ty case, there's a good chance that we just won't find a local virtual environment at all, and carrying on in that case despite a broken VIRTUAL_ENV variable seems like it could be really confusing. 

Why are we any less likely to find a local virtual environment in ty than in uv?

---

_Comment by @AlexWaygood on 2025-12-18 16:25_

I don't think we're less likely, but I think the behaviour is much more confusing and annoying in the situation where you don't find a virtual environment. We'll just carry on type checking all your code even if we don't find a virtual environment (which I think is correct, you shouldn't _have_ to have a virtual environment setup in order to run a type checker), but our results are very likely to be useless

---

_Comment by @zanieb on 2025-12-18 16:47_

But in contrast, for uv you said

> it still seems like there's a pretty good chance that they meant to just install the thing into their local virtual environment

I don't see how the chances are different here?

I think the argument at

>  We'll just carry on type checking all your code even if we don't find a virtual environment (which I think is correct, you shouldn't have to have a virtual environment setup in order to run a type checker)

is more compelling — in the sense that uv will fail if it does not find a virtual environment.

However, I think it also is an argument for not erroring... in the sense that if ty's _general_ behavior is to continue if a virtual environment is not found, it's surprising that setting `VIRTUAL_ENV` would prevent it from doing so? The issue of continuing to type check and report useless diagnostics is a more fundamental choice ty has made.

---

_Comment by @AlexWaygood on 2025-12-18 16:50_

the fact that `VIRTUAL_ENV` is set seems pretty explicit that the user _wants_ us to find a virtual environment. But we can't figure out what virtual environment it is that they want us to find!

We haven't received any user reports yet that they find our current CLI behaviour bad. I would be inclined to keep it as-is until we receive any complaints

---

_Comment by @zanieb on 2025-12-18 16:53_

> We haven't received any user reports yet that they find our current CLI behaviour bad. I would be inclined to keep it as-is until we receive any complaints

I'm not saying we should change it without hearing more feedback, per my first post:

> I don't think we need to act on this urgently, but I'd like to have a central place for discussion.

> the fact that VIRTUAL_ENV is set seems pretty explicit that the user wants us to find a virtual environment.

It depends how explicit this actually is in practice :) environment variables are often quite challenging to tie to explicit user intent, i.e., opposed to command-line flags.

---

_Comment by @MichaReiser on 2025-12-18 17:26_

> It depends how explicit this actually is in practice :) environment variables are often quite challenging to tie to explicit user intent, i.e., opposed to command-line flags.

Yeah, VS Code and Zed both set them and I find that very confusing.

---

_Comment by @AlexWaygood on 2025-12-18 17:29_

> Yeah, VS Code and Zed both set them and I find that very confusing.

Agreed

---
