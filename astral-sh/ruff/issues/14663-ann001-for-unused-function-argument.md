```yaml
number: 14663
title: ANN001 for unused function argument
type: issue
state: closed
author: towiat
labels:
  - rule
assignees: []
created_at: 2024-11-28T21:40:16Z
updated_at: 2024-12-10T09:53:54Z
url: https://github.com/astral-sh/ruff/issues/14663
synced_at: 2026-01-12T15:54:54Z
```

# ANN001 for unused function argument

---

_@towiat_

Given the setup
```toml
[tool.ruff]
select = [
    "ANN001", # missing-type-function-argument
    "ARG001", # unused-function-argument
]
```
and the function
```python
def callback(_unused, foo: str) -> None:
    print(foo)
```
ruff (version 0.8.0) does not raise `ARG001` since the argument name `_unused` matches the `dummy-variable-rgx` pattern.

It does, however, raise `ANN001` even though a type annotation for an unused argument doesn't make a lot of sense.

Shouldn't the check for `ANN001` also take the `dummy-variable-rgx` pattern into account?

---

_Comment by @MichaReiser on 2024-11-29 08:24_

Thanks for opening this issue. This is an interesting question and we may have to consider this in a more complete example. E.g is `callback` overriding some method or why is the first argument unused?

I agree that the type annotation seems rather useless for the `callback`'s body. However, annotating the parameter type is important to verify that the function is called with the right arguments. So there's value in having the annotation, but only if the signature can't be derived from e.g. a parent method.



---

_Label `rule` added by @MichaReiser on 2024-11-29 08:24_

---

_Comment by @towiat on 2024-11-29 12:29_

Thanks for looking into this. I'm also not completely sure, but here is some context:

I ran into this while writing a callback for a click option like this (snippet from the [click documentation](https://click.palletsprojects.com/en/stable/options/#callbacks-and-eager-options)):
```python
def print_version(ctx, param, value):
    if not value or ctx.resilient_parsing:
        return
    click.echo('Version 1.0')
    ctx.exit()

@click.command()
@click.option('--version', is_flag=True, callback=print_version,
              expose_value=False, is_eager=True)
def hello():
    click.echo('Hello World!')
```

Note that click calls the callback with positional parameters, so it doesn't care about the names.

In my use case I only need the `value` argument which leaves me with the following choices to annotate the callback:
```python
def callback(ctx: click.Context, param: click.Parameter, value: str):    # raises ARG001
    do_something_with(value)

def callback(_ctx, _param, value: str):                                  # raises ANN001
    do_something_with(value)

def callback(_ctx: click.Context, _param: click.Parameter, value: str):  # works, but... hmmm
    do_something_with(value)
```

Pyright has a `reportMissingParameterType` check which has an exemption for unused arguments, although it only considers single underscore `_` arguments as unused. See also [this Pyright issue](https://github.com/microsoft/pyright/issues/8884#issue-2503435825).

---

_Comment by @mnesarco on 2024-12-10 03:35_

Hello I have the same issue, specially in methods that override parent methods, usually some arguments are unused in subclasses but they need to keep the same signature so I use single underscore prefix to mark as unused, but ANN001 keeps kiking my ass 😅

---

_Comment by @MichaReiser on 2024-12-10 07:26_

Oh, I just realized that `ANN01` has [a setting](https://docs.astral.sh/ruff/settings/#lint_flake8-annotations_suppress-dummy-args) that disables the ANN rules for dummy variables. 

```toml
[tool.ruff.lint.flake8-annotations]
suppress-dummy-args = true
```

I think that should solve both your use cases.

---

_Comment by @towiat on 2024-12-10 09:23_

Huh, this works as described and does indeed solve my issue.

Since I somehow overlooked this despite studying the documentation (I swear!), would it make sense to mention this setting in the description of the affected rules (just like `dummy-variable-rgx` is mentioned in the [ARG001](https://docs.astral.sh/ruff/rules/unused-function-argument/) documentation)?

Other than that, this can be closed from my side. Thanks!

---

_Comment by @MichaReiser on 2024-12-10 09:47_

That's a good call out. Let me add it real quick.

---

_Closed by @MichaReiser on 2024-12-10 09:53_

---
