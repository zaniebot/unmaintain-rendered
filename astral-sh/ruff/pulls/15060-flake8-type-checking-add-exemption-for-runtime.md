```yaml
number: 15060
title: "[`flake8-type-checking`] Add exemption for runtime evaluated decorator classes"
type: pull_request
state: closed
author: viccie30
labels:
  - configuration
assignees: []
base: main
head: runtime-evaluated-decorator-classes
created_at: 2024-12-19T12:09:22Z
updated_at: 2024-12-31T14:15:39Z
url: https://github.com/astral-sh/ruff/pull/15060
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-type-checking`] Add exemption for runtime evaluated decorator classes

---

_@viccie30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR makes ruff recognize functions decorated with methods,
like FastAPI uses to specify endpoints.

I've implemented the check by generalizing
`ruff_linter::src::rules::fastapi::rules::is_fastapi_route_call` to
recognize methods of specific classes used as decorators.

## Test Plan

I have added tests by duplicating and adjusting
`crates/ruff_linter/resources/test/fixtures/flake8_type_checking/runtime_evaluated_decorators_{1..3}.py`.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-19 12:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-12-19 12:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/annotation.rs`:93 on 2024-12-19 12:25_

Nit: I suggest passing the `flake8_type_checking` settings instead of passing every setting individually

---

_Comment by @MichaReiser on 2024-12-19 12:27_

Would this address https://github.com/astral-sh/ruff/issues/13713 ?

---

_Comment by @viccie30 on 2024-12-19 12:29_

> Would this address #13713 ?

I think so. 

---

_Review comment by @viccie30 on `crates/ruff_linter/src/checkers/ast/annotation.rs`:93 on 2024-12-19 12:30_

I wanted to make as few changes as possible, but I can change this, of course.

---

_@viccie30 reviewed on 2024-12-19 12:30_

---

_Comment by @MichaReiser on 2024-12-19 12:32_

Thank you. I think this makes sense.

@Daverball, I'd be interested in your thoughts on this change because you're the most familiar with our type-checking rules.

---

_@viccie30 reviewed on 2024-12-19 12:55_

---

_Review comment by @viccie30 on `crates/ruff_linter/src/checkers/ast/annotation.rs`:93 on 2024-12-19 12:55_

I have changed this, and the callers, too.

---

_Comment by @Daverball on 2024-12-19 12:57_

This looks good to me. The flake8 plugin currently does something less clever for FastAPI support, with a toggle that you have to enable if you use FastAPI, which increases false negatives in the rest of your code. This approach seems more balanced and flexible.

My only concern is that we probably want to add additional settings to support marker generics like `sqlalchemy.orm.Mapped`, which require some or all of the symbols to be available at runtime. I think FastAPI also has some things like that. So at that point we'd have at least five very similar options, which begs the question of whether there's maybe a better way to organize them.

---

_Comment by @viccie30 on 2024-12-19 13:19_

> My only concern is that we probably want to add additional settings to support marker generics like `sqlalchemy.orm.Mapped`, which require some or all of the symbols to be available at runtime. I think FastAPI also has some things like that. So at that point we'd have at least five very similar options, which begs the question of whether there's maybe a better way to organize them.

I had throught about rolling this option into `runtime-evaluated-decorators` by adding a fallback from matching any identifier in the list literally to matching any value that is initialized by any identifier in the list, but I thought separating the two was cleaner.

---

_Comment by @Daverball on 2024-12-19 13:21_

The one thing this currently fails to cover is sharing your instances between modules:

```python
from datetime import datetime
from mymodule import app

@app.put("/datetime")
def set_datetime(value: datetime) -> None:
    pass
