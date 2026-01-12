```yaml
number: 3292
title: "Implement configuration options `runtime-evaluated-decorators` and `runtime-evaluated-baseclasses` for `flake8-type-checking`"
type: pull_request
state: merged
author: sasanjac
labels:
  - configuration
assignees: []
merged: true
base: main
head: sasanjac/2195-Implement-configuration-options-from-`flake8-type-checking`
created_at: 2023-03-01T11:50:34Z
updated_at: 2023-03-12T17:56:24Z
url: https://github.com/astral-sh/ruff/pull/3292
synced_at: 2026-01-12T04:39:44Z
```

# Implement configuration options `runtime-evaluated-decorators` and `runtime-evaluated-baseclasses` for `flake8-type-checking`

---

_Pull request opened by @sasanjac on 2023-03-01 11:50_

see #2195

---

_Comment by @sasanjac on 2023-03-01 11:51_

This is my first time developing in Rust, so I hope I did everything correctly and not implement stuff inefficiently 🙈

---

_@charliermarsh reviewed on 2023-03-01 23:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:4372 on 2023-03-01 23:17_

Could we instead colocate this with the logic that ensures we treat these as runtime annotations when futures aren't enabled? I.e., here:

```rust
StmtKind::AnnAssign {
    target,
    annotation,
    value,
    ..
} => {
    // If we're in a class or module scope, then the annotation needs to be
    // available at runtime.
    // See: https://docs.python.org/3/reference/simple_stmts.html#annotated-assignment-statements
    if !self.annotations_future_enabled
        && matches!(
            self.current_scope().kind,
            ScopeKind::Class(..) | ScopeKind::Module
        )
    {
        ...
```

---

_@charliermarsh reviewed on 2023-03-01 23:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:4389 on 2023-03-01 23:19_

Instead of checking based on strings here, it'd be preferable to instead use `self.resolve_call_path`, which will give you back a `CallPath` object, and automatically handles aliased imports etc. The `should_ignore_definition` method in `crates/ruff/src/rules/pydocstyle/helpers.rs` is one example of what that might look like.

---

_Comment by @sasanjac on 2023-03-02 17:51_

Maybe I need to understand the default behavior first:
```python
import datetime

import pydantic

class Meta(pedantic.BaseModel):
    date: datetime.date
```

does not report any violations.


```python
from __future__ import annotations

import datetime

import pydantic

class Meta(pedantic.BaseModel):
    date: datetime.date
```

reports `TCH003`. To my understanding, the annotations are already treated as runtime annotations when futures aren't enabled, so we only have to treat runtime evaluated annotations as runtime annotations when futures *are* enabled. Am I missing something here?

---

_Comment by @charliermarsh on 2023-03-02 17:58_

Your understanding is correct! And it mirrors Python's own behavior. Type annotations in class definitions _are_ evaluated at runtime if you _don't_ use `__future__` annotations.

