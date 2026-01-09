---
number: 17372
title: "`ARG001` False Positive (Django-Ninja)"
type: issue
state: open
author: UnoYakshi
labels:
  - bug
  - needs-design
assignees: []
created_at: 2025-04-13T12:00:02Z
updated_at: 2025-05-22T14:11:38Z
url: https://github.com/astral-sh/ruff/issues/17372
synced_at: 2026-01-07T13:12:16-06:00
---

# `ARG001` False Positive (Django-Ninja)

---

_Issue opened by @UnoYakshi on 2025-04-13 12:00_

### Summary

# Problem
In a simple Django-Ninja project, routers/APIs endpoints with "unused" (by developers) `request` trigger `ARG001`.


# Context
```python
# Imports and stuff...


@router.get('/', response=list[UserSchema])
def get_users(request: HttpRequest) -> list[UserSchema]:  # Triggers ARG001!
    """Retrieve all Users..."""

    return [UserSchema.from_orm(user) for user in BaseUser.objects.all()]


@router.get('/{uuid}', response=UserSchema)
def get_user(uuid: str) -> UserSchema:  # Doesn't trigger ARG001, but fails within Django-Ninja...
    """Retrieve a specific User by UUID..."""

    return UserSchema.from_orm(BaseUser.objects.get(id=uuid))
```
Please, ping me if you need copy-and-paste setup, I'll compose it. For now, my current project has too much project-specific configs (mixins, directories structure, etc.)

## Function: `get_users()`, Endpoint: `GET /`
`ruff check` for this file tells:
```
13:15 ARG001 Unused function argument: `request`
```

## Function: `get_user()`, Endpoint `GET /<uuid>`
### remove `request` parameter
Fails when calling the endpoint `GET /<uuid>`. Because, in fact, `request` IS used in runtime. Traceback:
```
Traceback (most recent call last):
  File "ABSOLUTE_PATH_TO_PROJECT/.venv/lib/python3.13/site-packages/ninja/operation.py", line 133, in run
    result = self.view_func(request, **values)
TypeError: get_user() got multiple values for argument 'uuid'
```
### rename `request` to `_request`
As suggested in `ARG001`, such arguments should be prefixed with `_`. However, it also gives an error:
```
NameError: Fields must not use names with leading underscores; e.g., use 'request' instead of '_request'.
```



# "Solutions"
1. Set `lint.dummy-variable-rgx = "^request$"`. I don't think I should do that, though: 
  - the purpose this of this parameter is different
  - there could be more implicitly used input parameters
2. Ignore `ARG001` altogether because IDEs will most likely highlight unused arguments, anyway. But I don't like the idea of CI not catching fundamental linting issues.
3. Adding `#noqa: ARG001` to every endpoint method is not good enough.

# Suggestion
Add a separate linting config parameter for such cases.
```toml
[lint.per-varname-ignores]
"request" = ["ARG001"]
```
Perhaps, it already exists and I'm blind/silly.

### Version

ruff 0.11.5

---

_Renamed from "`ARG001` False Positive (Djang-Ninja)" to "`ARG001` False Positive (Django-Ninja)" by @UnoYakshi on 2025-04-13 12:00_

---

_Comment by @ntBre on 2025-04-15 13:35_

Thanks for the report! This is interesting. So django-ninja requires you to include a `request` parameter in every handler even if you don't use it directly?

I like your idea of a config option for this because it seems difficult to detect otherwise, but it may also be doable with type inference that can inspect the decorator's definition.

---

_Label `bug` added by @ntBre on 2025-04-15 13:35_

---

_Label `needs-design` added by @ntBre on 2025-04-15 13:35_

---

_Comment by @UnoYakshi on 2025-04-16 10:34_

> So django-ninja requires you to include a `request` parameter in every handler even if you don't use it directly?

I'm working with Django-Ninja like for ca. a week, hence, there might be other ways. However, [how-to in docs](https://django-ninja.dev/guides/input/operations/) doesn't seem to suggest an alternative.


> I like your idea of a config option for this because it seems difficult to detect otherwise, but it may also be doable with type inference that can inspect the decorator's definition.

