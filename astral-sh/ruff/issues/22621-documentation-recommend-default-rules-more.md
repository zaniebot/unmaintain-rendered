```yaml
number: 22621
title: "[documentation] Recommend default rules more clearly, and more easily findable"
type: issue
state: open
author: k0pernikus
labels:
  - documentation
assignees: []
created_at: 2026-01-16T13:11:20Z
updated_at: 2026-01-16T13:54:20Z
url: https://github.com/astral-sh/ruff/issues/22621
synced_at: 2026-01-16T13:56:58Z
```

# [documentation] Recommend default rules more clearly, and more easily findable

---

_@k0pernikus_

I, as an experienced developer but newbie in both python ecosystem and ruff itself, had some difficulty parsing the documentation for recommended rule sets.

I assumed I could just use ALL and have a meaningful default in my `pyproject.toml`:

```
[tool.ruff.lint]
select = ["ALL"]
```

Yet this opened a cans of worms as suddenly I noticed that certain rules were mutually exclusive with each other and I had to disable some of them, (others were just my personal choice), so I ignored these:

```
ignore = [
    "D203",
    "D213",
    "COM812",
    "CPY001",
    "D100",
    "D101",
    "D102",
    "D103",
    "D104",
    "D107"
]

[tool.ruff.lint.per-file-ignores]
"**/tests/*" = ["S101"]
"**/bin/somescript.py" = ["T201"]
```

While researching, you do recommend some rules yet with the notion of "this might work for you" at

https://docs.astral.sh/ruff/tutorial/#rule-selection

> For example, a configuration that enables some of the most popular rules (without being too pedantic) might look like the following:
> 
> ```
> [lint]
> select = [
>     # pycodestyle
>     "E",
>     # Pyflakes
>     "F",
>     # pyupgrade
>     "UP",
>     # flake8-bugbear
>     "B",
>     # flake8-simplify
>     "SIM",
>     # isort
>     "I",
> ]
> ````

So is this what you recommend people to start with?
What do you recommend for people that want to be pedantic?  (Yet do not want to deal with the mutual exclusive rules that get imported via `ALL`)?

(Maybe there might even be a use case for a couple of predefined groups next to `ALL`, like `RECOMMENDED`, `WEAK`, `PEDANTIC`, though not part of my issue here. This is more about the developer experience of finding default / recommended settings fast.)

Basically, I am asking if you could please improve the documentation in this regard, and also highlight a recommended ruleset at best on your [main page](https://docs.astral.sh/ruff/) or move that part to a sort of quick-start guide right there: https://docs.astral.sh/ruff/tutorial/

It's also confusing that at the top of the tutorial you link to 

https://docs.astral.sh/ruff/configuration/

yet that part does not directly lead to the rule's selection page, even though for me, this is part of its config.

To showcase this:

<img width="2221" height="1109" alt="Image" src="https://github.com/user-attachments/assets/5f1dc086-247f-45f9-8d18-bba77a4ec997" />

Updating the docs to better handle the reading flow may help ease some friction setting ruff up.

Thank you for the tool and the documentation that you already created.

---

I also created a related [open ended question on stackoverflow](https://stackoverflow.com/questions/79866070/is-there-a-recommended-pedantic-rule-set-for-ruff-for-python-code-style) before



---

_Comment by @ntBre on 2026-01-16 13:54_

Thanks for the kind words and the nice write-up! I am coincidentally working on a significantly expanded set of default rules right now, so this is definitely on our mind. We do also want to introduce categories along the lines you mentioned, as tracked in #1774.

---

_Label `documentation` added by @ntBre on 2026-01-16 13:54_

---
