```yaml
number: 15364
title: "[`fastapi`] Handle parameters with `Depends` correctly (`FAST003`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: FAST003
created_at: 2025-01-09T03:13:05Z
updated_at: 2025-01-13T08:53:42Z
url: https://github.com/astral-sh/ruff/pull/15364
synced_at: 2026-01-10T20:34:00Z
```

# [`fastapi`] Handle parameters with `Depends` correctly (`FAST003`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-09 03:13_

## Summary

Resolves #13657.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-09 03:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 03:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:226 on 2025-01-09 07:59_

I'd call this `Dependency` to align it with the terminology used by fastapi https://fastapi.tiangolo.com/reference/dependencies

Dependable also sort of means something different [source](https://dictionary.cambridge.org/dictionary/english/dependable)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:142 on 2025-01-09 08:01_

Considering that you filter out `Dependable::None` here, I'd suggest to change `Dependable::from_paramter` to return an `Option<Dependable>` instead. It has the advantage (other than just being less code) that the compiler can prove, that `Dependable::None` is not a valid value after line 139

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:150 on 2025-01-09 08:03_

Using `collect` only to bail in the case of `Unknown` seems unfortunate. I'd prefer to rewrite the `dependables_parameter_names` like this

```rust
let mut dependable_paramter_names = Vec::new();

for dependable in dependables {
	if let Some(parameter_names) = dependable.parameter_names() {
		dependable_parameter_names.extend(paramter_names);
	} else {
		return;
	}
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:219 on 2025-01-09 08:05_

I find the name of this function rather confusing because `keywordable` isn't a term I've come across before. I'd prefer inlining the code again. I found that clearer in the absence of a better term

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:158 on 2025-01-09 08:07_

I think we should just collect the dependencies directly into the `named_args` vector instead of building a second vector (and iterating over all parameters again). 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:321 on 2025-01-09 08:09_

```suggestion
        let scope = &semantic.scopes[scope_id];
```

---

_@MichaReiser requested changes on 2025-01-09 08:10_

Nice. A few comments related to how the code is structured and the used terminology

---

_Label `rule` added by @MichaReiser on 2025-01-09 08:10_

---

_Label `preview` added by @dhruvmanila on 2025-01-09 08:20_

---

_@InSyncWithFoo reviewed on 2025-01-09 12:22_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:226 on 2025-01-09 12:22_

(I took that term from [this section header](https://fastapi.tiangolo.com/tutorial/dependencies/#create-a-dependency-or-dependable).) Renamed.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:142 on 2025-01-09 13:57_

Nice. I like the new name.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:161 on 2025-01-09 14:02_

Just for my understanding. Is this an error because fastapi only respects the first `Depends`? Do you have a reference that documents this behavior or did you find it out by try and error?

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:151 on 2025-01-09 14:03_

Nit: Can we rename this to `dependency_with_thing_id_param` and `dependency_without_thing_id_param`

---

_@MichaReiser approved on 2025-01-09 14:05_

This is great. One small nit and an understand question from my end.

---

_Unassigned @charliermarsh by @charliermarsh on 2025-01-09 14:15_

---

_@InSyncWithFoo reviewed on 2025-01-09 14:44_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:161 on 2025-01-09 14:44_

The documentation doesn't mention what happens when two `Depends` are used in a <i>path operation function</i>, only when [multiple of them are used in a <i>path operation decorator</i>](https://fastapi.tiangolo.com/tutorial/dependencies/dependencies-in-path-operation-decorators).

Testing this again, I think I made a mistake. FastAPI seems to respect the last rather than the first. The code in question:

```python
from fastapi import FastAPI, Depends
from typing import Annotated

app = FastAPI()

def foo(a: str) -> str:
    print(f'foo: {a}')
    return 'lorem'
    
def bar(b: str) -> int:
    print(f'bar: {b}')
    return 42
    
def baz(c: str) -> bytes:
    print(f'baz: {c}')
    return b''

@app.get("/things/{c}")  # `a`/`b`
async def read_thing(other: Annotated[str, Depends(baz), Depends(bar), Depends(foo)]):
    return other
```

```text
$ uv run fastapi run hello.py
      ...
      INFO   1.2.3.4:5 - "GET /things/ipsum HTTP/1.1" 422
```

```json
{
    "detail": [
        {
            "type": "missing",
            "loc": ["query", "a"],
            "msg": "Field required",
            "input": null
        }
    ]
}
```

It would be best if we could ask someone with FastAPI expertise to have a look.

---

_@MichaReiser reviewed on 2025-01-10 07:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:161 on 2025-01-10 07:38_

Thanks for the detailed investigation. 

@charliermarsh do you know someone we could ping on this? 

I'd otherwise suggest to bail (ignore the rule) if multiple `Depends` are seen

---

_@charliermarsh reviewed on 2025-01-11 13:35_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:161 on 2025-01-11 13:35_

I asked Sebastian

---

_@tiangolo reviewed on 2025-01-12 22:23_

---

_Review comment by @tiangolo on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py`:161 on 2025-01-12 22:23_

Thanks for the ping @charliermarsh ! (and thanks for doing it on DM, I wouldn't have seen the GitHub notification 😅)

Yeah, only one `Depends()` is supported in *path operation function* parameters. I actually didn't give much thought about what would happen if there's more than one when implementing it, I don't think I have tests for that. So I currently don't have any guarantee (tests) to ensure that it's always the first or last one (maybe I should? 🤔)... It's currently unspecified. 😅

Nevertheless, although there are no current guarantees, the implementation is probably gonna stay the same for quite a while.

---

_Comment by @charliermarsh on 2025-01-13 01:31_

I'll let @MichaReiser merge but I removed all the string allocations.

---

_Merged by @MichaReiser on 2025-01-13 08:51_

---

_Closed by @MichaReiser on 2025-01-13 08:51_

---

_Branch deleted on 2025-01-13 08:53_

---
