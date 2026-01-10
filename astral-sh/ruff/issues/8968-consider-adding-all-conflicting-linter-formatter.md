---
number: 8968
title: Consider adding «ALL_CONFLICTING_LINTER_FORMATTER_RULES» code
type: issue
state: closed
author: sam-hosseini
labels: []
assignees: []
created_at: 2023-12-02T16:32:55Z
updated_at: 2023-12-04T00:24:57Z
url: https://github.com/astral-sh/ruff/issues/8968
synced_at: 2026-01-10T01:22:48Z
---

# Consider adding «ALL_CONFLICTING_LINTER_FORMATTER_RULES» code

---

_Issue opened by @sam-hosseini on 2023-12-02 16:32_

Hello!

Thanks again for this 😊

I wanted to ask if you'd consider adding a code for all the rules specified in `Conflicting lint rules` [here](https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules).

The reason I'm asking is because I'm assuming many folks ([here](https://github.com/astral-sh/ruff/issues/8185#issuecomment-1778083766)/[here](https://www.reddit.com/r/Python/comments/15o73kq/comment/jvrrlxa/?context=3)/[here](https://www.reddit.com/r/Python/comments/15o73kq/comment/jvqnkzb/?context=3)), myself included, use ruff to take advantage of the 700+ built-in rules. This motivates me to set `ALL` in my `select` setting and selectively disable what I don't want.

However, when adding the formatter into the mix, we have to manually ignore all the rules that conflict with the linter (14 as of now) and make sure to update this in the future in upgrades.

Now, my config looks like this:
```toml
[tool.ruff.lint]
select = ["ALL"]
ignore = [
    ###############################################################
    # https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules
    ###############################################################
    "COM812",
    "COM819",
    "D206",
    "D300",
    "E111",
    "E114",
    "E117",
    "ISC001",
    "ISC002",
    "Q000",
    "Q001",
    "Q002",
    "Q003",
    "W191",
]
```
I think it'd be much better and concise if I could just do this:

```toml
[tool.ruff.lint]
select = ["ALL"]
ignore = ["ALL_CONFLICTING_LINTER_FORMATTER_RULES"]
```

And this «ALL_CONFLICTING_LINTER_FORMATTER_RULES» (or a better name) would be the list of 14 conflicting rules that is maintained by the library. What do you think? 😊

---

_Comment by @MichaReiser on 2023-12-04 00:24_

Thanks for this suggestion. We're currently considering the following options

* Category with all conflicting rules as you're proposing
* A new option to disable the warnings 
* Changing the semantics of all / recategorising the rules: Have fewer categories like `correctness`, `style`, `suspicious`, `format` (includes all conflicting rules) that allows easier opt-in/opt-out rather than listing plenty of rules or rule-groups. 

Our preferred option for now is to re-categorise the rules because we don't recommend using all `ALL` rules because we believe that many rules in ALL are too opinionated in and don't necessary help catch issues / improve productivity. But we understand that many use it because Ruff lacks a better default set. 

These options are not strictly exclusive. E.g. some users want to use the conflicting lint rules together with the formatter because they haven't run into any problems in practice. 

I'll close this issue and you can see our progress in https://github.com/astral-sh/ruff/issues/8175


---

_Closed by @MichaReiser on 2023-12-04 00:24_

---
