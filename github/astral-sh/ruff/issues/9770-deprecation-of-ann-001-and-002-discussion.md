---
number: 9770
title: Deprecation of ANN 001 and 002 Discussion
type: issue
state: closed
author: max-muoto
labels: []
assignees: []
created_at: 2024-02-02T02:15:19Z
updated_at: 2024-02-04T18:30:10Z
url: https://github.com/astral-sh/ruff/issues/9770
synced_at: 2026-01-07T13:12:15-06:00
---

# Deprecation of ANN 001 and 002 Discussion

---

_Issue opened by @max-muoto on 2024-02-02 02:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Wanted to start a discussion about the deprecation of `ANN001`, and `ANN002`. I have personally found these rules very helpful in gradually rolling out type-checking. The reasons why you would use them over a type-checker for enforcing these rules:

1. Ruff can give much quicker feedback than any type-checker that currently exists for Python. Having this instantaneous feedback that you're missing annotations can then help resolve issues quicker than simplify running your type-checker.
2. If you're using Pyright for enforcing type-safety. To my understanding, here's no effective way to use Pyright with Pycharm, meaning engineers using Pycharm don't have a great way to get quick feedback there right as they're writing code.

Personally, I think these rules are really helpful to help transition a codebase towards type-safety, or working with libraries that benefit from type hints. For example, in FastAPI, where query parameter types are enforced at runtime through argument annotations: https://fastapi.tiangolo.com/tutorial/query-params/. You might want part of your codebase under strict type-checking, and another part just with these rules (in addition to other ANN) rules, as you move towards enabling your type-checker there.

It also strikes me as odd, that these two rules would get removed, but the rest of the rule-set wouldn't also get gradually deprecated? I say this, because I feel if Ruff is going to keep rules like ANN 201-206, which help with enforcing return-types, why not also remove those? I get usage is likely more prevalent, and they offer return-type generation, but nonetheless.

There are also other rules remaining like ANN101 and ANN102, which in my opinion would be better candidates for deprecation. As with the `Self` return type, I'd argue there are very few instances where you want to manually annotate `self` or `cls`. I suppose in older versions these could be helpful, but still, 001 and 002 seem a lot more helpful than these.

Mostly curious if there might be the possibility of some reconsideration here, as I personally find these rules helpful. Or maybe others feel differently.

---

_Comment by @charliermarsh on 2024-02-02 02:19_

Hey sorry -- I think that's just a typo in the blog post. We deprecated [`ANN101`](https://docs.astral.sh/ruff/rules/missing-type-self/) and [`ANN102`](https://docs.astral.sh/ruff/rules/missing-type-cls/), the rules for annotating `self` and `cls` (which you mentioned at the end).

`ANN001` and `ANN002` are _not_ deprecated. I'll correct this in the blog post. If you saw references to these rules elsewhere, please let me know.


---

_Comment by @charliermarsh on 2024-02-02 02:19_

Ah, it's in the release notes too. Will fix.

---

_Comment by @max-muoto on 2024-02-02 02:20_

> Hey sorry -- I think that's just a typo in the blog post. We deprecated [`ANN101`](https://docs.astral.sh/ruff/rules/missing-type-self/) and [`ANN102`](https://docs.astral.sh/ruff/rules/missing-type-cls/), the rules for annotating `self` and `cls` (which you mentioned at the end).
> 
> `ANN001` and `ANN002` are _not_ deprecated. I'll correct this in the blog post. If you saw references to these rules elsewhere, please let me know.

Great to know! That makes a lot of sense! Didn't see this anywhere else, but I'll leave a comment if I do.

---

_Comment by @zanieb on 2024-02-02 02:24_

😮‍💨 Sorry I goofed it! Not the only rule I typo'd while working on this release — looking forward to using names instead of codes everywhere someday :)

---

_Closed by @zanieb on 2024-02-02 02:34_

---

_Comment by @joellidin on 2024-02-04 17:15_

Sorry if I am misunderstanding the blog post. But when reading this:
> They will not warn if selected by prefix (e.g., ANN).

I am reading that this should not warn without preview. However when I have this in my `pyproject.toml`:
```toml
[tool.ruff]
line-length = 88
target-version = "py310"

[tool.ruff.lint]
select = ["ANN"]
```
and running `ruff check` on this simple file:
```python
class A:
    @classmethod
    def b(cls) -> None:
        ...
```
I get this:
```console
src/test_ruff/test.py:3:11: ANN102 Missing type annotation for `cls` in classmethod
Found 1 error.
```
With the `--preview` flag I get no errors. Is this the expected behavior?

I am running on version 0.2.0 btw 

---

_Comment by @charliermarsh on 2024-02-04 18:13_

@joellidin -- Ah, by "warn", we mean, show a warning in the console indicating that the rule is deprecated. This is _different_ than the rule raising or not raising violations. For example, try using `select = ["ANN102"]` instead of `select = ["ANN"]`. In that case, you'll see:

```
warning: Rule `ANN102` is deprecated and will be removed in a future release.
bar.py:3:11: ANN102 Missing type annotation for `cls` in classmethod
Found 1 error.
```

---

_Comment by @joellidin on 2024-02-04 18:30_

Ah, understood! Thanks @charliermarsh 

---