I suggested another config parameter because, I presume, it'd most likely be the cheapest/simplest solution. I mean, it's `lint.dummy-variable-rgx` with another name, or are there caveats? I know a little bit of Rust so I might even try to add a PR for that as a little hands-on exercise.
As for the type inference, as far as I'm aware it's in the design/development stage? Or there is a nightly version available already?

---

_Comment by @ntBre on 2025-04-16 13:18_

> I'm working with Django-Ninja like for ca. a week, hence, there might be other ways. However, [how-to in docs](https://django-ninja.dev/guides/input/operations/) doesn't seem to suggest an alternative.

Yeah I looked at the docs a bit, and that was my conclusion too.

> I suggested another config parameter because, I presume, it'd most likely be the cheapest/simplest solution. I mean, it's `lint.dummy-variable-rgx` with another name, or are there caveats? I know a little bit of Rust so I might even try to add a PR for that as a little hands-on exercise. 

I think it is pretty much that straightforward. I'd want to check with @MichaReiser before adding a new config option though.

> As for the type inference, as far as I'm aware it's in the design/development stage? Or there is a nightly version available already?

We do have our type-checker, red-knot, in development right now, but it's a separate binary (no nightly release yet, but you can build from source if you're really curious 🙂). Eventually we also hope to use some of its type inference capabilities in ruff itself, but that is farther away.

In short, the config option seems like the quickest and easiest way to solve this right now, but Micha might have better ideas.

---

_Comment by @MichaReiser on 2025-04-22 09:51_

The underlying problem that I see here is that Ruff doesn't understand the `router.get` (and similar) decorators. 

One option is to disable `ARG001` for any decorated function (unless it is a known decorator). That's the safest but probably leads to many false positives. The alternative is to add an allow list of decorators where unused arguments aren't flagged. 

Both approaches have the downside that any unused argument won't be flagged for those functions, which may be overly permissive. 

I'm a bit hesitant to add either configuration option just yet unless there are other users/frameworks where people run into this. 

@UnoYakshi have you considered opening an issue in `django-ninja` asking if it would be an option to make the `request` argument optional (so that it doesn't have to be specified in the function if unused in the entire body)?

---

_Comment by @UnoYakshi on 2025-05-04 21:58_

Sorry for late reply, @MichaReiser.


> One option is to disable ARG001 for any decorated function.

I'd prefer not to exclude `ARG001` from anywhere if possible. I do like the rule most of the time, decorator or not.


> The alternative is to add an allow list of decorators where unused arguments aren't flagged.

I'm not really sure if the problem lies within decorators area. Do you mind elaborating why do you think otherwise? As I see it's possible to write similar non-decorated (maybe, currying?) code that could dynamically use a function with "unused" argument. E.g., getting a method from a module via `inspect`, not `import` (I can provide a code example if I didn't express the idea clear enough).


> @UnoYakshi have you considered opening an issue in django-ninja asking if it would be an option to make the request argument optional (so that it doesn't have to be specified in the function if unused in the entire body)?

I didn't think about that before, but I'll open an issue, sure. Update: [link to the issue](https://github.com/vitalik/django-ninja/issues/1458).

---

_Referenced in [vitalik/django-ninja#1458](../../vitalik/django-ninja/issues/1458.md) on 2025-05-04 22:05_

---

_Comment by @MichaReiser on 2025-05-07 06:42_

Thank you. The solution of prefixing request with an underscore that the maintainer from django-ninja suggested seems like a good solution to me.

---

_Comment by @UnoYakshi on 2025-05-21 15:55_

Except for it doesn't work on my side for now.
```
NameError: Fields must not use names with leading underscores; e.g., use 'request' instead of '_request'.
```

So I'm sticking with the dummy-thing for the time being.

---

_Comment by @MichaReiser on 2025-05-21 16:16_

Where do you get this `NameError`?

---

_Comment by @UnoYakshi on 2025-05-22 14:09_

> Where do you get this `NameError`?

When I start Django (`manage.py runserver ...`), it's thrown in the console with traceback. Or did you mean something else?

---

_Comment by @MichaReiser on 2025-05-22 14:11_

I saw you posted on the upstream issue too. Let's see what they reply

---

_Referenced in [astral-sh/ruff#18654](../../astral-sh/ruff/issues/18654.md) on 2025-06-12 22:28_

---
