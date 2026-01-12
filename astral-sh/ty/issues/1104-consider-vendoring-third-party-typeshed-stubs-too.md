```yaml
number: 1104
title: Consider vendoring third-party typeshed stubs, too
type: issue
state: open
author: aidandj
labels:
  - needs-decision
assignees: []
created_at: 2025-08-28T16:22:54Z
updated_at: 2025-11-14T17:45:10Z
url: https://github.com/astral-sh/ty/issues/1104
synced_at: 2026-01-12T15:54:24Z
```

# Consider vendoring third-party typeshed stubs, too

---

_@aidandj_

### Summary

Given this small code block:

```
import grpc

try:
    something = 1
except grpc.RpcError as e:
    e.code()
```

I'm getting this error: `warning[unresolved-attribute]: Type `RpcError` has no attribute `code``

This type exists [in typeshed](https://github.com/python/typeshed/blob/c0fed911d64e483e89dc2190aad057f93fc431d3/stubs/grpcio/grpc/__init__.pyi#L325) and pyright seems to pick it up and does not complain. But `ty` does.

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Comment by @AlexWaygood on 2025-08-28 16:41_

Thanks for the report!

> This type exists [in typeshed](https://github.com/python/typeshed/blob/c0fed911d64e483e89dc2190aad057f93fc431d3/stubs/grpcio/grpc/__init__.pyi#L325) and pyright seems to pick it up and does not complain. But `ty` does.

The difference here is that pyright vendors all of typeshed's third-party stubs. Ty instead currently follows mypy's model of only vendoring typeshed's stdlib stubs. That means that we're using the runtime source code for the `grpcio` library rather than typeshed's stubs for `grpcio`. The good news, however, is that typeshed publishes its `grpcio` stubs to PyPI -- installing `types-grpcio` into your Python environment before running ty should get rid of the `AttributeError`. If you're invoking ty using uv and you have it listed as a dev-dependency, it might be as simple as listing `types-grpcio` as a dev-dependency too -- or you could run `uv run --with=types-grpcio ty check` rather than `uv run ty check`.

---

_Comment by @AlexWaygood on 2025-08-28 16:59_

Ideally I feel like we'd emit a better diagnostic here, but I'm not totally sure how. It would be fairly easy to have an up-to-date list of third-party modules for which typeshed stubs exist, and suggest to the user that they install them whenever we emit an `unresolved-import` diagnostic for one of them. (We would update the list every time we sync our vendored copy of typeshed's stubs.) But this situation doesn't result in an `unresolved-import` diagnostic (`grpcio` is not a C extension), it results in an `unresolved-attribute` diagnostic. Installing typeshed's stubs for a dependency _might_ help with an `unresolved-attribute` diagnostic, but in some cases it also might hurt -- you can't say for sure like you can with an `unresolved-import` diagnostic.

We also haven't ruled out vendoring all of typeshed's third-party stubs, like pyright does. It would obviously have the benefit that it's what users of pyright will probably expect. It has the disadvantage that it would increase our binary size and possibly make it harder to reproduce type-checking results across machines.

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-28 17:00_

---

_Comment by @carljm on 2025-08-28 17:01_

> and possibly make it harder to reproduce type-checking results across machines.

Why would it have that effect?

---

_Label `needs-decision` added by @carljm on 2025-08-28 17:01_

---

_Comment by @aidandj on 2025-08-28 17:04_

Thanks for the clarification. I actually tried running it using the `--typeshed` argument, and got the same error. I now read the documentation closer and see that it does clarify that the typeshed argument only applies to stdlib.

Although I probably agree that installing individual 3rd party stubs is the "correct" approach, I foresee a very large number of duplicate issues as people make a switch from pyright to ty. I like the idea of keeping a list of 3rd party stubs as things get vendored. That would let you report an error similar to one that typescript sometimes throws, "have you tried installing `types-<package-x>`"

If typeshed is already being vendored, then it isn't really an added complication to maintain that list, once code is written once.

---

_Comment by @AlexWaygood on 2025-08-28 17:06_

>> and possibly make it harder to reproduce type-checking results across machiness
>
> Why would it have that effect?

Different versions of ty might have different versions of the grpcio stubs vendored with them, but it might not be immediately obvious to users that two different developers are implicitly using two different versions of the grpcio stubs because one of them hasn't upgraded to the latest version of ty.

Forcing users to explicitly list a stubs package as a dev-dependency means that the user knows exactly what version of the stubs they're using, it means that the stubs package is explicitly listed with its version if a user does something like `uv pip list`, and it means that a team of people all people working on the project can easily ensure they'll all be using exactly the same version of those stubs (it'll be pinned in their `uv.lock` file if they're using uv!).

---

_Comment by @carljm on 2025-08-28 17:10_

Ah, ok, I wondered if you were thinking of differing ty versions. I'm not sure how much to weight that as a concern -- it's inherently the case (just because of behavior changes in ty itself!) that if you have different developers using different ty versions, they're likely to get differing results.

---

_Comment by @AlexWaygood on 2025-08-28 17:12_

Yeah -- FWIW, the last time we had this discussion as a team I argued that LSP users would probably be expecting to have all of typeshed's third-party stubs vendored, like pyright, and that the benefits of doing so probably outweighed the costs given that we wanted ty to work great as an LSP as well as a CLI tool. I think @MichaReiser was the strongest voice arguing against doing that for now.

---

_Comment by @aidandj on 2025-08-28 17:13_

FWIW, users should also be locking the `ty` version if they want to maintain consistency between runs. This can be an issue when just using an LSP, unless the LSP can use the locally installed version. So if one were to lock the `ty` version that would provide the same version locking as locking individual stub packages.

---

_Comment by @AlexWaygood on 2025-08-28 17:16_

And vendoring the third-party stubs doesn't prevent a user from installing a different version of those stubs if they want to. The installed third-party stubs would take precedence over any vendored third-party stubs, as per the [spec](https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering)

---

_Renamed from "Typeshed type not used for grpc.RpcError" to "Consider vendoring third-party typeshed stubs, too" by @carljm on 2025-08-28 17:16_

---

_Label `diagnostics` removed by @AlexWaygood on 2025-08-28 17:18_

---

_Comment by @MichaReiser on 2025-09-15 08:45_

> I think @MichaReiser was the strongest voice arguing against doing that for now.

The concern I raised was that I'm not sure if bundling all stubs with ty is the right solution if we're mainly concerned about LSP only users where we could bundle the stubs as part of the VS Code extension instead (or download them on demand or something else)

---

_Comment by @aidandj on 2025-09-15 13:17_

I think having differing output when running ty check, and LSP output is confusing behavior. Someone is going to see no errors in Vscode, push code, and have CI fail.

---

_Comment by @MichaReiser on 2025-09-15 13:40_

Another option that we discussed is to show a code suggestion to install the missing typing stubs (where an option could also be to just download all and don't install them into a venv for users that don't use a venv).

I'd prefer this because I would prefer to pin my stubs over accidentially use a stub that comes from ty (unless there's some opt-out option but that seems the wrong way round to me)

---

_Comment by @zanieb on 2025-10-02 13:45_

We could make it easy to add associated type stub packages when adding a dependency in uv 🤔 

---

_Comment by @aidandj on 2025-10-02 14:00_

If you are maintaining a list of available types, isn't that most of the way to vendoring them?

---

_Comment by @MichaReiser on 2025-10-02 14:32_

> If you are maintaining a list of available types, isn't that most of the way to vendoring them?

It's different in that they are versioned with your project rather than that you get whatever version ty ships (which might not match your package version)

---

_Comment by @aidandj on 2025-10-02 15:40_

But you can always override with an installed stub

---

_Comment by @MichaReiser on 2025-10-02 15:48_

Sure, but it's the implicit nature I'm worried about. There should at least be a config option to disable this behavior (which we might even want to disable by default). 

---

_Comment by @aidandj on 2025-10-02 16:12_

> There should at least be a config option to disable this behavior (which we might even want to disable by default). 

This would get my vote. As long as I could configure it in pyproject.toml. would make a big difference for moving pyright projects to ty



---

_Added to milestone `Stable` by @carljm on 2025-11-14 17:43_

---

_Comment by @carljm on 2025-11-14 17:44_

Marking this for stable release as I think we need a decision here one way or the other by stable (as part of considering the full picture of https://github.com/astral-sh/ty/issues/1537); still marked `needs-decision`, marking this issue for stable release doesn't mean we are definitely going to do it

---