As an example, this code throws a runtime error (note that I've omitted the `datetime.date` import)

```py
import pydantic

class Meta(pydantic.BaseModel):
    date: datetime.date
```

However, this code does not:

```py
from __future__ import annotations

import pydantic

class Meta(pydantic.BaseModel):
    date: datetime.date
```

---

_Comment by @sasanjac on 2023-03-02 19:16_

Ok great, thank you!

---

_Comment by @sasanjac on 2023-03-02 19:41_

We might want to report this back to [flake8-type-checking](https://github.com/snok/flake8-type-checking) because I think it suggests moving type annotations in class definitions into a `TYPE_CHECKING` block even when `__future__` annotations are not used.

---

_Review requested from @charliermarsh by @sasanjac on 2023-03-03 10:32_

---

_Comment by @charliermarsh on 2023-03-05 23:25_

For reasons I can't quite figure out, I'm unable to push to the branch 😬 

```
ERROR: Permission to sasanjac-lab/ruff.git denied to charliermarsh.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

---

_Comment by @charliermarsh on 2023-03-05 23:30_

I pushed some changes to a branch [here](https://github.com/charliermarsh/ruff/tree/sasanjac/2195-Implement-configuration-options-from-%60flake8-type-checking%60), any way you could pull them in?

---

_Comment by @charliermarsh on 2023-03-05 23:35_

The changes are: removing the `pydantic.BaseModel` default (I'd like to avoid library-specific defaults but curious to hear your reaction), changing the `helpers` to take `Context` instead of `Checker` (a new pattern to decouple some stuff internally), and tweaking the docs a bit. I think that's it.

As a nit: should it be `runtime-evaluated-base-classes`? Since it'd typically be stylized as "base classes"?

Separately: should the decorators apply to _functions_ too? (`flake8-type-checking` seems to support FastAPI by omitting annotated functions, so wondering if we should support that too.)

And then as a higher-level question: how does this compare to the choices that `flake8-type-checking` made? We should probably document the differences in this PR -- the advantages and the disadvantages. Will this be useful in Pydantic contexts? Or too limited? Do we need to do something like `flake8-type-checking`'s option to enforce for _all_ classes?


---

_Comment by @andersk on 2023-03-05 23:36_

(The author of a PR from a fork has an option to [**Allow edits from maintainers**](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/allowing-changes-to-a-pull-request-branch-created-from-a-fork). @sasanjac must have unchecked it for this PR. Either @sasanjac you can turn that back on in the right sidebar, or @charliermarsh you could [open a PR to the PR](https://github.com/sasanjac-lab/ruff/compare/sasanjac/2195-Implement-configuration-options-from-%60flake8-type-checking%60...charliermarsh:ruff:sasanjac/2195-Implement-configuration-options-from-%60flake8-type-checking%60).)

---

_Comment by @charliermarsh on 2023-03-05 23:39_

That's what I thought, but I see this.

![Screen Shot 2023-03-05 at 6 38 23 PM](https://user-images.githubusercontent.com/1309177/222992598-38620f8f-79aa-403e-b357-8a12b76710e8.png)

I'll open a PR to the PR.


---

_Comment by @andersk on 2023-03-05 23:47_

Maybe you didn’t sufficiently escape the `` ` `` characters in this unusual branch name for your shell?

---

_Comment by @charliermarsh on 2023-03-06 02:36_

Oh something like that is definitely possible. (I just use `gh pr checkout 3292` then `git push`, but it is possible.)

---

_Comment by @sasanjac on 2023-03-06 07:58_

> For reasons I can't quite figure out, I'm unable to push to the branch 😬 
> 
> 
> 
> ```
> 
> ERROR: Permission to sasanjac-lab/ruff.git denied to charliermarsh.
> 
> fatal: Could not read from remote repository.
> 
> 
> 
> Please make sure you have the correct access rights
> 
> and the repository exists.
> 
> ```

Strange, I didn't know you had to explicitly allow this.

---

_Comment by @sasanjac on 2023-03-06 08:10_

> The changes are: removing the `pydantic.BaseModel` default (I'd like to avoid library-specific defaults but curious to hear your reaction), changing the `helpers` to take `Context` instead of `Checker` (a new pattern to decouple some stuff internally), and tweaking the docs a bit. I think that's it.
> 

Yeah, that sounds reasonable. 
> 
> As a nit: should it be `runtime-evaluated-base-classes`? Since it'd typically be stylized as "base classes"?
> 
True. 
> 
> Separately: should the decorators apply to _functions_ too? (`flake8-type-checking` seems to support FastAPI by omitting annotated functions, so wondering if we should support that too.)
> 
Interesting, I haven't used FastAPI yet but this feature sounds very reasonable and I could look into that. 
> 
> And then as a higher-level question: how does this compare to the choices that `flake8-type-checking` made? We should probably document the differences in this PR -- the advantages and the disadvantages. Will this be useful in Pydantic contexts? Or too limited? Do we need to do something like `flake8-type-checking`'s option to enforce for _all_ classes?
> 
> 
Yeah, maybe we should document it but I think the new approach is more precise than a blank ignore of imports when a class inherits from another. Also, @sondrelg is very active, even here and open to discussions. I thought we could maybe backport the same behavior to `flake8-type-checking`. 


---

_Comment by @sondrelg on 2023-03-06 08:45_

The decisions made in flake8-type-checking were originally made because I was using FastAPI and pydantic myself at the time, and I wanted to use the plugin in those projects. I didn't know about any other prominent libraries that evaluated type hints at runtime at the time (but there are several, including `attrs` with [cattrs](https://pypi.org/project/cattrs/)). I wouldn't have added library specific options again I don't think.

In retrospect I also think supporting FastAPI is a little bit of a waste of time. This is a subjective evaluation of course, but the concept of [dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/) makes it unreasonably hard to support properly. FastAPI can include arbitrary functions in a type signature like this (from the docs):

```python
@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons
```

And the type hints of the function `common_parameters` here are evaluated at runtime. That means you need to keep track of project-wide state and figure out how to follow nested imports in several files etc., as the dependency functions could live in a separate file, and may even be imported via another file, and so on.  All type hints in FastAPI endpoints are also evaluated. 

Basically I think if you're doing a FastAPI project, there are too many places where type hints are evaluated to consider a plugin like flake8-type-checking to be particularly useful. At least it would need to become a much larger, more complex project.

Is it really true that all class annotations are evaluated on Python 3.10+? I have several projects without `__future__` imports where I've never run into issues with this - but I think those may all use pydantic 😅 

---

_Comment by @sasanjac on 2023-03-06 10:54_

> ```python
> @app.get("/items/")
> async def read_items(commons: dict = Depends(common_parameters)):
>     return commons
> ```
> 
> And the type hints of the function `common_parameters` here are evaluated at runtime. That means you need to keep track of project-wide state and figure out how to follow nested imports in several files etc., as the dependency functions could live in a separate file, and may even be imported via another file, and so on. All type hints in FastAPI endpoints are also evaluated.

I'm haven't really used `FastAPI` but couldn't you just specify `app.get` as a `runtime-evaluated-decorator` and be done?

---

_Comment by @sasanjac on 2023-03-06 11:09_

> The decisions made in flake8-type-checking were originally made because I was using FastAPI and pydantic myself at the time, and I wanted to use the plugin in those projects. I didn't know about any other prominent libraries that evaluated type hints at runtime at the time (but there are several, including `attrs` with [cattrs](https://pypi.org/project/cattrs/)). I wouldn't have added library specific options again I don't think.

Yeah, here it is up to you to decide if you want to deprecate config options, but I think the way of specifying base classes and decorators that lead to the corresponding type annotations not reported is a) a more generic way, no need to have library specific options and b) more precise because only the affected classes/functions are exempt from the rule. However, the user has to actively put the respective classes/decorators in the config.

---

_Comment by @sondrelg on 2023-03-06 13:47_

> > ```python
> > @app.get("/items/")
> > async def read_items(commons: dict = Depends(common_parameters)):
> >     return commons
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > And the type hints of the function `common_parameters` here are evaluated at runtime. That means you need to keep track of project-wide state and figure out how to follow nested imports in several files etc., as the dependency functions could live in a separate file, and may even be imported via another file, and so on. All type hints in FastAPI endpoints are also evaluated.
> 
> I'm haven't really used `FastAPI` but couldn't you just specify `app.get` as a `runtime-evaluated-decorator` and be done?

You could, if you were only using that decorator pattern. Unfortunately it's very common to use `APIRouter`s to create aggregation of subpaths, like this:

```python
# users.py

user_router = APIRouter(base='/users')

@user_router.get()
def list_users(): ...

@user_router.get('/{id}')
def retrieve_user(id: UUID): ...

# and so on
```

So one FastAPI project could have `n` decorator patterns in addition to `m` functions that act as "dependencies" where their types will be evaluated, but there is nothing that "marks" them as dependency in the file they are declared in.

---

_Comment by @sondrelg on 2023-03-06 13:50_

> > The decisions made in flake8-type-checking were originally made because I was using FastAPI and pydantic myself at the time, and I wanted to use the plugin in those projects. I didn't know about any other prominent libraries that evaluated type hints at runtime at the time (but there are several, including `attrs` with [cattrs](https://pypi.org/project/cattrs/)). I wouldn't have added library specific options again I don't think.
> 
> Yeah, here it is up to you to decide if you want to deprecate config options, but I think the way of specifying base classes and decorators that lead to the corresponding type annotations not reported is a) a more generic way, no need to have library specific options and b) more precise because only the affected classes/functions are exempt from the rule. However, the user has to actively put the respective classes/decorators in the config.

Decorators for classes might make it possible to whitelist attrs classes from type checking in a reliable way, but if the intention for this feature was to add FastAPI support by whitelisting functions decorated with certain patterns you should probably also have another feature for adding dependencies. I don't think this is a great idea though, because:

- It's hard to do correctly
- you end up checking very few elements, so the benefit of the plugin is low
- the consequence of messing up is high (potentially runtime errors in prod, if your test coverage isn't good enough)

This is why I personally think you should focus on adding the features you need to support Pydantic usage for now, and think about remaining patterns later if it comes up 👍 

---

_Comment by @charliermarsh on 2023-03-07 04:34_

Ok cool -- thank you for all the discussion here too, very clarifying for me! I'm happy with what's happening here, let's rock. Thanks for diving into Rust :)

---

_Merged by @charliermarsh on 2023-03-07 04:34_

---

_Closed by @charliermarsh on 2023-03-07 04:34_

---

_Label `configuration` added by @charliermarsh on 2023-03-07 04:34_

---

_Branch deleted on 2023-03-07 07:53_

---

_Comment by @Cielquan on 2023-03-12 17:39_

Does this implementation also work on subclasses?

E.g. I have an application with different configs and one base config
```py
import pydantic
import utils

class BaseConfig(pydantic.BaseModel):
    debug = False
    database_url: str

class ProdConfig(BaseConfig):
    database_url ="production"
    prod_only: utils.Type

class DevConfig(BaseConfig):
    debug = True
    database_url = "dev"
```

---

_Comment by @charliermarsh on 2023-03-12 17:42_

No, it wouldn't work for subclasses in the same file like that. It could work if you exported `BaseConfig` from another file, then included `my_file.BaseConfig` in the list of runtime-evaluated base classes.

---

_Comment by @Cielquan on 2023-03-12 17:54_

Thats a bummer but good to know. Thanks.

---

_Comment by @charliermarsh on 2023-03-12 17:56_

Yeah we can't do that level of type inference yet. We track _imports_, but we can't track arbitrary base class resolution yet.

---
