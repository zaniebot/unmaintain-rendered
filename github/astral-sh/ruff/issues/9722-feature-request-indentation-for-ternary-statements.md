---
number: 9722
title: "[Feature request] Indentation for ternary statements"
type: issue
state: open
author: tylerlaprade
labels:
  - formatter
  - style
assignees: []
created_at: 2024-01-30T21:53:34Z
updated_at: 2026-01-01T04:13:56Z
url: https://github.com/astral-sh/ruff/issues/9722
synced_at: 2026-01-07T13:12:15-06:00
---

# [Feature request] Indentation for ternary statements

---

_Issue opened by @tylerlaprade on 2024-01-30 21:53_

Ternary statements that are broken across multiple lines are indented at the same level as the parent line, making them very hard to read, especially if surrounded by additional logic. Prettier handles the equivalent case in JS/TS by adding an indentation level to the ternaries. This would likely need to be behind a flag for ease of migration from Black, but ideally this would be the only behavior if that constraint didn't exist.

#### Ruff
<img width="364" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/8528b9b8-06c7-4cf4-b735-819ae6e37331">

#### Prettier
<img width="687" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/a0da3eda-8f69-4f18-a3cb-585a6be894b3">

---

_Label `formatter` added by @AlexWaygood on 2024-01-30 22:04_

---

_Comment by @JelleZijlstra on 2024-01-30 23:57_

How would you expect the Python code to be formatted? Since Python ternaries are written out in a different order than JS ternaries, it's difficult to apply the Prettier format directly.

---

_Comment by @tylerlaprade on 2024-01-31 03:24_

That's a good point, I hadn't considered that difference. I think a single additional indent in front of `if` and `else` would make the most sense.

---

_Label `style` added by @MichaReiser on 2024-01-31 07:33_

---

_Comment by @MichaReiser on 2024-01-31 07:34_

I like the idea and I want to improve the conditional formatting because the current formatting can be hard to read. That's why Black's now parenthesizes long conditionals but I would prefer to find a more readable style without adding extra parentheses (see https://github.com/psf/black/issues/4123)



---

_Referenced in [uclahs-cds/Ligare#46](../../uclahs-cds/Ligare/pulls/46.md) on 2024-02-26 23:05_

---

_Referenced in [uclahs-cds/Ligare#47](../../uclahs-cds/Ligare/issues/47.md) on 2024-02-26 23:08_

---

_Referenced in [algolia/api-clients-automation#3204](../../algolia/api-clients-automation/pulls/3204.md) on 2024-06-18 20:08_

---

_Comment by @vanakema on 2024-11-05 18:38_

I'm with @tylerlaprade on indenting the `if` and `else`. It's simple and it makes it way more readable.

Here's an example
Current Ruff formatting
```py
        foo = Foo(
            bar_id=bar.id,
            baz_id=f'{baz.name}_{bar_numer}_{idx}'
            if bar_number
            else f'{baz.name}_{idx}',
            zap=zap,
        )
```

Desired ruff formatting (which ruff format does not allow)
```py
        foo = Foo(
            bar_id=bar.id,
            baz_id=f'{baz.name}_{bar_numer}_{idx}'
                if bar_number
                else f'{baz.name}_{idx}',
            zap=zap,
        )
```

This makes it easier to glean while skimming code that it is a ternary and not part of the arg list

---

_Comment by @mathmul on 2025-04-29 04:09_

And here I am wishing
```python
def get_payment_method_client(payment_method: PaymentMethodEnum, credentials: dict) -> BaseClient:
    def raise_payment_method_not_found():
        raise PaymentMethodNotFoundException()
    return (
        AlmaPayClient(credentials)    if PaymentMethodEnum.ALMA_PAY == payment_method else
        SmartGiftyClient(credentials) if PaymentMethodEnum.SMART_GIFTY == payment_method else # Note: Excluding this comment the line has less characters than line-length in pyproject.toml
        # TODO: add other payment methods here
        raise_payment_method_not_found()
    )
```
wouldn't turn into a mess that is
```python
def get_payment_method_client(payment_method: PaymentMethodEnum, credentials: dict) -> BaseClient:
    def raise_payment_method_not_found():
        raise PaymentMethodNotFoundException()

    return (
        AlmaPayClient(credentials)
        if PaymentMethodEnum.ALMA_PAY == payment_method
        else SmartGiftyClient(credentials)
        if PaymentMethodEnum.SMART_GIFTY == payment_method
        # TODO: add other payment methods here
        else raise_payment_method_not_found()
    )
```
forcing me to keep
```python
def get_payment_method_client(payment_method: PaymentMethodEnum, credentials: dict) -> BaseClient:
    match payment_method:
        case PaymentMethodEnum.ALMA_PAY:
            return AlmaPayClient(credentials)
        case PaymentMethodEnum.SMART_GIFTY:
            return SmartGiftyClient(credentials)
        # TODO: add other payment methods here
        case _:
            raise PaymentMethodNotFoundException()
```
which will be almost double the lines, once I finish implementing the remaining 11 payment methods... and less readable IMO.

---

_Comment by @tylerlaprade on 2025-04-29 23:55_

@mathmul, that will be WAY easier to read as a switch with 13 cases as opposed to 13 if/else ternaries. That's the exact usecase that switches are designed for.

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-11-11 12:38_

---

_Comment by @hauntsaninja on 2025-11-11 20:21_

I definitely agree that vanakema's example is formatted badly (as also evidenced by upvotes). Black since 2024 has `parenthesize_conditional_expressions`, so Black would format it like so:
```
        foo = Foo(
            bar_id=bar.id,
            baz_id=(
                f"{baz.name}_{bar_number_asdf}_{idx}"
                if bar_number_asdf
                else f"{baz.name}_{idx}"
            ),
            zap=zap,
        )
```

However, *once already parenthesised* I don't see an advantage in adding additional indentation to the condition clause and the else clause. That is, for me it makes things worse to reformat this:
```
class A:
    def meth(self):
        return (
            ["version_number", "created_at", "updated_at"]
            if obj
            else self.readonly_fields
        )
```
to this:
```
class A:
    def meth(self):
        return (
            ["version_number", "created_at", "updated_at"]
                if obj
                else self.readonly_fields
        )
```
It feels overindented for no clear reason and reduces symmetry between the two branches (in a way that the JS/TS case does not). And the version of that without parentheses and just indents is not valid Python syntax

---

_Comment by @jackdent on 2026-01-01 04:13_

I'd love a way to enforce that a ternary operator needs to be surrounded by parens if it's going to span multiple lines.

---
