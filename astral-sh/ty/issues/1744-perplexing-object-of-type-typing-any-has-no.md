```yaml
number: 1744
title: "Perplexing \"Object of type `typing.Any` has no attribute\""
type: issue
state: closed
author: alex
labels:
  - diagnostics
assignees: []
created_at: 2025-12-03T13:57:28Z
updated_at: 2025-12-03T20:42:22Z
url: https://github.com/astral-sh/ty/issues/1744
synced_at: 2026-01-12T15:54:25Z
```

# Perplexing "Object of type `typing.Any` has no attribute"

---

_@alex_

### Summary

I apologies because I don't have a minimal reproducer for this! I'm seeing the following errors with `ty`:

```
error[unresolved-attribute]: Object of type `typing.Any` has no attribute `string`
  --> src/cryptography/hazmat/bindings/openssl/binding.py:90:26
   |
88 |     # shared object that were loaded have the same version and raise an
89 |     # ImportError if they do not
90 |     so_package_version = _openssl.ffi.string(
   |                          ^^^^^^^^^^^^^^^^^^^
91 |         _openssl.lib.CRYPTOGRAPHY_PACKAGE_VERSION
92 |     )
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Object of type `typing.Any` has no attribute `CRYPTOGRAPHY_PACKAGE_VERSION`
  --> src/cryptography/hazmat/bindings/openssl/binding.py:91:9
   |
89 |     # ImportError if they do not
90 |     so_package_version = _openssl.ffi.string(
91 |         _openssl.lib.CRYPTOGRAPHY_PACKAGE_VERSION
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
92 |     )
93 |     if version.encode("ascii") != so_package_version:
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Object of type `typing.Any` has no attribute `OpenSSL_version_num`
   --> src/cryptography/hazmat/bindings/openssl/binding.py:104:9
    |
103 |     _openssl_assert(
104 |         _openssl.lib.OpenSSL_version_num() == openssl.openssl_version(),
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
105 |     )
    |
info: rule `unresolved-attribute` is enabled by default

```

Right off the bat I'm confused, because my understanding of the `Any` type is that all operations are valid on it :-) I also can't seem to create a minimal reproducer out of it. I'm wondering if this is somehow related to #136 and declared vs. observed field types, but teh error doesn't give me a lot to go on.

This can be reproduced in https://github.com/pyca/cryptography with `ty check src/cryptography/ vectors/cryptography_vectors/ tests/ release.py noxfile.py`

### Version

ty 0.0.1-alpha.30 (d5754d3b5 2025-12-03)

---

_Comment by @AlexWaygood on 2025-12-03 14:04_

There are two types associated with `Any`, and I think (at least one) issue here is that we're doing a terrible job at distinguishing between the two types in our diagnostic here, which results in a very confusing message :-)

The first type is the one you're thinking of: if an object _inhabits_ the type `Any`, then it of course has arbitrary attributes:

```py
from typing import Any

def f(x: Any):
    x.foooooooooooooooofaaaaaaaaaaaaaa  # fine
```

but there is also the type of the symbol `typing.Any` itself! The runtime symbol `typing.Any` doesn't inhabit the type `Any`: it's obviously the case that if you do `x = Any` at runtime, `x` does not have arbitrary attributes:

```py
>>> from typing import Any
>>> x = Any
>>> x.foooooofarrrr
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    x.foooooofarrrr
AttributeError: type object 'Any' has no attribute 'foooooofarrrr'
```

Because this symbol requires so much extensive special casing in ty, we infer this symbol as inhabiting a singleton type that has exactly one inhabitant (the symbol `typing.Any`). And we then give it a very confusing `Display` in diagnostics (`typing.Any` for the singleton definition-of-Any type, as opposed to `Any` for the type `Any` itself.)

So I _suspect_ that _somewhere_ you (or a third-party package you depend on) has done `x = Any` where you/they _meant_ to do `x: Any`. But we should definitely improve our diagnostics here, in multiple ways :-)

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-03 14:04_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-03 14:27_

---

_Comment by @alex on 2025-12-03 14:29_

Ohhhh man, if that's what's happening a) I'll be extremely mortified, b) please consider this an diagnostic bug :D

---

_Comment by @alex on 2025-12-03 14:30_

https://github.com/pyca/cryptography/blob/main/src/cryptography/hazmat/bindings/_rust/_openssl.pyi 🙃

Great call.

---

_Comment by @alex on 2025-12-03 14:38_

Notwithstanding the confusing error message here, I think it's pretty great that ty is finding bugs in our type annotations. Well done!

(I believe that once https://github.com/pyca/cryptography/pull/13965 lands our only remaining blocker to using ty will be https://github.com/astral-sh/ty/issues/1112)

---

_Comment by @AlexWaygood on 2025-12-03 17:29_

Here's an example of what our diagnostics will look like if https://github.com/astral-sh/ruff/pull/21777 is merged:

```
error[unresolved-attribute]: Special form `typing.Any` has no attribute `foo`
 --> foo.py:5:1
  |
3 | X = typing.Any
4 |
5 | X.foo
  | ^^^^^
  |
help: Objects with type `Any` have a `foo` attribute, but the symbol `typing.Any` does not itself inhabit the type `Any`
help: This error may indicate that `X` was defined as `X = typing.Any` when `X: typing.Any` was intended
info: rule `unresolved-attribute` is enabled by default
```

with colour:

<img width="2198" height="462" alt="Image" src="https://github.com/user-attachments/assets/6c95864c-7113-4b75-96ef-e8073b6e64fd" />

I've also filed https://github.com/astral-sh/ruff/pull/21775 to disambiguate the display of our special-form singleton types in other situations too

---

_Comment by @alex on 2025-12-03 17:34_

I promise I'll have no excuse in the future, because the help text is quite clear :-)

---

_Closed by @AlexWaygood on 2025-12-03 20:42_

---
