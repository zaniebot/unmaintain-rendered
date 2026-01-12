```yaml
number: 2214
title: "error message improvement: mark the inconsistent part of the overriding signature"
type: issue
state: closed
author: pjonsson
labels:
  - diagnostics
assignees: []
created_at: 2025-12-24T23:37:49Z
updated_at: 2025-12-25T12:35:27Z
url: https://github.com/astral-sh/ty/issues/2214
synced_at: 2026-01-12T15:54:26Z
```

# error message improvement: mark the inconsistent part of the overriding signature

---

_@pjonsson_

### Summary

This might be a documentation issue, but I'm filing it as a bug since I don't understand what action the error message is prompting me to do.

I just got:
```
error[invalid-method-override]: Invalid override of method `get_by_name_unsafe`
   --> datacube/index/postgres/_products.py:349:9
    |
347 |     # pylint: disable=method-hidden
348 |     @override
349 |     def get_by_name_unsafe(self, name: str) -> Product:
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `AbstractProductResource.get_by_name_unsafe`
350 |         with self._db_connection() as connection:
351 |             result = connection.get_product_by_name(name)
    |
   ::: datacube/index/abstract/_products.py:345:9
    |
344 |     @abstractmethod
345 |     def get_by_name_unsafe(self, name: str) -> Product:
    |         ---------------------------------------------- `AbstractProductResource.get_by_name_unsafe` defined here
346 |         """
347 |         Fetch product by name
    |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default
```
and I'm confused even after reading the documentation. One issue in this repository mentioned something about Pyright implementing the correct semantics for LSP, but mypy does not, but Pyright accepts this particular code without complaints.

It would be helpful if the error markers pinpointed the incompatible part of the signature instead of the entire definition.

### Version

ty 0.0.7

---

_Comment by @AlexWaygood on 2025-12-25 00:42_

Thanks for the report, and sorry our diagnostic here was so unhelpful!

I think this _may_ be a duplicate of https://github.com/astral-sh/ty/issues/1644 and/or https://github.com/astral-sh/ty/issues/2154? Is either the superclass method or the subclass method overloaded, by any chance?

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-25 00:42_

---

_Comment by @pjonsson on 2025-12-25 10:45_

I'm not familiar with the mechanics of overload in Python, but I did a `git grep` and there is no `overload` decorator anywhere in the source code (plenty of overrides though, but I assume that wasn't what you meant since you asked about overloads). It predates me, but the code base hasn't had type annotations for that long, so it's not impossible that other decorators are missing if the code would run fine without them.

