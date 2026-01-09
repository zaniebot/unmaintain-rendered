---
number: 1069
title: Surprising config file resolution behavior
type: issue
state: closed
author: smackesey
labels:
  - configuration
assignees: []
created_at: 2022-12-05T18:52:57Z
updated_at: 2023-02-03T18:28:30Z
url: https://github.com/astral-sh/ruff/issues/1069
synced_at: 2026-01-07T13:12:14-06:00
---

# Surprising config file resolution behavior

---

_Issue opened by @smackesey on 2022-12-05 18:52_

Our repo has two pyproject.toml files with `[tool.ruff]` sections:

- `pyproject.toml`  ("root")
- `examples/docs_snippets/pyproject.toml`  ("docs_snippets")

The logic that resolves the config file for any given invocation of `ruff` surprised me:

```
$ ruff `git ls-files '*.py'`  # uses root config for files in `examples/docs_snippets`
$ ruff `git ls-files 'examples/docs_snippets/*.py`  # uses docs_snippets config
```

So, for a given file in `examples/docs_snippets`, different runs of `ruff` from the same cwd and with the same config files on disk will use different `pyproject.toml` files. It suprised me to have the config used for a particular file determined by whatever unrelated files also happen to be being processed instead of just (a) the cwd; and (b) location of existing config files.

I don't have a strong opinion on _how_ a config file is resolved, but IMO it should always resolve to the same one if the launch CWD and config files on disk are constant.



---

_Comment by @charliermarsh on 2022-12-05 19:27_

The current behavior is: take the common ancestor of all provided files, then find the most specific `pyproject.toml` file in that ancestor path. (So, in the first invocation, it uses the root; in the second, `examples/docs_snippets` is the common ancestor, so it ends up using `examples/docs_snippets/pyproject.toml`.)

Need to think on the right change here. 


---

_Label `configuration` added by @charliermarsh on 2022-12-05 19:27_

---

_Comment by @smackesey on 2022-12-05 19:57_

>Need to think on the right change here.

IMO the best approach is to always use the most specific config for every file processed (allowing multiple configs to be used in a given run for different files). Together with config inheritance (#1055), this is most convenient from a user perspective, allowing a single run from the root to process every file with appropriate config.

---

_Comment by @charliermarsh on 2022-12-05 20:01_

Yeah I think that's right.

---

_Comment by @charliermarsh on 2022-12-05 20:02_

(Just not a trivial change so will take a bit of time.)

---

_Comment by @smackesey on 2022-12-06 02:54_

Another quirk I've discovered is that the `pyproject.toml` is chosen using the logic you've described above even if it doesn't have a `[tool.ruff]` section. IMO without that section it should definitely look further up the tree.

---

_Comment by @charliermarsh on 2022-12-06 04:02_

I'll try to prioritize this and get it done this week.

---

_Comment by @smackesey on 2022-12-06 04:24_

If you can get config inheritance working that would be huge for us. Would go a long way towards preventing confusing and frustrating scenarios where implicit differences in config cause editor-driven and CLI or CI-driven invocations of `ruff` to disagree on the correct state of the code.

I realize of course it's a tricky problem-- one that none of the other python linters have solved (to my knowledge). It leaves monorepos in a bad spot.

---

_Comment by @charliermarsh on 2022-12-06 04:41_

Yeah I can totally empathize. Monorepos are amazing but tend to be so under-supported by tooling. Will try to make this happen soon!

---

_Comment by @charliermarsh on 2022-12-06 04:55_

(I'm probably over-stating how much work this is, the code is actually pretty well set-up for it.)

---

_Comment by @smackesey on 2022-12-06 12:28_

Noticed after updating to `0.0.165` that:

```
$ ruff --config=pyproject.toml --select=I001 `git ls-files 'examples/docs_snippets/*.py'`
```

No longer treats the `docs_snippets` package as first party when sorting imports. I assume this is a consequence of the changed logic around finding the "project root", but this seems like a bug right? Regardless of where the project root is, files inside `docs_snippets` should be identified as part of the `docs_snippets` package and imports from `docs_snippets` treated as first-party.

---

_Comment by @charliermarsh on 2022-12-06 14:36_

I think you want to set this:

```toml
[tool.ruff]
src = ["python_modules/dagster", "examples/docs_snippets"]
```

Otherwise, it's as if you're trying to import `doc_snippets` from the root directory.


---

_Comment by @smackesey on 2022-12-06 15:00_

Oh, hmm, I hadn't seen the `src` option. Our monorepo actually has ~50 packages-- for instance, there is also `python_modules/dagit` and `python_modules/dagster_graphql`. But these didn't suffer from the same "not first-party" problem as `examples/docs_snippets`, presumably because I never ran e.g.:

```
$ ruff --config=pyproject.toml --select=I001 `git ls-files 'python_modules/dagster-graphl/*.py'`
```

Instead the formatting is handled by:

```
$ ruff --config=pyproject.toml --select=I001 `git ls-files '*.py'`
```

For these other packages, and first party is correctly detected. I guess that seems unintuitive to me? `docs_snippets` gets treated as non-first-party _only_ when I directly target it with `git ls-files 'examples/docs_snippets/*.py'`.

---

_Comment by @charliermarsh on 2022-12-06 16:25_

Hmm, I don't think that's quite what's happening. If you're using `--config=pyproject.toml`, then it shouldn't matter whether you "directly target" a given directory -- all resolution will happen from the root directory.

In fact, it does look like `dagster-graphql` is being classified as third-party (I confirmed this with some extra logging). Notice this diff after changing the TOML file to:

```toml
src = ["python_modules/dagster", "python_modules/dagster-graphql", "examples/docs_snippets"]
```

Which leads to this change:

```diff
--- a/python_modules/dagster-graphql/dagster_graphql/implementation/fetch_runs.py
+++ b/python_modules/dagster-graphql/dagster_graphql/implementation/fetch_runs.py
@@ -1,7 +1,6 @@
 from collections import defaultdict
 from typing import TYPE_CHECKING, Dict, KeysView, List, Mapping, Sequence, cast