```

The question is if that matters, since red knot presumably will be able to handle that case in the future.

---

But we could also support that use-case by extending `runtime-evaluated-decorators` to match if the dotted name starts the same way, rather than only when it matches exactly:

```toml
[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-decorators = ["mymodule.app"]
```

would match `@mymodule.app` but also `@mymodule.app.route`

---

_Comment by @viccie30 on 2024-12-19 13:25_

> The one thing this currently fails to cover is sharing your instances between modules:
> 
> ```python
> from datetime import datetime
> from mymodule import app
> 
> @app.put("/datetime")
> def set_datetime(value: datetime) -> None:
>     pass
> ```
> 
> The question is if that matters, since red knot presumably will be able to handle that case in the future.
> 
> But we could also support that use-case by extending `runtime-evaluated-decorators` to match if the dotted name starts the same way, rather than only when it matches exactly:
> 
> ```toml
> [tool.ruff.lint.flake8-type-checking]
> runtime-required-decorators = ["mymodule.app"]
> ```
> 
> would match `@mymodule.app` but also `@mymodule.app.route`

I had thought about how to add that possibility, but I could not find a clean way. If this is something you would like to see as you sketched it above, I'd be happy to add it.

---

_Comment by @Daverball on 2024-12-19 13:52_

> If this is something you would like to see as you sketched it above, I'd be happy to add it.

I think that would be enough to cover most FastAPI use-cases, so it seems like a desirable improvement to the semantics of that setting. Although it might require some documentation improvements, so people understand, that this is something they can do.

It might also be interesting to investigate whether we can get away with just the original setting with this change, as long as we can match the binding in the module it was defined to the fully qualified name. I.e.

`mymodule/__init__.py`
```python
from fastapi import FastAPI as Api

app = Api()

@app.put("/datetime")  # matches "mymodule.app" because we're in `mymodule`.
def set_datetime(value: datetime) -> None:
    pass
```

---

_Comment by @Daverball on 2024-12-19 14:00_

Actually, the other use-case can already be supported by adding `mymodule.app.put` etc. although that would be quite tedious and error-prone, so maybe it makes more sense to support glob patterns, i.e. `mymodule.app.*`, rather than arbitrary sub-matches.

---

_Comment by @viccie30 on 2024-12-19 14:11_

> Actually, the other use-case can already be supported by adding `mymodule.app.put` etc. although that would be quite tedious and error-prone, so maybe it makes more sense to support glob patterns, i.e. `mymodule.app.*`, rather than arbitrary sub-matches.

Would you like me to add that to this PR or open another PR for that? And do you still want to merge this PR?

---

_Comment by @Daverball on 2024-12-19 14:30_

I have no merging power. All I can give is my opinion.

I think the feature is fine the way you implemented it, but if we can support this use-case with only the existing setting by slightly changing the semantics, that would be even better, since we wouldn't need to rely on a potentially expensive `resolve_assignment` that way and can keep the configuration more simple.

If you feel like experimenting with the suggested approach in a separate PR, feel free. But please don't feel compelled to invest the time. I'm happy to try it myself, although I definitely won't get around to it today.

@MichaReiser Care to chime in, with how you would like this to move forward?

---

_Label `configuration` added by @MichaReiser on 2024-12-30 16:23_

---

_Comment by @MichaReiser on 2024-12-30 17:01_

Sorry, I was out for most days last week and I also needed some time to familiarize myself with the subject. 

Adding regex support is an option but I'm a bit hesitant of doing so because a) it requires matching on the qualified names string representation which requires allocating a `String` and most (if not all) other qualified name settings don't support it. We could consider only testing if the qualified name starts with the same segments, but this seems like a possible footgun to me which is why I think that having to list all methods might be better for now (related issue https://github.com/astral-sh/ruff/pull/15060).

I'm not that much concerned about `resolve_assignment`. We use it in other rules already. But I'm interested in exploring ways to avoid introducing a new option because it's not very obvious which one a user has to pick.

I'm probably overlooking something, but could we combine the two options and change `required_decorators` to: 

```rust
fn runtime_required_decorators(
    decorator_list: &[Decorator],
    decorators: &[String],
    semantic: &SemanticModel,
) -> bool {
    if decorators.is_empty() {
        return false;
    }

    decorator_list.iter().any(|decorator| {
        let callable = map_callable(&decorator.expression);

        if let Expr::Attribute(ast::ExprAttribute { value, .. }) = callable {
            if let Some(qualified_name) = resolve_assignment(value, semantic) {
                return decorators.iter().any(|decorator_class| {
                    QualifiedName::from_dotted_name(decorator_class) == qualified_name
                });
            }
        }

        semantic
            .resolve_qualified_name(callable)
            .is_some_and(|qualified_name| {
                decorators
                    .iter()
                    .any(|base_class| QualifiedName::from_dotted_name(base_class) == qualified_name)
            })
    })
}
```

Or are there cases where the resolved qualified name wouldn't be unique?

---

_Comment by @viccie30 on 2024-12-30 17:51_

> Or are there cases where the resolved qualified name wouldn't be unique?

I don't think so, unless someone would want to have
```python
var = Class()

@var.method
def function():
    ....
```
match and
```python
var = Class()

@Class
def function():
    ....
```
not match, but I don't think that will be an issue in practice.

I added a new option because I thought there might be confusion otherwise, but looking at it now I think adding this to the existing check is fine.

---

_Comment by @MichaReiser on 2024-12-31 14:11_

Thank you @viccie30 for putting up this PR. @Daverball followed up on the discussion and created a PR that extends the existing option to cover class methods too. See https://github.com/astral-sh/ruff/pull/15204

I'll close this PR in favor of this new PR as it achieves the same end-goal but without introducing a new setting. Thanks again for PRing this change and initiating the discussion!

---

_Closed by @MichaReiser on 2024-12-31 14:11_

---

_Branch deleted on 2024-12-31 14:15_

---