It's also possible I'm not reading the error message from Ty right, here is the entire output when type checking the file:
```
$ uv run ty check datacube/index/postgres/_products.py
error[missing-argument]: No argument provided for required parameter `name` of function `get_by_name_unsafe`
   --> datacube/index/postgres/_products.py:228:20
    |
226 |         _LOG.info("Updating product %s", product.name)
227 |
228 |         existing = self.get_by_name_unsafe(product.name)
    |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
229 |         changing_metadata_type = (
230 |             product.metadata_type.name != existing.metadata_type.name
    |
info: Union variant `def get_by_name_unsafe(self, name: str) -> Product` is incompatible with this call site
info: Attempted to call union type `(bound method Self@update.get_by_name_unsafe(name: str) -> Product) | (def get_by_name_unsafe(self, name: str) -> Product)`
info: rule `missing-argument` is enabled by default

error[invalid-argument-type]: Argument to function `get_by_name_unsafe` is incorrect
   --> datacube/index/postgres/_products.py:228:44
    |
226 |         _LOG.info("Updating product %s", product.name)
227 |
228 |         existing = self.get_by_name_unsafe(product.name)
    |                                            ^^^^^^^^^^^^ Expected `ProductResource`, found `str`
229 |         changing_metadata_type = (
230 |             product.metadata_type.name != existing.metadata_type.name
    |
info: Function defined here
   --> datacube/index/postgres/_products.py:349:9
    |
347 |     # pylint: disable=method-hidden
348 |     @override
349 |     def get_by_name_unsafe(self, name: str) -> Product:
    |         ^^^^^^^^^^^^^^^^^^ ---- Parameter declared here
350 |         with self._db_connection() as connection:
351 |             result = connection.get_product_by_name(name)
    |
info: Union variant `def get_by_name_unsafe(self, name: str) -> Product` is incompatible with this call site
info: Attempted to call union type `(bound method Self@update.get_by_name_unsafe(name: str) -> Product) | (def get_by_name_unsafe(self, name: str) -> Product)`
info: rule `invalid-argument-type` is enabled by default

error[invalid-method-override]: Invalid override of method `get_unsafe`
   --> datacube/index/postgres/_products.py:339:9
    |
337 |     # pylint: disable=method-hidden
338 |     @override
339 |     def get_unsafe(self, id_: int) -> Product:
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `AbstractProductResource.get_unsafe`
340 |         with self._db_connection() as connection:
341 |             result = connection.get_product(id_)
    |
   ::: datacube/index/abstract/_products.py:335:9
    |
334 |     @abstractmethod
335 |     def get_unsafe(self, id_: int) -> Product:
    |         ------------------------------------- `AbstractProductResource.get_unsafe` defined here
336 |         """
337 |         Fetch product by id
    |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

error[invalid-method-override]: Invalid override of method `get_by_name_unsafe`
   --> datacube/index/postgres/_products.py:349:9
    |
347 |     # pylint: disable=method-hidden
348 |     @override
349 |     def get_by_name_unsafe(self, name: str) -> Product:
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `AbstractProductResource.get_by_name_unsafe`
350 |         with self._db_connection() as connection:
351 |             result = connection.get_product_by_name(name)
    |
   ::: datacube/index/abstract/_products.py:345:9
    |
344 |     @abstractmethod
345 |     def get_by_name_unsafe(self, name: str) -> Product:
    |         ---------------------------------------------- `AbstractProductResource.get_by_name_unsafe` defined here
346 |         """
347 |         Fetch product by name
    |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

Found 4 diagnostics
```


---

_Comment by @AlexWaygood on 2025-12-25 10:50_

Is it possible that you have two classes named `Product`, one in one file and one in another...?

Do mypy and/or pyright complain about this override, or only ty?

---

_Comment by @pjonsson on 2025-12-25 11:10_

Yes, there are 3 classes named `Product`, one in a separate driver that shouldn't leak into the postgres driver, and two other definitions.
```
datacube/model/__init__.py:class Product:
datacube/virtual/impl.py:class Product(VirtualProduct):  # type: ignore[no-redef]
```
Most of the code base has type annotations now, and all of it type checks with mypy (but there are some ignores as you can see). The file in question also comes out clean when fed into pyright, but pyright has (what looks like valid) complaints for other parts of the code base.

And I know the virtual stuff predates the type annotations by years, so it does "interesting" things.

When looking around, I also found this in the constructor of the class that it complains about:
```python
        self.get_unsafe = lru_cache()(self.get_unsafe)  # type: ignore[method-assign]
        self.get_by_name_unsafe = lru_cache()(self.get_by_name_unsafe)  # type: ignore[method-assign]
```
which coincides with the methods that Ty is complaining about.

So I guess I bumped into something that isn't supported by Ty yet, but I think my suggestion to mark the incompatible part of the signature instead of the entire signature would still be helpful.

---

_Comment by @AlexWaygood on 2025-12-25 12:35_

> I think my suggestion to mark the incompatible part of the signature instead of the entire signature would still be helpful.

Yes, I agree, and that's already tracked in #1644 :-) I was just trying to see if we could figure out what the cause of this specific diagnostic was. 

---

_Closed by @AlexWaygood on 2025-12-25 12:35_

---
