```yaml
number: 19130
title: "[ty] Add into_callable method for Type"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: into-callable
created_at: 2025-07-03T18:28:40Z
updated_at: 2025-07-16T21:12:31Z
url: https://github.com/astral-sh/ruff/pull/19130
synced_at: 2026-01-12T15:56:32Z
```

# [ty] Add into_callable method for Type

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Was just playing around with this, there's definitely more to do with this function, but it seems like maybe a better option than having so many arms in has_relation_to for (_, Callable).

Not super invested in this but wanting to hear opinions if people have time

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-03 18:28_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-03 18:28_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-03 18:28_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-03 18:28_

---

_Renamed from "Add into_callable method for Type" to "[ty] Add into_callable method for Type" by @MatthewMckee4 on 2025-07-03 18:30_

---

_Comment by @github-actions[bot] on 2025-07-03 18:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~45MB
+     memo fields = ~41MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~54MB
+     memo fields = ~60MB

operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB
-     memo fields = ~88MB
+     memo fields = ~80MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

werkzeug (https://github.com/pallets/werkzeug)
-     memo fields = ~129MB
+     memo fields = ~142MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-07-03 18:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1246 on 2025-07-03 19:37_

```suggestion
            Type::Union(union) => union.try_map(db, |element| element.into_callable(db)),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1470 on 2025-07-03 19:39_

```suggestion
            (_, Type::Callable(_)) => self.into_callable(db).is_some_and(|callable| callable.has_relation_to(db, target, relation)),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1248 on 2025-07-03 19:40_

Could you make this an exhaustive match, so that we don't forget to add branches to it in the future when we add new `Type` variants?

I think there are some branches that we're obviously missing currently too which should return `Some()`, such as `DataclassDecorator`, `TypeVar` and `Intersection`. These don't seem to have any ecosystem hits or mdtest failures so I think it's okay to leave them out for now, but adding a TODO for them would be good, I think.

---

_@AlexWaygood reviewed on 2025-07-03 19:44_

This is great! I think it's exactly the direction we want to head in, as it will make subtyping/assignability between various types and protocols with `__call__` methods easier too.

---

_@AlexWaygood reviewed on 2025-07-03 19:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1229 on 2025-07-03 19:49_

I think for `type[Any]`, `type[Unknown]` and `type[Todo]`, we actually want to return `Some(ty)`, where `ty` is `Callable[..., <that dynamic type>]`. `type[Any]` represents a gradual, unknown subclass of type that we need to treat as permissively as possible. So that would imply:

```suggestion
            Type::SubclassOf(subclass_of_ty) => match subclass_of_ty.subclass_of() {
                SubclassOfInner::Class(class) => class.into_callable(db),
                SubclassOfInner::Dynamic(dynamic) => Callable::single(db, Signature::new(Parameters::unknown(), dynamic))
            }
```

---

_@MatthewMckee4 reviewed on 2025-07-03 19:59_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1248 on 2025-07-03 19:59_

Do you want me to add todo in all arms? or the ones im unsure about at least?

---

_Comment by @MatthewMckee4 on 2025-07-03 20:11_

Seems like they're caught in mypy primer, ill make them return None but add a todo comment

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1195 on 2025-07-04 01:43_

I don't think this is the right signature for the dynamic callable type. We should use `Signature::dynamic(self)` here.
```suggestion
                Signature::dynamic(self),
```

---

_@carljm approved on 2025-07-04 01:47_

This looks great, thank you!

cc @abhijeetbodas2001, I think my review comments on your returns-Never PR mentioned wanting this method.

---

_Merged by @carljm on 2025-07-04 02:04_

---

_Closed by @carljm on 2025-07-04 02:04_

---

_Branch deleted on 2025-07-16 21:12_

---
