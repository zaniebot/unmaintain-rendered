```yaml
number: 8183
title: disable Force color over no color
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: bug/8173-force-color
created_at: 2024-10-14T18:28:22Z
updated_at: 2024-10-15T13:28:05Z
url: https://github.com/astral-sh/uv/pull/8183
synced_at: 2026-01-12T16:08:12Z
```

# disable Force color over no color

---

_@Aditya-PS-05_

closes #8173 


---

_@zanieb reviewed on 2024-10-14 18:31_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:84 on 2024-10-14 18:31_

Won't this always match since the default is "auto" at the clap-level?

---

_@Aditya-PS-05 reviewed on 2024-10-14 18:36_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:84 on 2024-10-14 18:36_

Yupp, But I thought if `--color` is passed explicitly, we can use the value of `args.color` directly.

---

_@Aditya-PS-05 reviewed on 2024-10-14 18:39_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:84 on 2024-10-14 18:39_

Also, when I tested the expected result was coming for all three separate cases. Means for `FORCE_COLOR` and `--color` and with sub arguments.

---

_Comment by @samypr100 on 2024-10-14 18:42_

Unless I'm missing something, I believe this changes standard behavior since env var should still take priority over cli when honoring the order of `no color > force color | force cli color > auto`

For example, if you have `NO_COLOR` set and pass `--color always` as well, `NO_COLOR` should take precedence.

---

_@Aditya-PS-05 reviewed on 2024-10-14 19:37_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:84 on 2024-10-14 19:37_

> Won't this always match since the default is "auto" at the clap-level?

Done purposely, as when the user does not anything (neither the environment variables nor the --color), then I wanted the auto property. As If `--color` is given, it doesn't matter the value, we can always use them. 

---

_Comment by @zanieb on 2024-10-14 20:08_

> if you have NO_COLOR set and pass --color always as well, NO_COLOR should take precedence.

Should it? Honestly not sure.

---

_Comment by @samypr100 on 2024-10-14 20:24_

> > if you have NO_COLOR set and pass --color always as well, NO_COLOR should take precedence.
> 
> Should it? Honestly not sure.

Fair, I was considering CI scenarios when I said this. Wouldn't this be a breaking if changed?

---

_Comment by @charliermarsh on 2024-10-14 20:25_

The docs (https://no-color.org/) at least state that "per-instance command-line arguments should override the NO_COLOR environment variable".

---

_Comment by @samypr100 on 2024-10-14 20:27_

> The docs (https://no-color.org/) at least state that "per-instance command-line arguments should override the NO_COLOR environment variable".

Does the same apply for FORCE_COLOR? Or CLICOLOR_FORCE?

---

_Comment by @samypr100 on 2024-10-14 20:31_

> > The docs (https://no-color.org/) at least state that "per-instance command-line arguments should override the NO_COLOR environment variable".
> 
> Does the same apply for FORCE_COLOR? Or CLICOLOR_FORCE?

Sounds like it does at least for `CLICOLOR_FORCE`, https://bixense.com/clicolors/
`FORCE_COLOR` doesn't say.

I've likely been using it wrong 😆

To me, its a bit vague since the relationship between `NO_COLOR` and `FORCE_COLOR` and `CLICOLOR_FORCE` in this scenario is not well defined in any of the standards from what I can see.

Sounds like `uv` should follow this then?

1. Explicit CLI `--color never` / `--color always` takes precedence over the env vars.
2. Env vars then kick-in, `NO_COLOR` > `FORCE_COLOR` | `CLICOLOR_FORCE`
3. Auto Detection

What about an explicit `--color auto`? It's not clear which one would apply in the presence of the env vars being set.


---

_Comment by @charliermarsh on 2024-10-14 23:36_

Yeah, agree with this @samypr100:

- Explicit CLI --color never / --color always takes precedence over the env vars.
- Env vars then kick-in, NO_COLOR > FORCE_COLOR | CLICOLOR_FORCE
- Auto Detection

I think `--color=auto` should also override the env vars (https://bixense.com/clicolors/ shows an example of that, arguably).


---

_Comment by @charliermarsh on 2024-10-15 12:26_

@Aditya-PS-05 - Do you plan to pursue the follow-up changes mentioned here?

---

_Comment by @Aditya-PS-05 on 2024-10-15 12:29_

> Yeah, agree with this @samypr100:
> 
>     * Explicit CLI --color never / --color always takes precedence over the env vars.
> 
>     * Env vars then kick-in, NO_COLOR > FORCE_COLOR | CLICOLOR_FORCE
> 
>     * Auto Detection
> 
> 
> I think `--color=auto` should also override the env vars (https://bixense.com/clicolors/ shows an example of that, arguably).

@charliermarsh 
I am making this change following your instructions.

---

_Closed by @Aditya-PS-05 on 2024-10-15 13:02_

---

_Comment by @Aditya-PS-05 on 2024-10-15 13:28_

All the required changes are done in the pr #8215

---
