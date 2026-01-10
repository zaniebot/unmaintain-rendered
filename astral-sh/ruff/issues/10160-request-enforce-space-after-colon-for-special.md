```yaml
number: 10160
title: "Request: Enforce space after colon (`:`) for special comments"
type: issue
state: open
author: Avasam
labels:
  - rule
assignees: []
created_at: 2024-02-29T06:12:01Z
updated_at: 2024-07-14T19:55:12Z
url: https://github.com/astral-sh/ruff/issues/10160
synced_at: 2026-01-10T11:09:52Z
```

# Request: Enforce space after colon (`:`) for special comments

---

_Issue opened by @Avasam on 2024-02-29 06:12_

I've open the same request over at https://github.com/PyCQA/flake8-pyi/issues/465 , but I think that Ruff could generalize this, or at least expand on supported keywords. 

Comments handled by Ruff
`# noqa:Y011` --> `# noqa: Y011` (not handled by flake8-noqa https://github.com/astral-sh/ruff/issues/850 )
`# fmt:off` --> `# fmt: off` (except if this is added to a formatter)
`# ruff:noqa` --> `# ruff: noqa`
`# ruff:noqa:F811` --> `# ruff: noqa: F811` (or at least `# ruff: noqa:F811` if generalized)
`# flake8:noqa:F811` --> `# flake8: noqa: F811` (or at least `# flake8: noqa:F811` if generalized)
`# ruff:isort:skip_file` --> `# ruff: isort: skip_file` (or at least `# ruff: isort:skip_file` if generalized)
`# isort:skip_file` --> `# isort: skip_file`

Not handled by Ruff, but popular:
`# type:ignore` --> `# type: ignore`
`# pyright:ignore` --> `# pyright: ignore`
`# nopycln:import` --> `# nopycln: import`
...

Basically the same as https://docs.astral.sh/ruff/rules/missing-space-after-todo-colon/ but with a lot more keywords

Formatters (black and Ruff) already take of adding a space after `#` 

---

_Label `rule` added by @AlexWaygood on 2024-02-29 07:13_

---

_Comment by @MichaReiser on 2024-02-29 07:37_

@JelleZijlstra what do you think of adding this to Black/Ruff's formatter in addition or instead of a lint rule? 

---

_Comment by @JelleZijlstra on 2024-02-29 07:45_

Yes, I was thinking about that. I feel it goes against the spirit of Black's AST safety check if we modify comments (it doesn't actually violate the check, of course, because comments aren't part of the AST). That concern becomes bigger the bigger the set of comments is for which we apply the transformation.

I think I'd be OK with normalizing whitespace in `# type:` comments in Black, because those are a standardized part of the language. I'd be hesitant to change `# fmt:off` comments because the point of those is to make Black *not* change things. With other tool-specific comments, I'd be a bit hesitant because we'd need to encode specific knowledge of what each tool accepts.

---

_Comment by @AlexWaygood on 2024-02-29 07:56_

> With other tool-specific comments, I'd be a bit hesitant because we'd need to encode specific knowledge of what each tool accepts.

We'd need to do that for a ruff lint rule as well though, right?

---

_Comment by @MichaReiser on 2024-02-29 07:58_

> Yes, I was thinking about that. I feel it goes against the spirit of Black's AST safety check if we modify comments (it doesn't actually violate the check, of course, because comments aren't part of the AST). That concern becomes bigger the bigger the set of comments is for which we apply the transformation.
I'm personally less concerned about that, but I know it is an important property of/for Black. What you mean by that is that it doesn't change the representation as Python understands the AST? I'm saying that because our AST has much more details than Python's, e.g. we represent implicit concatenated strings and Black's unstable string-processing changes our AST (but presumably not Python's). One exception that Ruff already does here today is that it formats code blocks in docstrings.

> I'd be hesitant to change # fmt:off comments because the point of those is to make Black not change things.

That makes sense. Although I could see us formatting it as well, considering that [they get correctly indented today](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACiAGNdAD2IimZxl1N_WlbvK5V_5PM4Nceez9J8N3C1qIg8e7k4xLRet8g5rDCr1ZTcLnwPqqNluQY86f5RteS3yDZKrumMMSNP1D3BaaIGngwC75nTfhipsVg_PwJW6isSDNnkYGp7AAAAIFUG6pwdLrEAAX-jAQAAAOrpGraxxGf7AgAAAAAEWVo=). 

> I'd be a bit hesitant because we'd need to encode specific knowledge of what each tool accepts

That's fair. I'm not too familiar with any of those tools so it's hard for me to judge how common it is that the suppression comment format changed in the past. One argument I see against it is that Black so far has avoided very specific formatting rules to keep its formatting predictable (which I consider a key difference to Prettier, which has tons of very specific rules). Adding these tool-specific formatting rules goes against this.

> We'd need to do that for a ruff lint rule as well though, right?

The difference that I see here is that you can opt out of the linter in case your tool changes in an incompatible way but you can't really do this in the formatter.

---

_Comment by @JelleZijlstra on 2024-02-29 08:01_

> We'd need to do that for a ruff lint rule as well though, right?

As @MichaReiser said, a lint rule is more granular while a formatter is pretty much all-or-nothing. (At least, Black is; I believe your formatter is a little more configurable.) With a lint rule you could add a configuration option to allow people to control what set of comments get formatted.

---

_Comment by @Avasam on 2024-02-29 16:01_

> you could add a configuration option to allow people to control what set of comments get formatted.

Good point. Ruff could have defaults for all the special comments it supports (see my list above) and let users expand with their other tools (up to maintainers if they want to support other popular tool comments by default)

With regex (maybe glob?) support, this could technically superseed https://docs.astral.sh/ruff/rules/missing-space-after-todo-colon/ entirely.

---

_Comment by @egberts on 2024-07-08 22:01_

Latest PyCharm fights with RUFF over their preferred format of RUF100 with this single space between RUF100 and colon symbol.

PyCharm only recognizes this format but RUF complains:
```Python
call_me()  # NOQA : RUF100
```

RUF reformats as :
```Python
call_me()  # NOQA: RUF100
```
but PyCharm complains:

Nobody is happy.

---

_Comment by @dhruvmanila on 2024-07-09 04:13_

@egberts Can you say more about this? What do you mean by "PyCharm only recognizes this format (`NOQA : RUF100`)" ?

<img width="678" alt="Screenshot 2024-07-09 at 09 42 50" src="https://github.com/astral-sh/ruff/assets/67177269/26470b48-dc93-45ca-a652-508743b1c378">


---

_Comment by @egberts on 2024-07-14 18:56_

It's a battle between RUFF and PyCharm,  one wants a space between the NOQA and the semicolon, the other doesn't.

After letting RUFF making the fix, PyCharm complains again.  Comply with PyCharm and RUFF  fix it up right back.

Who is correct?  We don't care, we just want consistency and concurrent agreement.

---

_Comment by @charliermarsh on 2024-07-14 19:55_

I think the confusion on my end is that I've never seen PyCharm enforce this and I use it daily. I would be extremely surprised if PyCharm required a space before a colon here. Are you sure you don't have some specific configuration that's causing this?

---
