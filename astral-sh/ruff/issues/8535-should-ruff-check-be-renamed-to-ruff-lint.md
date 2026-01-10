```yaml
number: 8535
title: "Should `ruff check` be renamed to `ruff lint`?"
type: issue
state: closed
author: thernstig
labels:
  - cli
assignees: []
created_at: 2023-11-07T08:36:36Z
updated_at: 2024-02-23T21:46:25Z
url: https://github.com/astral-sh/ruff/issues/8535
synced_at: 2026-01-10T11:09:50Z
```

# Should `ruff check` be renamed to `ruff lint`?

---

_Issue opened by @thernstig on 2023-11-07 08:36_

This is a wild idea, but since Ruff is still in its `0.X` semver phase, maybe not a bad idea after all.

Should `ruff check` be renamed to `ruff lint`?

Since `ruff format` was introduced, the term `check` is harder to to understand what it does. In addition the website goes about mentioning the terms _format_ and _lint_. As evident by this:

![image](https://github.com/astral-sh/ruff/assets/30827238/8c83543b-c124-4e18-9d56-4816bb5c6c77)

It adds more "confusion" since you have `ruff format --check`. One could think it does both the format and the normal `ruff check`.


---

_Renamed from "Should `ruff check` be renamed to `ruff lint`" to "Should `ruff check` be renamed to `ruff lint`?" by @thernstig on 2023-11-07 08:36_

---

_Comment by @zanieb on 2023-11-07 14:50_

Thanks for raising. 

We're interested in making `ruff check` a unified command that checks for both lint and format errors #8232. We might want to _separately_ introduce a `ruff lint` command though — this seems as good a place as any for discussion on that.

---

_Label `cli` added by @zanieb on 2023-11-07 14:50_

---

_Comment by @T-256 on 2023-11-07 15:15_

Like `ruff format` that applies modifications by default, I suggest `ruff lint` do the same.
The current behavior of `--fix` would be default for `ruff lint` and can be disable by `--check`. (like `ruff format` we have today)

---

_Comment by @parafoxia on 2023-11-08 11:54_

I disagree that a `ruff lint` command should fix code by default. Ruff (afaik) is the only Python linter with the capability to fix issues, so may be confusing for people switching from something else. Linters in other languages, such as Cargo, tend not to fix by default either, so I don't believe there's a precedent for this. Additionally, the linter fixes aren't 100% safe to apply (see #8556 as an example).

Whether a unified `ruff check` should run something equivalent to `ruff lint --fix && ruff format` is another question though.

---

_Comment by @thernstig on 2023-11-08 17:31_

Linters I am aware of has a `--fix` for linting faults. Or e.g. Prettier (a formatter only) has `--write`. I think code transformations (which linters are) should not do fixes by default as @parafoxia mentions, due to the dangers of it. When users specifically has to add a `--fix`, they can check the changes made and are aware they are being made.

---

_Comment by @tmeiczin on 2023-11-09 16:20_

I agree, a `ruff lint` should identify, but not fix issues by default. There are times when the linter does not have enough context to determine if a change is appropriate and can potentially introduce a bug. In regard to formatting, I think `ruff format` is explicit in that I'm asking it to format my code and should do so by default.  I like the idea of `ruff check` identifying both lint and format, but not fixing and I kinda find `--fix` to be awkward. I would probably prefer a `ruff fix` command, but it's a balance between number of commands and options per command.

---

_Comment by @Avasam on 2023-12-16 20:35_

My thoughts:
`ruff lint`: only lints by default
`ruff format`: formats by default
`ruff check`: shorthand for `ruff lint && ruff format --check`
`ruff fix`: shorthand for `ruff lint --fix && ruff format`

---

_Comment by @MichaReiser on 2024-02-23 21:46_

I'll close this in favor of https://github.com/astral-sh/ruff/issues/8232 where potentially introducing a `ruff lint` command will come up. 

---

_Closed by @MichaReiser on 2024-02-23 21:46_

---
