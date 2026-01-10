---
number: 20637
title: TC001-3 runtime-evaluated-decorators not working when an instance is created via a function.
type: issue
state: open
author: eseglem
labels:
  - documentation
assignees: []
created_at: 2025-09-29T18:57:40Z
updated_at: 2025-11-06T13:28:44Z
url: https://github.com/astral-sh/ruff/issues/20637
synced_at: 2026-01-10T01:23:01Z
---

# TC001-3 runtime-evaluated-decorators not working when an instance is created via a function.

---

_Issue opened by @eseglem on 2025-09-29 18:57_

### Summary

Using FastAPI routers that have been created via a function seem to result in TC001-3 errors where using the class directly does not. I tried dozens of iterations of settings in my actual project to try and find one that worked, but was unable to find anything that worked. I have set up a few very minimal examples re-creating the issue in the Ruff Playground.

- Calling `APIRouter` directly works correctly with no errors: https://play.ruff.rs/2d99831a-42e9-44fe-818a-2a4a15b2a3ea
- Using a function to create the `APIRouter` results in errors: https://play.ruff.rs/86ca5e05-b4dd-4b3a-a1b9-5eff615478f9

~~I am less confident it is not a settings issue related to naming in a single file test app, but I also just tried it as a subclass of `APIRouter` which results in the same errors: https://play.ruff.rs/5ba29657-3a1f-4b2e-9478-94df4c6581df~~ It was a settings issue fixed by using `"<filename>.MyAPIRouter.get"` in the settings. 

### Version

v0.13.2

---

_Comment by @ntBre on 2025-09-29 20:23_

Thanks for the report! I poked around a bit, and it looks like this is a limitation in how we resolve decorators:

https://github.com/astral-sh/ruff/blob/00927943026af2a2a5071acc5d35879579a4d410/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L181-L191

Possibly we could also try to resolve the type too, but I did find a workaround. If I put your function example into a file called `try.py`, I can avoid the diagnostic by setting `runtime-evaluated-decorators` to `["try.new_router.get"]`. Basically, as shown in the code above, we resolve the path to the attribute, including the current file's module and the function, rather than resolving the type annotations on the function.

This is a pretty deep implementation detail, but this works in the [playground](https://play.ruff.rs/dc82080d-9897-4b8f-b9cb-9f1b63aa1d1a) with `<filename>` as the module name too.

---

_Label `question` added by @ntBre on 2025-09-29 20:24_

---

_Comment by @eseglem on 2025-09-29 21:43_

Thank you for the response and the workaround. I can confirm that the workaround works. Adding `["router.new_router.get", "router.new_router.post", ...]` to the `runtime-evaluated-decorators` does stop the errors from occurring.

I was focused on the typing of the router, and adding the instances of it to the settings (like `"<filename>.router.get"`). It did not even cross my mind to add the function to the settings like that.

As a user I was expecting it to resolve the type and only need to add the `fastapi.APIRouter.*` decorators to the settings. If this is not going to change, I think it could be worth updating the docs to explain it does not resolve the type and include some additional examples such as this one.

I am sure it has its own issues, but the other thing I had been hoping would work, was a regex (`["*.get", "*.post", ...]`) for the decorators. Especially when I was thinking I needed to add every instance of the router.

---

_Comment by @ntBre on 2025-09-29 21:55_

No problem, glad it helped!

I found it surprising that we weren't resolving the types too. It might be worth looking into that, but updating the docs seems like a good idea if not.

There was some previous discussion of a regex on the PR adding the snippet above (https://github.com/astral-sh/ruff/pull/15204#issuecomment-2565732182) and in https://github.com/astral-sh/ruff/pull/15060#issuecomment-2565719324. It seems like we were hesitant to add it at the time, but maybe we can revisit it if checking the types doesn't pan out.

---

_Referenced in [astral-sh/ruff#21282](../../astral-sh/ruff/issues/21282.md) on 2025-11-05 21:43_

---

_Label `question` removed by @MichaReiser on 2025-11-06 13:28_

---

_Label `documentation` added by @MichaReiser on 2025-11-06 13:28_

---