-from dagster_graphql.implementation.fetch_assets import get_asset_nodes_by_asset_key
 from graphene import ResolveInfo

 from dagster import (
@@ -16,6 +15,7 @@
 from dagster._core.storage.pipeline_run import RunRecord, RunsFilter
 from dagster._core.storage.tags import TagType, get_tag_type
 from dagster._legacy import PipelineDefinition, PipelineRunStatus
+from dagster_graphql.implementation.fetch_assets import get_asset_nodes_by_asset_key
```

So there are a few ways to solve this. Immediately, you could add your modules as `known-first-party`, or add them to `src`. Later on, you could add `pyproject.toml` files to all of your packages, and we could use those to automatically detect module roots (when we support cascading configuration files).


---

_Comment by @charliermarsh on 2022-12-06 21:17_

I'm gonna try to write up a proposal for how this system change before I implement it, to ensure that it works well for individual projects and monorepos.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-07 19:15_

---

_Comment by @charliermarsh on 2022-12-07 19:35_

My current plan is as follows:

- From now on, only consider `pyproject.toml` files with a `tool.ruff` section.
- Always use the `pyproject.toml` containing a `tool.ruff` section "closest" to a given file.
- Always resolve paths (like per-file-ignores, exclusions, etc.) relative to the `pyproject.toml` file.
- Add an `extends` key to `pyproject.toml`, which takes a relative path to another `pyproject.toml` and performs a deep merge with the inherited `pyproject.toml`.
- Modify the semantics of `extend-ignore` and `extend-select` to _always_ extend and never override. (This would allow nested configs to add and remove errors from parents without risk of overriding a _parent's_ `extend-ignore` or `extend-select`.)

---

A couple other relevant details:

- If we can't find any `pyproject.toml` file, we'll just use the current working directory and default settings. (So we'll probably remove the `.git` detection?)
- We _could_ also support a `ruff.toml` file similar to Hatch's `hatch.toml`.
- We'll need to remove some keys from the `Settings` struct (like `--format`) which don't make sense in an inherited model and only as top-level options.

---

One thing I'm unsure of is how the `src` path resolution should work here (which, today, is only used for inferring first-party imports, but in the future will be applicable beyond that).

Thinking aloud for a sec, let's say I have the following setup:

- `./python_modules/dagster/dagster`
- `./python_modules/dagit/dagit`
- `./examples/docs_snippets/docs_snippets`

Say, in my root `./pyproject.toml`, I have:

```toml
[tool.ruff]
src = ["python_modules/dagster", "python_modules/dagit"]
```

And in `./examples/doc_snippets/pyproject.toml` (which I want to customize to exclude `E501`, or something), I have:

```toml
[tool.ruff]
extends = "../../pyproject.toml"
extend-ignore = ["E501"]
```

The nice thing about this model is that running `ruff examples/doc_snippets` from root and `ruff .` from `./examples/doc_snippets` will have the same behavior, which is a great property and one that @smackesey has brought up a few times.

The confusing thing is that because we set `src = ["python_modules/dagster", "python_modules/dagit"]` in the root `./pyproject.toml` file, and we're inheriting it in `./examples/doc_snippets/pyproject.toml`, we won't treat any of the `doc_snippets` imports as first-party.

To achieve that, we'd need to add `src = ["."]` to `./examples/doc_snippets/pyproject.toml`, or include `"examples/doc_snippets"` in the root `./pyproject.toml` file.

A few ways to solve that problem:

1. Don't solve it, and assume that you should enumerate all modules in the root `pyproject.toml`.
2. Add `extends-src = ["."]`.
3. Do something else? Infer sources differently?


---

_Comment by @charliermarsh on 2022-12-07 19:36_

@smackesey - No urgency at all but I'd like to ensure that the scoped work here leads to an improvement in Dagster's setup and workflow, so feedback is very welcome.


---

_Comment by @smackesey on 2022-12-07 20:19_

Hey Charlie thanks for writing this up. In general I really like this plan as it incorporates basically everything I requested! Some specific questions/comments:

>Modify the semantics of extend-ignore and extend-select to always extend and never override. (This would allow nested configs to add and remove errors from parents without risk of overriding a parent's extend-ignore or extend-select.)

(1) Will `extend-exclude` get the same treatment?

(2) Would it be possible in this scheme to override (without the old mechanism of direct setting overwriting) an inherited `select`/`ignore` by setting the opposite parameter? So if the parent config does `--extend-select=F401`, will the current config's `--extend-ignore=F401` beat out the parent?

>We could also support a ruff.toml file similar to Hatch's hatch.toml.

I think this is important since other tools may deal with `pyproject.toml` the way ruff currently deals with it-- i.e. thinking the closest one is the source of truth regardless of the existence of a tool-specific section. Like if you have a root `pyproject.toml` (R) and a subproject `pyproject.toml` (S), another tool might discover S inside the subproject, and even though there are no settings for the tool, not look any further up to find R (where it is configured). If we allow `ruff.toml` then users always have an option to configure ruff without screwing with the config of other tools.

>One thing I'm unsure of is how the src path resolution should work

Before commenting, I need to clarify my own understanding of `src`-- is the idea that the union of all files rooted under an `src` entry constitutes one big body of "first-party" code? For example, below there is an example `pyproject.toml` and some imports in a hypothetical `dagster-graphql` file. I marked the imports with whether they are considered "first-party" given the settings. Are my marking correct?:

```
### pyproject.toml (root)

[tool.ruff]
src = [
    "python_modules/dagster/dagster",
    "python_modules/dagit",
]

### python_modules/dagster-graphql/dagster_graphql/foo/bar.py

from dagster import op   # first party
from dagster_graphql import implementation  # third party
```

---

_Comment by @charliermarsh on 2022-12-07 20:32_

> (1) Will extend-exclude get the same treatment?

Yeah. (Paths will be relative to the `pyproject.toml` in which they're defined.)

> (2) So if the parent config does --extend-select=F401, will the current config's --extend-ignore=F401 beat out the parent?

Hah that's tricky. If you had this setup, you _would_ end up ignoring `F401`, because ignores "beat" selects given equal codes. But, I guess if the parent had `ignore = ["F401"]`, then you wouldn't have _any_ way to enable `F401` in a child. I've gotten think on this. Maybe the semantics should be: resolve the parent codes, then apply the child rules (as opposed to: combine the parent and child rules, then resolve the complete set).

> If we allow ruff.toml then users always have an option to configure ruff without screwing with the config of other tools.

Yeah this makes sense (and it's not much effort to support).

> Before commenting, I need to clarify my own understanding of `src`...

It's probably easiest to explain it with a detailed example, and then maybe you can help me simplify that explanation.

When we go to resolve an import, there are some trivial cases: if an import has a dot level (`from . import foo`), we know it's a "relative import"; if an import is a known standard library module, we know it's "standard library".

If it doesn't fit one of those cases, we take the first part of the import -- so for `import foo`, we take `foo`; for `from foo.impl import *` we take `foo` again; for `import foo.impl`, we take `foo` again.

We then iterate over the `src` entries and try to find that module. So for `import foo`, and given the above `pyproject.toml`, we'd check:

- `python_modules/dagster/dagster/foo`
- `python_modules/dagster/dagster/foo.py` 
- `python_modules/dagit/foo`
- `python_modules/dagit/foo.py` 

If we can resolve one of these, we mark it as "first-party". Otherwise, it's "third-party".

Your example above is correct, except that `python_modules/dagster/dagster` should be `python_modules/dagster`, so that when we try to `from dagster import op`, we look for `python_modules/dagster/dagster`. Here's the complete example:

```
### pyproject.toml (root)

[tool.ruff]
src = [
    "python_modules/dagster",  # Removed the last `/dagster`.
    "python_modules/dagit",
]

### python_modules/dagster-graphql/dagster_graphql/foo/bar.py

from dagster import op   # first party
from dagster_graphql import implementation  # third party
```


---

_Comment by @smackesey on 2022-12-07 20:42_

>Hah that's tricky. If you had this setup, you would end up ignoring F401, because ignores "beat" selects given equal codes. But, I guess if the parent had ignore = ["F401"], then you wouldn't have any way to enable F401 in a child. I've gotten think on this. Maybe the semantics should be: resolve the parent codes, then apply the child rules (as opposed to: combine the parent and child rules, then resolve the complete set).

I think this is important because you should always be able to enable a check from the CLI regardless of what is in your config files.

>It's probably easiest to explain it with a detailed example, and then maybe you can help me simplify that explanation.

OK thanks for clarifying. So, I'm not sure of how `isort` actually does it, but shouldn't imports be considered "first-party" if they are importing from the same package as the current file? Like:

```
### python_modules/dagster-graphql/dagster_graphql/foo/bar.py

# This should be considered first-party no matter what is set in `src`
from dagster_graphql import implementation
```

I would think the "first party" resolution logic from `src` and `known-first-party` should apply _on top_ of baseline logic, which is: "If we are importing from a module that lives in the same root package as the current module, it's first party."


---

_Comment by @charliermarsh on 2022-12-07 20:49_

Let me do a bit of research into how `isort` handles this. (The current logic is based on [reorder-python-imports](https://github.com/asottile/reorder_python_imports) which I liked for its simplicity.) It's not totally clear to me how you would reliably detect the root package for a given file. E.g., above, how would you know that `from dagster_graphql import implementation` is first-party, but `from foo import other_thing` isn't?


---

_Comment by @charliermarsh on 2022-12-07 20:50_

I guess you could traverse upwards and look for `__init__.py`, but I think there are cases like namespace packages where `__init__.py` is omitted. (I can't really remember, I always try and get in and out of that kind of stuff as quickly as possible.)

---

_Comment by @smackesey on 2022-12-07 20:55_

>  E.g., above, how would you know that from dagster_graphql import implementation is first-party, but from foo import other_thing isn't?

Pretty sure every Python module has a unique `__name__`:

```
$ ipython

In [1]: import dagster._core

In [2]: dagster._core.__name__
Out[2]: 'dagster._core'
```

Once you have the name of the current module, you can easily tell if the module being imported from should be first-party or not (prior to applying the extra logic of `src`/`known-first-party`).

So you would need to model the logic used to generate a module's `__name__`-- not sure off the top of my head how it's done for namespace packages, but I've learned it before so I'm going to look into it.

---

_Comment by @charliermarsh on 2022-12-07 21:01_

I do think that the current behavior is similar to `isort`'s? I recreated the example above, and running `isort` on this directory treats both of those imports as third-party:

![Screen Shot 2022-12-07 at 4 00 38 PM](https://user-images.githubusercontent.com/1309177/206294730-2e870fec-f9ee-414e-adaf-6d222a87f767.png)



---

_Comment by @smackesey on 2022-12-07 22:16_

>I do think that the current behavior is similar to isort's?

Interesting, yeah you're right-- in that case, IMO this represents an an opportunity to improve on isort's behavior, which is not suited to monorepos. It is confusing to not see intra-package imports classified as first party by static tooling, because logically they _are_ first-party.

Let me just describe what I think is the ideal functionality, with some concrete examples from Dagster:

1. Within a single run of `ruff`, `ruff` is able to vary the set of first-party packages depending on what file is being processed.
2. The current package is always classified as a first party package.
3. This is achievable with minimal configuration.

Regarding (1), this matters because a monorepo can easily contain, e.g. example project code (ours does) which should treat the main packages as third-party.

(2) is tricky to nail 100% because a module's `__name__` is determined during the import process-- I don't think there's a 100% reliable way to determine it statically. However, tracing `__init__.py` up the file hierarchy is going to work in 99% of cases. My preferred solution is that this is the first step in a "first party" check, then it falls back to `src` and `known-first-party`.

However, if you decide to go with a wholly explicit approach (all first party must be listed in `src`), I think the status quo could be improved by allowing glob patterns here. Like I've mentioned before, Dagster has like ~50 packages. New examples etc are added fairly regularly. It is a maintenance burden to keep an accurate list of these in config. But e.g. this would be much easier:

```
src = [
    "python_modules/*",
    "python_modules/libraries/*",
    "examples/*",
    "helm/dagster/schema",
    "integration_tests/test_suites/*",
    "integration_tests/python_modules/*"
]
```

This would capture all of Dagster's packages. A caveat is that this overshoots a little bit-- like `python_modules/libraries` would be captured by `python_modules/*`, but it shouldn't be on `src`-- so some kind of exclusion (maybe exclusion patterns like `git` uses) or additional check logic would be required.

---

_Comment by @smackesey on 2022-12-07 22:39_

Another possibility, and I think you ultimately might need something like this anyway if you want to replicate e.g. `pylint` or `pyright`' s ability to catch unresolvable imports, is to make use of `sys.path` and basically simulate the logic of Python's import system. This would probably give the best user experience. You would want to provide a `python-interpreter` or `python-path` setting like that provided by pylance/pyright that you could set to a virtualenv.

---

_Comment by @charliermarsh on 2022-12-07 22:42_

Ok cool. This all makes sense. I'm leaning towards the `__init__.py` tracing... but that opinion may change as I spend more time with the problem.

---

_Referenced in [dagster-io/dagster#10901](../../dagster-io/dagster/pulls/10901.md) on 2022-12-07 22:48_

---

_Comment by @charliermarsh on 2022-12-07 23:12_

I'll provide an update here when I have an estimate of when this should all ship. I'm gonna ship pieces of it incrementally.

---

_Comment by @smackesey on 2022-12-08 00:03_

Another config-related item that would be useful-- environment variable interpolation, specifically for the planned `extends` field. Our use case is that we have a second `internal` repo where we've had to maintain a copy in `pyproject.toml` of the config from the OSS repo. But ideally we could just do this:

```
[tool.ruff]
extends = "${OSS_REPO}/pyproject.toml"
# ... a few internal-specific settings
```

This becomes more useful the more repos you have.

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-12-08 18:38_

---

_Comment by @charliermarsh on 2022-12-10 15:50_

Alright, let's see how much of this I can get done today.

---

_Referenced in [astral-sh/ruff#1190](../../astral-sh/ruff/pulls/1190.md) on 2022-12-11 03:41_

---

_Comment by @charliermarsh on 2022-12-11 05:02_

Hierarchical settings (use the closest `pyproject.toml` for every file) is pretty much good to go in #1190.

(I have to make some decisions around how Ruff behaves when it can't find a `pyproject.toml`, and how it treats a few of the special-case, top-level "options" like `fix = true`, which doesn't easily cascade.)


---

_Closed by @charliermarsh on 2022-12-12 15:12_

---

_Comment by @charliermarsh on 2022-12-12 22:19_

@smackesey - I think I'm likely to support glob patterns in `src` as described above (https://github.com/charliermarsh/ruff/issues/1222).

---

_Comment by @charliermarsh on 2022-12-13 03:11_

This is going out in [v0.0.178](https://github.com/charliermarsh/ruff/releases/tag/v0.0.178) -- both the hierarchical resolution, and the globbing (https://github.com/charliermarsh/ruff/issues/1222).

---

_Comment by @smackesey on 2022-12-15 18:53_

This is coming along great and we'll merge to Dagster pretty soon. Just wondering if the `__init__.py` auto-detect first party stuff went in with the rest?

---

_Comment by @charliermarsh on 2022-12-15 18:55_

@smackesey -- Na that hasn't gone out yet. But I did ship the `python_modules/*` globbing which hopefully helps a bit?

---

_Comment by @smackesey on 2022-12-15 19:01_

Correct me if I'm wrong but the globbing just helps capture _all_ of our packages under `src`, which is then used to determine first-party right? So if I did this in root `pyproject.toml`:

```
[tool.ruff]

src =[
  "examples/*",
  "python_modules/*"
]
```

That would mean that all of our examples and python modules would form one giant first-party blob right? So that would mean that, no matter which package I'm in, everything caught by the globs is first party. So if I were in a file `examples/my-example/my_example/foo.py`, and I imported `dagster-aws`, it would be treated as first party.

What I'm looking to achieve is to treat _only_ the current package (and possibly core `dagster`) as first party. This is achievable with the new hierarchical config system by giving every package a `pyproject.toml` where it lists itself as first-party, but with `__init__.py` detection we wouldn't even need the `pyproject.toml`.

---

_Comment by @charliermarsh on 2022-12-15 19:03_

Yeah I think what you're describing is correct (I thought you wanted all modules to use the same set of third- and first-party rules).


---

_Comment by @charliermarsh on 2022-12-15 19:20_

So the idea was to use `__init__.py` to detect the current package for every file, and then automatically mark imports from that package as first-party, right?


---

_Comment by @smackesey on 2022-12-15 19:24_

>So the idea was to use __init__.py to detect the current package for every file, and then automatically mark imports from that package as first-party, right?

exactly



---

_Comment by @charliermarsh on 2022-12-15 19:30_

Makes sense. I need to figure out if this is behavior that always applies, or is just the new default behavior for the `src` field. Or if it’s a separate setting, or something.

---

_Comment by @smackesey on 2022-12-15 20:37_

IMO it should be on by default, but togglable with a setting. Although I can't think of why one would turn it off (doesn't mean there isn't a case!)-- it's basically the definition of "first party" to be in the current package-- and if you want to make other stuff first party too, you can always specify that explicitly.

---

_Comment by @charliermarsh on 2022-12-15 21:14_

Sounds good. This won’t go out today, but it shouldn’t be too hard to implement.

---
