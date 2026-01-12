```yaml
number: 3303
title: "COM812: correct?"
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-03-02T12:26:43Z
updated_at: 2023-07-08T20:43:26Z
url: https://github.com/astral-sh/ruff/issues/3303
synced_at: 2026-01-12T15:54:43Z
```

# COM812: correct?

---

_@spaceone_

```python
foo = tuple
print(
    list(foo(1, 2, 3, 4))
    + list(foo(6, 7, 8, 9))
    + list(foo(6, 7, 8, 9))
    + list(foo(6, 7, 8, 9))
    + list(foo(6, 7, 8, 9))
)
```
tells me: `foo.py|7 col 28 error| COM812 [*] Trailing comma missing` which looks strange.

Also `black` wouldn't change the code:
```console
$ black --diff foo.py
All done! ✨ 🍰 ✨
1 file would be left unchanged
```

is that trailing comma correct?

---

_Comment by @bluetech on 2023-03-02 14:56_

It is technically correct. I noticed myself many cases where a trailing comma is technically correct but undesirable, like this one, but I can't quite phrase the correct heuristic. It definitely involves single-argument function calls, but sometimes I do want a trailing comma for these...

> Also black wouldn't change the code

Does black add trailing commas at all?

If a trailing comma is present in this example, does black *remove* it?

---

_Comment by @spaceone on 2023-03-02 14:57_

no, `black` doesn't remove it.
The trailing commata is sometimes a marker in black to split things into multiline.
but that is also probably not related to this rule.

---

_Comment by @bluetech on 2023-03-02 15:05_

So black can't guide us on the desired heuristic here.

I myself turned off COM812 due to these two cases in Django code:

```py
results = list(
    # ... Long queryset ...
    xxx   # <-- Don't want comma here
)
```

```py
class MyModel(models.Model):
    my_field = models.CharField(
        ...,
        help_text=_(
             "Long help text "
             "Here"   # <-- Don't want comma here
        ),
    )
```

One heuristic is to not add trailing comma on single-argument function calls, but this is not 100%.

Another option is to add a setting for function names to which trailing comma shouldn't be added (`list` and `_` in my case), but would be better if it's not needed.

---

_Comment by @bluetech on 2023-03-02 15:08_

Some discussion in flake8-commas issue: https://github.com/PyCQA/flake8-commas/issues/59

---

_Label `question` added by @charliermarsh on 2023-03-02 15:20_

---

_Comment by @JonathanPlasse on 2023-03-02 20:33_

> One heuristic is to not add trailing comma on single-argument function calls, but this is not 100%.

In which condition would having a comma for single argument in function be desirable?

---

_Comment by @bluetech on 2023-03-02 21:06_

Any function that can have more arguments. If we stay in DJango just for the example

```py
my_field = models.CharField(
    _("My field")
)
```

I'd want a trailing comma in this case. But not having one is better than disabling COM802 entirely.

---

_Comment by @charliermarsh on 2023-03-02 21:10_

Yeah it's very reasonable to have a trailing comma for a single-argument function IMO.

Black does add a trailing comma if you have more than one argument -- so it doesn't add one after `list(...)` here:

```py
x(
    list(
        1,
        2,
        3,
        4,
        5,
        6,
        7,
        812093129032910381290321903219031,
        812093129032910381290321903219031,
    )
)
```

But it does add one after `y` here:

```py
x(
    list(
        1,
        2,
        3,
        4,
        5,
        6,
        7,
        812093129032910381290321903219031,
        812093129032910381290321903219031,
    ),
    y
)
```

---

_Comment by @charliermarsh on 2023-03-02 21:11_

We could avoid adding a trailing comma for single-argument functions that match certain expressions... like binary expressions, etc.

---

_Comment by @Avasam on 2023-03-14 21:18_

Whilst I don't use black, I do use [`add-trailing-comma`'s multiline method invocation style](https://github.com/asottile/add-trailing-comma#multi-line-method-invocation-style----why), which is worth taking a look at.

---

_Comment by @charliermarsh on 2023-03-14 22:10_

Looks like `add-trailing-comma` _does_ add a comma in this case, like Ruff (but avoids trailing commas for functions with trailing `*args` or `**kwargs`).

---

_Comment by @Avasam on 2023-03-14 23:26_

> [...] (but avoids trailing commas for functions with trailing `*args` or `**kwargs`).

To be exact, [it only does so for python 3.5](https://github.com/asottile/add-trailing-comma#trailing-commas-for-function-definitions-with-unpacking-arguments), [so does Black](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#trailing-commas), and because that'd be a syntax error. On recent python versions everything multiline gets a trailing comma. I don't think Ruff is even concerned about EOL python versions.

The reasoning `add-trailing-comma` uses on reducing lines in a diff is sound to me. And the reason why I want trailing commas in the first place.
I can see the argument for aesthetics on single-parameters. Even Black didn't take a hard stance here (which I find surprising).

Since this is more or less all gonna end up being the same rule in Ruff, maybe this should be left to the user's choice in whatever config system Ruff is going towards (insert trailing commas for single multiline items, remove trailing commas for single multiline items, or leave as-is with a "[magic trailing comma](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#the-magic-trailing-comma)" like black does)


---

_Comment by @charliermarsh on 2023-03-14 23:56_

Yeah right now the Ruff formatter uses similar trailing comma rules to Black. I think I'm fine with us inserting a trailing comma even for single-argument calls in COM812 right now -- it can look odd in some cases, but I think it's "correct" and matches the intent of the rule, so I'll close this.

---

_Closed by @charliermarsh on 2023-03-14 23:56_

---

_Comment by @benjamin-kirkbride on 2023-07-08 20:43_

![image](https://github.com/astral-sh/ruff/assets/3373830/52daca39-0e45-4b96-b40a-75148f23f3ac)

is this related?

---
