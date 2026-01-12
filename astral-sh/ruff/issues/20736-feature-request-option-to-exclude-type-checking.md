```yaml
number: 20736
title: "[feature request] option to exclude type checking imports in `ruff analyze`"
type: issue
state: closed
author: gwdekker
labels:
  - wish
  - analyze
assignees: []
created_at: 2025-10-07T06:40:34Z
updated_at: 2025-11-16T12:30:25Z
url: https://github.com/astral-sh/ruff/issues/20736
synced_at: 2026-01-12T15:54:57Z
```

# [feature request] option to exclude type checking imports in `ruff analyze`

---

_@gwdekker_

tach by default ignores dependencies if the import statement is wrapped by if TYPE_CHECKING:. Is that something that is possible with ruff analyze graph as well, and would you consider adding it if not yet possible? (from another thread on the issue tracker I found [Grimp](https://grimp.readthedocs.io/en/stable/usage.html), which supports this with graph = grimp.build_graph('mypackage', exclude_type_checking_imports=True).

came up in https://github.com/astral-sh/ruff/issues/20701

@seddonym 

---

_Label `wish` added by @MichaReiser on 2025-10-07 10:23_

---

_Label `analyze` added by @MichaReiser on 2025-10-07 10:23_

---

_Comment by @MichaReiser on 2025-10-07 10:24_

An option like this does make sense to me (which we might want to default to false). 

An alternative is that ruff always returns all imports but sets a typing_only property for imports that are in TYPE_CHECKING blocks.

---

_Comment by @jhirshman on 2025-10-10 02:57_

+1 here.  we recently changed from tach to using just ruff and are seeing the same thing

---

_Comment by @seddonym on 2025-10-10 09:23_

If you're looking for a Tach replacement right now, Import Linter supports this via the exclude-type-checking-imports setting. https://import-linter.readthedocs.io/en/stable/usage.html#top-level-configuration

---

_Comment by @gauthsvenkat on 2025-11-03 09:18_

Happy to pick this up, though I need some advice on what would be the best way to do this.

- If we do it right, we need to build a semantic model in `ruff analyze` and create a dependency on `crates/ruff_python_semantic` which has a ` is_type_checking_block` [helper function](https://github.com/astral-sh/ruffruff/blob/main/crates/ruff_python_semantic/src/analyze/typing.rs#L419-L419).  
We should be able to plug it in `collector.rs`'s `visit_stmt` [method](https://github.com/astral-sh/ruff/blob/main/crates/ruff_graph/src/collector.rs#L97) to track type checking blocks.

- The `CollectedImport` [enum](https://github.com/astral-sh/ruff/blob/main/crates/ruff_graph/src/collector.rs#L156) can be made in to a struct and have a `typing_only` boolean field for this purpose.

@MichaReiser do we go this route? Let me know if I'm overlooking something.

---

_Comment by @MichaReiser on 2025-11-03 15:08_

I don't think it's enough to just depend on `ruff_python_semantic`. We'd have to do the AST walking to build the semantic model, and I believe, this is somewhat tight to our linting. The good news is that we don't have to be that strict about `TYPE_CHECKING`. In fact, ty, our upcoming type checker, treats any `if TYPE_CHECKING:` as a type checking block, regardless from where `TYPE_CHECKING` is imported from or if it's a local variable. 

E.g. see here https://github.com/astral-sh/ruff/blob/cedf36c415e54054c172699738bed559506870b0/crates/ty_python_semantic/src/types/infer/builder.rs#L4797-L4801

What's unclear to me is how we want to design this feature. Do we want to change the output format (which would be a breaking change) or make this a configuration option? What's the better solution very much depends on how you all intend to use the feature. Is it that you always want to ignore type-checking imports or do you want to ignore type checking imports only for some analysis runs?

Hearing more about your use cases would help us make a decision here

---

_Comment by @gwdekker on 2025-11-04 06:48_

We use `ruff analyze graph` as a way to do checks on the repository structure of our monorepo, which is structured as a [polylith monorepo](https://polylith.gitbook.io/polylith). For that use case, flags (either as config on the relevant pyproject.toml section or as flags on the command itself) seem sensible. For now we always want to ignore type-checking imports. However, even if we in the future want to be able to switch, it would for our use case acceptable to do two runs, one with and one without the flag. 

I actually really like the choice made for the configuration of [import-linter](https://import-linter.readthedocs.io/en/stable/usage.html#top-level-configuration). They have namespace_packages (what you already have), a flag on external dependencies (what you have through either specifying or omitting a path to a python executable - maybe your design could be improved by following that example), and a flag on what to do with typing imports. To me that covers all main flavours you would want from a tool like this.

As a later, separate next step, `ruff analyze graph` could consider adding a `--detailed` graph which could cover some of the other open wishes:
- #15195 has a good list of ideas, including a way of marking if an import was for typing purposes or not 
- #20737 adding line numbers could maybe also be part of that.
(as an aside, you could even consider adding 3 levels of detail: one where the outputs are grouped on package level (as mentioned here: https://github.com/astral-sh/ruff/issues/13431), one as is right now, and one more detailed one). Although as long as you have the more detailed one, constructing the others is not that complicated either).

So my suggestion would be:
- first implement the configuration options
- later design the alternative, potentially breaking, output format.

---

_Comment by @gwdekker on 2025-11-04 06:51_

> In fact, ty, our upcoming type checker, treats any if TYPE_CHECKING: as a type checking block, regardless from where TYPE_CHECKING is imported from or if it's a local variable.

A bit off-topic but wanted to ask, feel free to ignore: so would `ty` correctly find a type checking block if I would do the following:

```python
from typing import TYPE_CHECKING as TC

if TC:
   ...
``` 

---

_Comment by @MichaReiser on 2025-11-04 18:13_

I was wrong. ty supports both (any variable named `TYPE_CHECKING` and aliased). Do you need the aliasing? Having a first version that only supports `TYPE_CHECKING` is much easier and I suspect it should get us quiet far?

---

_Comment by @gauthsvenkat on 2025-11-05 08:15_

> Do you need the aliasing?

(Answering on behalf of @gwdekker): We do not use aliasing in our monorepo.

> Having a first version that only supports TYPE_CHECKING is much easier and I suspect it should get us quiet far?

Yes, that is my gut feeling as well. It would certainly work for our use-case.

---

_Closed by @MichaReiser on 2025-11-16 12:30_

---
