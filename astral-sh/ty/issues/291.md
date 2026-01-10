---
number: 291
title: Reimplement popular Mypy Plugins
type: issue
state: closed
author: johnthagen
labels:
  - question
assignees: []
created_at: 2025-05-09T11:43:47Z
updated_at: 2026-01-08T18:25:51Z
url: https://github.com/astral-sh/ty/issues/291
synced_at: 2026-01-10T01:51:14Z
---

# Reimplement popular Mypy Plugins

---

_Issue opened by @johnthagen on 2025-05-09 11:43_

> For [better discoverability of the decision](https://github.com/astral-sh/ruff/issues/15828#issuecomment-2865283264), cross post  to document reasons for 
> - https://github.com/astral-sh/ruff/issues/15828

### Description

In order to migrate from Mypy to Ruff for static type checking, one thing users may need are reimplementations of the most popular Mypy plugins.

- https://mypy.readthedocs.io/en/stable/extending_mypy.html#extending-mypy-using-plugins

Assuming for a moment that exposing a Mypy-compatible plugin interface is out of scope, the following Mypy plugins are ones I've seen in wide use within the community and could be considered:

- Some appear built into Mypy already: https://github.com/python/mypy/tree/master/mypy/plugins
- (Deprecated) Numpy: https://numpy.org/devdocs/reference/typing.html#mypy-plugin
- Pydantic: https://docs.pydantic.dev/latest/integrations/mypy/#enabling-the-plugin
- `django-stubs`: https://github.com/typeddjango/django-stubs?tab=readme-ov-file#installation
- `djangorestframework-stubs`: https://github.com/typeddjango/djangorestframework-stubs?tab=readme-ov-file#installation

Another list of Mypy plugins I found:

- https://github.com/typeddjango/awesome-python-typing?tab=readme-ov-file#mypy-plugins

Related to

- https://github.com/astral-sh/ruff/issues/3893

### References

- https://news.ycombinator.com/item?id=43919746
- https://www.youtube.com/live/XVwpL_cAvrw?feature=shared&t=2528

---

_Label `question` added by @MichaReiser on 2025-05-09 15:01_

---

_Comment by @max-muoto on 2025-05-09 16:22_

Implementation of a Pydantic plugin probably shouldn't be necessary post dataclass transforms - effectively every single behavior is natively supported in the typing system at this point: https://peps.python.org/pep-0681/

---

_Comment by @max-muoto on 2025-05-09 16:28_

I think this also begs a more philosophical question around the direction of Ty. Some type-checkers such as Pyright have avoided implementing plugin support to instead push for extending the native type-system: https://github.com/microsoft/pyright/issues/637#issuecomment-632759717

This in turn led to the aforementioned dataclass transforms. I'm certainly sympathetic to lack of robust type-checking for Django for non-MyPy type-checkers but https://github.com/sbdchd/django-types does a good job of typing Django without the need for any custom extensions from my experience.

---

_Comment by @carljm on 2025-05-09 16:37_

We also prefer a well-specified inter-operable type system over plugins. We currently have no plans to build a plugin system for `ty`. Implementing special-cased support for some widely-used library is more plausible, but also not on the current roadmap, and something we would prefer to avoid in favor of adding a general means to the type system to support the needed pattern(s).

---

_Comment by @w0rp on 2025-05-10 03:43_

I will add in that based on my professional on the job experience with Django for 11 years I have viewed typing Django as pretty much a lost cause. Django dev teams typically aren't interested in adopting type checking. I had previously success re-tooling Django classes to add in type information that gave better results than the mypy Django plugins. 

In my opinion: Django was built a long time ago long before PEP 484, and it will take significant efforts from the Django team to make breaking changes to Django to make it easier to produce accurate types for QuerySet classes, Manager classes, and that's even without a refactoring I think is needed for better asyncio support and avoiding the N+1 query problem.

I'd probably say: avoid adopting a plugin architecture for some time to come and focus on achieving feature parity with pyright.

---

_Comment by @lexicalunit on 2025-05-19 17:17_

+1 to feature party with pyright and to not adding plugins. Getting the most basic things working with https://github.com/sbdchd/django-types (django-stubs but with the mypy plugin stripped out) would be ideal.

Right now I get issues with code as simple as:

```python
from django.db import models

class Foo(models.Model):
    id: int  # even with explicitly added annotations it still doesn't work
    name = models.TextField()

def bar():
    f = Foo.objects.create()
    print(f.id)
    print(f.name)
```


```log
error[unresolved-attribute]: Type `Self` has no attribute `id`
  --> my/test/file:9:11
   |
25 | def bar():
26 |     f = Foo.objects.create()
27 |     print(f.id)
   |           ^^^^
28 |     print(f.name)
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `Self` has no attribute `name`
  --> my/test/file:10:11
   |
26 |     f = Foo.objects.create()
27 |     print(f.id)
28 |     print(f.name)
   |           ^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default
```

---

_Comment by @jellis18 on 2025-06-18 14:22_

So it appears that django-types is incompatible with `ty` at the moment? Has anyone found a way to get it to work?

---

_Comment by @w0rp on 2025-06-18 15:33_

> So it appears that django-types is incompatible with `ty` at the moment? Has anyone found a way to get it to work?

See my comment above. In my experience re-implementing classes in Django with explicit declarations and `# type: ignore` re-declarations of fields in Model classes works better. In combination with [django-types](https://github.com/sbdchd/django-types) it's the best you will ever do. The same is true for Pyright, which backs Pylance.

Django is sorely in need of a new major version with first-class type support via a dataclass re-implementation of Django models and `async def` methods for `QuerySet` classes with a removal of the magic addition of QuerySet methods to `Manager` classes. (Would it really kill people to write `Foo.objects.all().bar()` instead of `Foo.objects.bar()`?)

I use FastAPI for all new APIs instead of Django and DRF. I recognise the ongoing need to maintain Django code, plus Django's ease-of-use for implementing interactive blog and news sites, forums, admin panels, etc.

---

_Comment by @matmair on 2025-06-22 09:36_

No Django support will hold this back in a mayor part of the python ecosystem. You need to disable some pretty basic rules to use ty with Django right now. This is one of the few areas where Mypy still has the edge.

---

_Comment by @AdityaMayukhSom on 2025-11-10 21:59_

> Implementation of a Pydantic plugin probably shouldn't be necessary post dataclass transforms - effectively every single behavior is natively supported in the typing system at this point: https://peps.python.org/pep-0681/

Any thoughts on how to enable it to understand the kwargs for initialization on BaseModel implementations? 

<img width="542" height="190" alt="Image" src="https://github.com/user-attachments/assets/e2b40f8b-c8db-4d1d-bf35-8c25ad0cee68" />

Keeps on throwing `Argument `description` does not match any known parameterty[unknown-argument](https://ty.dev/rules#unknown-argument)`

---

_Comment by @carljm on 2025-11-10 22:08_

@AdityaMayukhSom Might be https://github.com/astral-sh/ty/issues/1425 -- are those fields on `Group` not type-annotated, by any chance?

If it's not that, you'd probably need to open a dedicated issue and show more of your code for a full repro.

---

_Comment by @AdityaMayukhSom on 2025-11-10 23:15_

> [@AdityaMayukhSom](https://github.com/AdityaMayukhSom) Might be [#1425](https://github.com/astral-sh/ty/issues/1425) -- are those fields on `Group` not type-annotated, by any chance?
> 
> If it's not that, you'd probably need to open a dedicated issue and show more of your code for a full repro.

<img width="923" height="365" alt="Image" src="https://github.com/user-attachments/assets/85edfecf-e5c9-48b8-bc8c-ca37f782bdab" />

<img width="1092" height="605" alt="Image" src="https://github.com/user-attachments/assets/c7e6369f-235f-4a68-849e-f615040c1da6" />

All are type annotated. @carljm 
P.S.: `TypeId` is alias for `uuid.UUID`, defined as `TypeId = uuid.UUID`


---

_Comment by @carljm on 2025-11-10 23:33_

@AdityaMayukhSom Can you create a new issue (not a comment on this issue)? Thank you! If you are able to minimize the problem to the smallest example where it still reproduces (and show full code for that example, including all base classes) that would be super helpful.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:53_

---

_Comment by @carljm on 2026-01-08 18:25_

This issue is too broad -- we definitely don't intend to add built-in support for _all_ mypy plugins. Closing in favor of #1018 , #2404 , #2403 

---

_Closed by @carljm on 2026-01-08 18:25_

---
