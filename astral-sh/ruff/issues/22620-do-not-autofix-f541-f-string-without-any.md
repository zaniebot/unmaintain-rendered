```yaml
number: 22620
title: "Do not autofix F541 `f-string without any placeholders` / mark as risky"
type: issue
state: open
author: k0pernikus
labels:
  - fixes
  - needs-decision
assignees: []
created_at: 2026-01-16T12:36:06Z
updated_at: 2026-01-19T12:52:14Z
url: https://github.com/astral-sh/ruff/issues/22620
synced_at: 2026-01-19T13:34:18Z
```

# Do not autofix F541 `f-string without any placeholders` / mark as risky

---

_@k0pernikus_

### Summary

Given this code:

```python
class Greeter:
    def hello(self, name: str) -> str:
        return f"Hello name!"
```

ruff will correctly report:

```shell
F541 [*] f-string without any placeholders
 --> utils\src\utils\greeting.py:3:16
  |
1 | class Greeter:
2 |     def hello(self, name: str) -> str:
3 |         return f"Hello name!"
  |                ^^^^^^^^^^^^^^
  |
```

Yet its autofix hides the bug the developer made:

```
help: Remove extraneous `f` prefix

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

It doesn't really help that it does not produce an output:

```shell
ruff check --fix
Found 1 error (1 fixed, 0 remaining).
```

File will now contain:

```python
class Greeter:
    def hello(self, name: str) -> str:
        return "Hello name!"
```

----

This is my `ruff.toml`:

```toml
target-version = "py314"
line-length = 88
indent-width = 4

[lint]
select = [
    "E",
    "F",
    "UP",
    "B",
    "SIM",
    "I",
    "W",
]
ignore = []

[lint.isort]
combine-as-imports = true
lines-after-imports = 2

[format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
```

---

Small unit test for the Greeter:

```python
def test_hello_world():
    greeter = Greeter()
    msg = greeter.hello("User")

    assert msg == "Hello User!"
```

The code as was intended by the devloper:

```python
class Greeter:
    def hello(self, name: str) -> str:
        return f"Hello {name}!"
```

---

This might also relate to how [ARG001](https://docs.astral.sh/ruff/rules/unused-function-argument/), [ARG002](https://docs.astral.sh/ruff/rules/unused-method-argument/) are not part of the recommended rule set, but `F` is:


> For example, a configuration that enables some of the most popular rules (without being too pedantic) might look like the following:
> 
> ```toml
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
> ```

-- [ruff on rule selection](https://docs.astral.sh/ruff/linter/#rule-selection)





---

_Comment by @k0pernikus on 2026-01-16 12:43_

I added [`extend-unsafe-fixes`](https://docs.astral.sh/ruff/settings/#lint_extend-unsafe-fixes) for that rule in my project's `ruff.toml`:

```toml
[lint]
select = [
    "E",
    "F",
    "UP",
    "B",
    "SIM",
    "I",
    "W",
]

ignore = []

extend-unsafe-fixes = ["F541"]
```

That works for me:

```shell
ruff check --fix 
F541 f-string without any placeholders
 --> utils\src\utils\greeting.py:3:16
  |
1 | class Greeter:
2 |     def hello(self, name: str) -> str:
3 |         return f"Hello name!"
  |                ^^^^^^^^^^^^^^
  |
help: Remove extraneous `f` prefix

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

It is also really great that you have also included the `--unsafe-fixes`-flag!


---

_Comment by @ntBre on 2026-01-16 14:02_

Thanks for the report! Yeah, I think this should probably be an unsafe fix by default since the intention of the original code isn't clear.

---

_Label `fixes` added by @ntBre on 2026-01-16 14:02_

---

_Comment by @MichaReiser on 2026-01-19 09:29_

I'm not sure we should make this change. `F541` is a stylistic rule. It's not a correctness rule. 

Given that the rule doesn't judge the code's correctness and is only enforcing a specific style, applying that stylistic change doesn't make the code any more or less correct. But it does make the code more consistent with the style that the project wants to enforce. In that sense, it's very clear what the users' intentions are; strings without placeholders should not use f-strings. Making the fix unsafe also makes it less useful overall, since it now requires explicit opt-in, which many users are unlikely to do.



---

_Comment by @AlexWaygood on 2026-01-19 11:29_

Hmm @MichaReiser I don't agree that F541 is a stylistic rule. The original pyflakes linter had very strict criteria for adding rules: rules could only be added if they were almost certain to indicate a likely bug in the code. Sometimes it might be the case that you really did just mean to write a regular string and you didn't need the `f` prefix, but I agree with OP that it seems just as likely that you _meant_ to put placeholders somewhere in the f-string and forgot to do so. In that way, the rule is effective at finding subtle bugs in your code where you're not creating the strings that you meant to at all.

I'm sympathetic to OP's argument here that this autofix can be counterproductive -- it can just hide the underlying bug, if there is one, which often isn't what you'd want -- and that therefore it should be explicitly opt-in. Though this doesn't really correspond to our usual understanding of "fix safety" -- the fix is certainly _safe_, as it doesn't change runtime behaviour.

---

_Comment by @MichaReiser on 2026-01-19 11:45_

> to indicate a likely bug in the code

This very much depends on how you intend to use f-strings. I could totally see myself using f-strings everywhere, and there's nothing wrong with that. That's why I'd consider `F541` either a stylistic or a restriction lint. It may occasionally catch bugs, but I still think it's more about consistency, e.g., forgetting to remove a `f` prefix after I removed some placeholders. 

I agree that there will be instances where the fix is undesired. But I don't think always making the fix as unsafe is the right approach here. Instead, we could consider to be clever about when the fix is likely unsafe. E.g, can we split the string by whitespace and are there any symbols in scope that match a word in the string? If so, mark the fix as unsafe.



---

_Label `needs-decision` added by @MichaReiser on 2026-01-19 11:45_

---

_Comment by @AlexWaygood on 2026-01-19 11:50_

> I could totally see myself using f-strings everywhere, and there's nothing wrong with that.

Hmm, maybe. I don't think that's how most Python users conventionally use f-strings, however

> I don't think always making the fix as unsafe is the right approach here. Instead, we could consider to be clever about when the fix is likely unsafe. E.g, can we split the string by whitespace and are there any symbols in scope that match a word in the string? If so, mark the fix as unsafe.

Yeah, that would indeed be very cool, and probably a better solution to just marking the fix as unsafe -- agreed!

---

_Comment by @MichaReiser on 2026-01-19 12:52_

> Hmm, maybe. I don't think that's how most Python users conventionally use f-strings, however

The diff in [#22650](https://github.com/astral-sh/ruff/pull/22650) suggests to me that it's more common than one might think.



---
