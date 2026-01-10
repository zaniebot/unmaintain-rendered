```yaml
number: 5243
title: RUF012 triggers many false positives (are they really? they are correct) in some projects
type: issue
state: open
author: scastlara
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-21T08:42:14Z
updated_at: 2025-03-04T11:59:50Z
url: https://github.com/astral-sh/ruff/issues/5243
synced_at: 2026-01-10T11:09:47Z
```

# RUF012 triggers many false positives (are they really? they are correct) in some projects

---

_Issue opened by @scastlara on 2023-06-21 08:42_

Hello 👋🏽 

In this [PR](https://github.com/astral-sh/ruff/pull/4390) RUF012 was extended to apply to non-dataclass classes. 

This has the (maybe desirable, maybe not, but was surprising to me) effect of _forcing typing_ for a RUF rule, in places where you would not necessarily use typing (if you did not want to).

For instance, in Django Rest Framework ([from their docs](https://www.django-rest-framework.org/api-guide/serializers/#modelserializer)):

```python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ['id', 'account_name', 'users', 'created']  # RUF012
```

Or in Django [itself](https://docs.djangoproject.com/en/4.2/ref/models/options/#django.db.models.Options.indexes):

```python
class Customer(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)

    class Meta:
        indexes = [
            models.Index(fields=["last_name", "first_name"]),  # RUF012
            models.Index(fields=["first_name"], name="first_name_idx"),  # RUF012
        ]
```

You could argue that's good, but at least to me, it's _different_ from dataclasses, since in there you are required to use typing to even use them, so they are opt-in even before you use ruff. You could also argue both Django and Django Rest Framework should be recommending tuples instead.

Isn't this too trigger happy for a RUF rule? Does it make sense to split it from the dataclass one? 

EDIT: Actually, it is a separated code already 🤦🏽 


In any case, feel free to close if this is just what it is! Ignoring the rule is easy for these kind of projects.

---

_Comment by @tjkuson on 2023-06-21 09:01_

For what it's worth, the Django project is against adding type hints, at least in the near future. You can read more in this [PR](https://github.com/django/deps/pull/65) and a [technical board statement](https://groups.google.com/g/django-developers/c/C_Phs05kL1Q/m/SX1oIqYKCQAJ). This might be why it doesn't recommend the `ClassVar` type annotation (and annotations in general).

For balance, however, the typed-django project also hasn't gone all-in with `ClassVar` as suggested in this [PR](https://github.com/typeddjango/django-stubs/pull/394#issuecomment-1302527184) (though a quick search of the codebase shows `ClassVar` is still used in some places).

Is it perhaps worth resolving a balance of only flagging a violation when type annotations are already used to annotate a variable?

---

_Comment by @scastlara on 2023-06-21 09:03_

> For what it's worth, the Django project is against adding type hints, at least in the near future.

Yep, but I wonder why a RUF rule should really be forcing type hints at all. 

---

_Comment by @charliermarsh on 2023-06-21 11:06_

Thanks for this! I’m a little busy this morning but I’ll get back to it later today and share my perspective :)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-21 11:06_

---

_Label `question` added by @charliermarsh on 2023-06-21 11:06_

---

_Comment by @ITProKyle on 2023-06-21 18:11_

Adding to the _false positives_, this is also flagging pydantic models which function similar to dataclasses in how they are defined but actually create new instances of objects when instantiating the class.

```python
from pydantic import BaseModel

class MyExample(BaseModel):

    foo: dict[str, str] = {}
```

In this example, `foo` is flagged with `RUF012: Mutable class attributes should be annotated with typing.ClassVar` but pydantic creates a new instance of the empty dict each time the class is instantiated. It's not well documented if at all (https://github.com/pydantic/pydantic/discussions/3877) but is how it functions.

---

_Comment by @charliermarsh on 2023-06-21 18:39_

Thank you for all the feedback here :)

A couple misc. reactions, from reading through the thread:

- This rule is admittedly "pedantic". I wish we had a way to express "pedantic" rules, and we will in the future, but for now, I intentionally made this its own rule code, independent of the `dataclass` version, so that it could be selectively disabled for projects in which it doesn't apply. (I do consider that a valid workaround for the issues raised thus far.)
- We should omit Pydantic models from this rule, that seems straightforward.
- On whether we should suggest `typing.ClassVar`: the question for me is whether the critique here is about the _rule_, or the _suggestion_. In the Django case, should we not be flagging those attributes _at all_, or should we not be _suggesting_ `typing.ClassVar` (and instead suggest something else, like using immutable data structures)?
- Related to the above: I'm not opposed to confining this check to classes with at least one typed attribute, but the thing is, it's still problematic (in the sense of the rule) to use a mutable default in those contexts -- the thing we're linting against is still present. (`typing.ClassVar` at least gives you clear semantics around how the field is meant to behave, and a way to enforce those semantics via a type checker, which is why it's presented as a possible solution.)


---

_Comment by @charliermarsh on 2023-06-21 18:39_

(To be clear: I'm looking for more feedback here, including on the questions above. Thanks all!)

---

_Comment by @LefterisJP on 2023-06-21 21:25_

I also wanted to ask here, when you have a class variable as an "immutable" constant for that class, what should you do? Should you still annotate with this? Is there some other annotation here, as you probably don't want to have mutable class variables in most cases?

---

_Comment by @scastlara on 2023-06-21 21:35_

> In the Django case, should we not be flagging those attributes at all, or should we not be suggesting typing.ClassVar (and instead suggest something else, like using immutable data structures)?

To me, it makes sense that a typing rule should be suggesting typing.ClassVar, while a bugbear-like rule should be suggesting inmutable data structures (even if Django uses mutable ones extensively). There is no way for ruff (or any other tool) to automatically decide what you meant, and so this would be two rules, or one rule with two possible fixes.

As my final 2c, the problem to me is not that the rule is pedantic per se, it is that it’s basically a typing rule, when many projects use typing as an opt-in thing (or not at all).

When and if ruff re-classifies its rules, it might become more clear. As it stands the RUF set of rules are basically whatever, so it makes sense that rules like this one end up there. Disabling is a perfectly OK workaround.

Thanks a lot for giving it a thought!



---

_Comment by @g-as on 2023-06-21 22:48_

> (To be clear: I'm looking for more feedback here, including on the questions above. Thanks all!)

One of my use cases is the following:

```python
from typing import ClassVar


class SomeAbstractBaseClass:

    some_attribute: ClassVar[dict[str, str]]


class SomeConcreteNewClass(SomeAbstractBaseClass):

    some_attribute = {"1": "2"}
```
This triggers `RUF012` on the subclass, where I would have expected this to pass.

---

_Comment by @kpfleming on 2023-06-24 11:09_

I've just had this fire on an Ansible plugin module which does not have any type annotations. For now I'll disable RUF012 and hope that #5275 resolves it.

---

_Comment by @smackesey on 2023-07-04 15:22_

>We should omit Pydantic models from this rule, that seems straightforward.

I'd suggest a setting that lets you whitelist certain ancestor classes, with maybe default set to `Pydantic.BaseModel` or whatever. Since there are multiple libs where the "problem" syntax is part of a DSL.

I am not sure if this is possible yet, but it might be with the new import resolver.

---

_Comment by @charliermarsh on 2023-07-04 15:32_

Yeah makes sense. Also happy to add other libs to the exemption if it’d be helpful.

---

_Comment by @johnthagen on 2023-07-06 17:58_

Related suggestion, have you considered a new category for "Ruff Warnings" (`RUW`?)? I think for things like `RUF012` it could be nice to put them in a new category so that people could opt in to these kinds of lints separate from the other lints in `RUF`?

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:10_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:10_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:10_

---

_Comment by @flaeppe on 2023-09-28 09:37_

> [...] In the Django case, should we not be flagging those attributes at all, or should we not be suggesting typing.ClassVar (and instead suggest something else, like using immutable data structures)? [...]

In the Django and its (database) model case it's not valid, as there's class descriptors doing a bit of magic there. As such the `ClassVar` is only valid for 1 case, when there's no instance of a `Model`. Though if you have a `Model` _instance_, the attribute contains the python value of what's stored in a database.

---

_Comment by @alexei on 2023-12-16 13:58_

I found Django migrations are an area where `ruff` and `mypy` might not agree as far as this rule is concerned.

### Sample 1

```python
class Migration(migrations.Migration):
    dependencies = [...]
    operations = [...]
```

`ruff` isn't satisfied:

> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`


### Sample 2

```python
class Migration(migrations.Migration):
    dependencies: ClassVar[list[tuple[str, str]]] = [...]

    operations: ClassVar[list[Operation]] = [...]
```

`ruff` is satisfied, but not `mypy`:

> error: Cannot override instance variable (previously declared on base class "Migration") with class variable  [misc]

### Sample 3

```python
class Migration(migrations.Migration):
    dependencies: list[tuple[str, str]] = [...]

    operations: list[Operation] = [...]
```

`mypy` is fine with it, `ruff` insists:

> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`


---

_Comment by @charliermarsh on 2023-12-16 15:53_

How does Mypy behave if you use `: Sequence[...]` instead of `: list[...]`? That would signal to Ruff that the type is immutable.

---

_Comment by @alexei on 2023-12-16 16:24_

@charliermarsh it behaves as desired. I hadn't considered `Sequence` before. Thanks for the tip!

---

_Comment by @charliermarsh on 2023-12-16 20:42_

@alexei - No prob! I should probably add this to the rule docs.

---

_Comment by @ITProKyle on 2023-12-18 15:14_

@charliermarsh, I appreciate the fix in #5274 to omit pydantic models however, it does not omit classes that subclass user defined models.

```python
from pydantic import BaseModel


class MyBaseModel(BaseModel):
    
    def __bool__(self) -> bool:
        """pydantic.BaseModel does not implement __bool__."""
        return bool(self.model_dump(exclude_unset=True))


class MyExample(MyBaseModel):

    subclass_field: dict[str, str] = {}  # RUF012
```

If this is not something that ruff can determine on it's own, an option like [`runtime-evaluated-base-classes`](https://beta.ruff.rs/docs/settings/#flake8-type-checking) for this case would be an acceptable solution.

---

_Comment by @charliermarsh on 2023-12-18 15:47_

@ITProKyle -- We can definitely detect this for subclasses defined in the same file, and I'll make that change now. Unfortunately we can't yet support it for classes defined _across_ files.

---

_Comment by @ITProKyle on 2023-12-18 16:16_

Every usage I have are in different file and even different python packages all together so it sounds like a combination of both detection and a config setting would be needed in my case.

---

_Comment by @charliermarsh on 2023-12-18 16:39_

It would be nice if we could somehow reuse an existing setting here. It's not quite identical to `runtime-evaluated-base-classes` though 🤔 

---

_Comment by @kratsg on 2024-04-03 00:12_

I ran into this for a [`beanie.Document`](https://github.com/roman-right/beanie) class where the MRO does include `pydantic` (my guess is `ruff` doesn't see the subclass):

```
>>> Document.__mro__
(<class 'beanie.odm.documents.Document'>, <class 'lazy_model.parser.new.LazyModel'>, <class 'pydantic.main.BaseModel'>, <class 'beanie.odm.interfaces.setters.SettersInterface'>, <class 'beanie.odm.interfaces.inheritance.InheritanceInterface'>, <class 'beanie.odm.interfaces.find.FindInterface'>, <class 'beanie.odm.interfaces.aggregate.AggregateInterface'>, <class 'beanie.odm.interfaces.getters.OtherGettersInterface'>, <class 'object'>)
```

So I think for now, i will need to add in a `#noqa: RUF012` for those lines that have a problem at the moment, as I believe there's no way to configure to "teach" `ruff` about the `Document` class here.

---

_Comment by @Anselmoo on 2024-05-24 21:41_

> > (To be clear: I'm looking for more feedback here, including on the questions above. Thanks all!)
> 
> One of my use cases is the following:
> 
> ```python
> from typing import ClassVar
> 
> 
> class SomeAbstractBaseClass:
> 
>     some_attribute: ClassVar[dict[str, str]]
> 
> 
> class SomeConcreteNewClass(SomeAbstractBaseClass):
> 
>     some_attribute = {"1": "2"}
> ```
> 
> This triggers `RUF012` on the subclass, where I would have expected this to pass.

I find a similiar observation for:

```py
some_attribute = ClassVar[dict[str, str]]

class MyExample:

    subclass_field: some_attribute = {}
```

here is `RUF012` appears, but I expect not to be, which might be more convenient if you inherit over several classes:

```py
class MyExample2:

    subclass_field: some_attribute = {}
class MyExample3:

    subclass_field: some_attribute = {}
```



---

_Comment by @dhruvmanila on 2024-05-27 05:05_

> I find a similiar observation for:
> 
> ```python
> some_attribute = ClassVar[dict[str, str]]
> 
> class MyExample:
> 
>     subclass_field: some_attribute = {}
> ```

I think this might be because it uses type alias.

---

_Comment by @Anselmoo on 2024-05-27 14:12_

Yes, but is it bad to use `alias`, respectively, own _type definitions_ to avoid code redundancy? 

* If so, using `some_attribute = ClassVar[dict[str, str]]` should cause already an alert?

---

_Comment by @dhruvmanila on 2024-05-28 04:19_

No, it's not bad but just a limitation of Ruff which will be resolved with the ongoing red knot project.

---

_Comment by @beauxq on 2025-01-10 16:02_

> Yes, but is it bad to use `alias`, respectively, own _type definitions_ to avoid code redundancy?
> 
>     * If so, using `some_attribute = ClassVar[dict[str, str]]` should cause already an alert?

I personally would not put `ClassVar` in a type alias.
I think of a type alias as an alias to a type, and `ClassVar` is not part of the type.
```python
MyTypeAlias = dict[str, tuple[int, bytes, Literal[1, 2]]]

class C:
    subclass_field: ClassVar[MyTypeAlias]
```
I would only put something in the type alias if it's part of the type.

---

_Comment by @beauxq on 2025-01-10 16:11_

Since many voices here are expressing that `RUF012` is too strict, I just want to make sure other perspectives are heard.
For me personally, `RUF012` is not strict enough ( https://github.com/astral-sh/ruff/issues/13137 )
So I don't see a problem with it being too strict.

---

_Comment by @beauxq on 2025-01-10 19:11_

I do think it's a problem the way it suggests `typing.ClassVar` when that is very often not the appropriate solution.
`ClassVar` is sometimes the appropriate solution.

But more often, the better solution is to move the initialization to `__init__`

```python
class C:
    a: list[int]

    def __init__(self):
        self.a = []
```

---

_Comment by @Tatsh on 2025-02-06 20:36_

My issue with RUF012 is that it triggers even when a parent class (type stub) has the attribute properly annotated.

Type:

```python
# __set__ value type
_ST = TypeVar("_ST", contravariant=True)
# __get__ return type
_GT = TypeVar("_GT", covariant=True)
_ErrorMessagesDict: TypeAlias = dict[str, _StrOrPromise]

class Field(RegisterLookupMixin, Generic[_ST, _GT]):
    default_error_messages: ClassVar[_ErrorMessagesDict]
```

Code:

```python
class CustomField(Field[Any, Any]):
    default_error_messages = {'invalid': _('"%(value)s" is not a x')}  # RUF012
```

---

_Comment by @zyv on 2025-03-04 06:44_

Could `django.contrib.admin.ModelAdmin` please be added to the base class exception list? For example, `radio_fields` is expected to be a dictionary... thanks!

Migrations have already been mentioned, but it's so problematic that I had to add a per-file ignore:

```toml
[tool.ruff.lint.per-file-ignores]
"**/migrations/**.py" = ["RUF012"]
```

---

_Comment by @zyv on 2025-03-04 07:37_

P.S. I opened typeddjango/django-stubs#2524 for the `ModelAdmin` issue. I was wondering what you (Astral) guys think about migrations. Are you open to adding an exception for `django.db.migrations.Migration` or would I have more luck trying to get `ClassVar` markers on `lists` in TypedDjango? I'm a bit on the fence on this.

---

_Comment by @beauxq on 2025-03-04 11:59_

I think things can improve with this when we have red knot.
Right now, ruff can't really see the relationship between  base classes and subclasses.
```python
class B:
    a: ClassVar[int] = 3

class C(B):
    a = 4  # ruff can't see that this is already annotated with ClassVar
```
So you might have to put `ClassVar` in thousands of subclasses, when it should be sufficient to just put it in one place.

What `RUF012` is trying to do, can't be done well without a type-checker.

---
