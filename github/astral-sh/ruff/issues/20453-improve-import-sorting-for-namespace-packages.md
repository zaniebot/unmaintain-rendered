---
number: 20453
title: Improve import sorting for namespace packages
type: issue
state: closed
author: lkollar
labels:
  - question
  - isort
assignees: []
created_at: 2025-09-17T15:49:43Z
updated_at: 2025-09-18T17:05:29Z
url: https://github.com/astral-sh/ruff/issues/20453
synced_at: 2026-01-07T13:12:16-06:00
---

# Improve import sorting for namespace packages

---

_Issue opened by @lkollar on 2025-09-17 15:49_

### Summary

With Ruff 0.13 import sorting has changed in a way that 1st party namespace packages are now categorised as 3rd party. 

Let's assume that a Foo Corp puts all their packages into the `foo.` namespace. Up until 0.13, the source repo for `foo.bar` package would have these imports:

```python
import httpx

import foo.baz  # <- another library published by Foo Corp
import foo.bar  # <- current project
```

I think this was likely sorted this way because the presence of a `foo` namespace package in the repository made Ruff consider all packages in that namespace as they belong to the project. This wasn't correct, but it happened to yield the right result.

With Ruff 0.13, import sorting changes this to:

```python
import httpx
import foo.baz  # <- now a 3rd party dependency

import foo.bar
```

So while the detection logic is more correct, the end result is not ideal.

I wonder if Ruff's import sorting algorithm could be improved so that if it detects a local `foo` namespace package, it categorises all packages in that namespace as first party.

---

_Comment by @MichaReiser on 2025-09-17 15:52_

CC: @dylwil3 

---

_Label `question` added by @dylwil3 on 2025-09-17 21:14_

---

_Label `isort` added by @dylwil3 on 2025-09-17 21:14_

---

_Comment by @dylwil3 on 2025-09-17 21:14_

Probably what is confusing is that there isn't (as far as I know) an _official_ definition of "first" and "third" party. Currently, we are trying to model that "first" party means "the current package and only the current package" and third party is anything else that isn't a builtin (roughly) - even things in the same top-level namespace. In fact, the impetus for the behavior stabilized in 0.13 was to achieve exactly the _opposite_ of what you're suggesting here (unless I'm misunderstanding) - see #12984. 

I don't think it's possible to support both versions of what folks might consider "first party" by default. However, in your case, I wonder if it would solve the problem to specify this in your settings:

```toml
[tool.ruff.lint.isort]
known-first-party = ["foo"]
```

---

_Comment by @godlygeek on 2025-09-17 23:06_

(I'm a coworker of @lkollar - so, same organization! 👋)

> I don't think it's possible to support both versions of what folks might consider "first party" by default.

I can see that people might have different opinions of what constitutes "first party", and that many of those definitions aren't amenable to programmatic detection (I'd think the most common definition is "packages made by my organization", but there's no surefire way to know what packages are made by what organization, and using the implicit namespace packages is at best a heuristic).

But, that said...

> Currently, we are trying to model that "first" party means "the current package and only the current package"

Isn't "the current package and only the current package" exactly what people would expect `local-folder` to be for?

It seems as though Ruff has currently standardized on:

- `first-party` - absolute imports of modules in the current folder
- `local-folder` - relative imports of modules in the current folder

Different folks having different definitions for "first party" seems like a red herring to me. Surely whether a module is "first party" or not is a property of the module itself and doesn't change based on how you spell the `import` of that module, but Ruff currently decides whether or not to put the `import` in the `first-party` section based on whether the module is imported with `from .` or not!

---

_Comment by @dylwil3 on 2025-09-18 01:49_

As far as I can tell, the definition of first party I described agrees with the one at least implicitly adopted by `isort`. For example, the configuration option for [`known-first-party`](https://pycqa.github.io/isort/docs/configuration/options.html#known-first-party) has the description:

> Force isort to recognize a module as being part of the _current python project_.

(emphasis my own).

The question of namespace packages and the question of whether first party includes "packages from your organization" is addressed [here](https://github.com/PyCQA/isort/issues/1057#issuecomment-974658141) where it appears they also come to the same conclusion of what first party "ought" to mean (namely - the package under development).

I'm not trying to say any definition is more or less correct, just that I believe the one adopted by `isort` and by Ruff seem to be about the same, whereas the one you are suggesting seems to differ.

But I'd be happy for others to weigh in if I've gotten something muddled here.

I would also be particularly interested to hear if the configuration option `known-first-party` does not solve the issue for your use-case, since I would consider that a bug or a deficiency in Ruff.


---

_Comment by @godlygeek on 2025-09-18 06:46_

> I would also be particularly interested to hear if the configuration option `known-first-party` does not solve the issue for your use-case, since I would consider that a bug or a deficiency in Ruff.

It doesn't allow me to get the ordering I'd like, at least not by itself.

Setting `known-first-party = ["someorg"]` when developing the `someorg.foo` module means `someorg.foo` and `someorg.bar` are both treated as first-party and combined into the same section. This does match the behavior that Ruff had prior to 0.13, but I don't think this was the ideal behavior, either.

I consider it useful for readers of the code to be able to see at a glance what's part of the current repo and what's not, as well as to see at a glance what's coming from PyPI and what's coming from a private package repo instead. Ruff's defaults currently combine stuff that's coming from PyPI with stuff that's coming from our private package index (because other `someorg.` modules are treated as third party). If I set `known-first-party = ["someorg"]`, Ruff will instead combine stuff that's coming from our private package index with stuff that's local to the current repo. But neither of those behaviors are what I want. I don't want other modules developed by my company to be sorted in the same section as the current module's imports, nor do I want them to be sorted into the 3rd party imports section, since they are neither part of the current project nor are they 3rd party (at least, not by my understanding of what the terms "first party" and "third party" mean).

The grouping that I'd like, and that seems natural to me based on the names of the 3 sections, is:

- `local-folder` - the module being developed in the current folder (regardless of whether the import statements referring to that module are absolute or relative)
- `first-party` - other modules made by the same party as the module being developed in the current folder (in my case, by my company)
- `third-party` - non-stdlib modules made by different parties than the module being developed in the current directory

---

_Comment by @MichaReiser on 2025-09-18 07:04_

> Setting known-first-party = ["someorg"] when developing the someorg.foo module means someorg.foo and someorg.bar are both treated as first-party and combined into the same section. This does match the behavior that Ruff had prior to 0.13, but I don't think this was the ideal behavior, either.

To clarify my understanding. Is `someorg` a namespace package where some sub-packages are first-party and others come from pypi? 

---

_Comment by @godlygeek on 2025-09-18 07:14_

No, it is a namespace package owned by an organization, and we can imagine that every package below it is published only to a private package repository and not to pypi.org

That is, when developing `someorg.http_library`, I want the imports of `someorg.tcp_library` to be sorted into a separate section than the ones for `someorg.http_library` (since it's a separate library, not part of the current project), and also separate from the modules that aren't named `someorg.` at all (since I want readers to be able to quickly distinguish what we own versus what's an open source dependency).

I want 3 sections: the thing in this repo, other things owned by the same people as the thing in this repo, other things owned by other people than the thing in this repo. 

---

_Comment by @MichaReiser on 2025-09-18 07:22_

I think that should work if you use `known-first-party = ["someorg.http_library"]` and e.g. `known-first-party = ["someorg.tcp_library"]) 

You may need to use [custom sections](https://docs.astral.sh/ruff/settings/#lint_isort_sections) if you want to group them into more sections than just first/third party

---

_Comment by @godlygeek on 2025-09-18 07:54_

We've already got the right number of sections for the behavior that I want, the problem is that none of the 3 sections are being used for what their name says.

If I understand correctly, the current default behavior is:
- The `local-folder` section contains all the relative imports
- The `first-party` section contains all the absolute imports of modules in the local folder
- The `third-party` section contains all the absolute imports of other non-stdlib modules

All 3 of these seem wrong to me. The `local-folder` section doesn't include absolute imports of modules that are in the local folder (and it may include relative imports of modules that are _not_ in the local folder, since you can do a relative import of a different subpackage of a common parent namespace package). The `first-party` section is limited to what's in the local folder, rather than consisting of modules made by a particular party. And the `third-party` section likewise doesn't contain modules made by a different party than the first party, but instead can contain any other module regardless of what party owns it. None of the 3 names seem to bear any relationship to what's actually sorted into those sections!

The behavior I'd like to see instead is:
- The `local-folder` section contains all imports of modules in the local folder (regardless of whether those imports are relative or absolute)
- The `first-party` section contains all imports of modules owned by the same party as the modules in the local folder
- The `third-party` section contains all imports of modules owned by other parties than the modules in the local folder

I'm not asking to add new sections, I'm asking for these 3 sections to be used for the thing their name says they're for: for `local-folder` to be used for the modules in the current folder (regardless of how they're imported), and for `first-party` to be used for modules owned by the same party as owns the local folder.

---

_Comment by @MichaReiser on 2025-09-18 08:22_

> The local-folder section contains all the relative imports

Yes, all relative imports (imports starting with one or more `.`) are categorized as local folder. I can see how the name `local-folder` might be misleading. The local-folder doesn't mean the same as "it's a file from your project". I think a better name for this section would have been relative because that's really what it is. 

> The first-party section contains all the absolute imports of modules in the local folder

No, first-party imports are all modules that are part of your project (and part of a package). A file is part of your project if it is in a sub-directory of [`src`](https://docs.astral.sh/ruff/settings/#src). See https://github.com/astral-sh/ruff/blob/512395f4e6a52dd54136667cdb7e0401ee8b8a0d/crates/ruff_linter/src/rules/isort/categorize.rs#L159-L185


> The third-party section contains all the absolute imports of other non-stdlib modules

Sort of. Third-party imports are all modules that haven't been categorized into any other section. Which normally comes down to all modules installed into a venv


I think the main issue here is that the name `local-folder` is misleading. `local-folder` isn't the same as `current-folder`. 

---

_Comment by @godlygeek on 2025-09-18 14:52_

The name `local-folder` is misleading because it implies that things in that section are related by folder, when they aren't. (What does the word "folder" mean in `local-folder`, if not a directory on disk?)

The names `first-party` and `third-party` are misleading because they imply that things are related by the party creating them, when they aren't. (What does the word "party" mean in `first-party` and `third-party`, if not a person or group of people?)

---

_Comment by @MichaReiser on 2025-09-18 14:58_

I understand that the names can be somewhat misleading. I'm happy to accept a PR that clarifies the terminology but I don't think it justifies renaming the groups (especially given that first and third party are widely used in the industry and I don't think they're confusing). 



---

_Closed by @MichaReiser on 2025-09-18 14:59_

---

_Comment by @godlygeek on 2025-09-18 15:30_

I'm not asking to rename the groups, I'm asking to use the groups for the things that their names say they're for.

I would like the `local-folder` group to be used for modules that are in the local (meaning "on this machine") folder (meaning "filesystem directory").

I would like the `first-party` group to be used for modules made by the same party as the local project.

I would like the `third-party` group to be used for modules made by different parties than the local project.

> first and third party are widely used in the industry and I don't think they're confusing

The problem I'm raising with the current behavior is exactly that "first party" and "third party" do have standard industry meanings, and these groups don't match that meaning. We say that an HTTP cookie is first-party if it's created by the company whose website you're visiting, and third-party if it's created by a different company. We say that a Nintendo Switch game is first-party if it's created by Nintendo, and third-party if it's created by a different company. Every definition I can find for the word "party" refers to a person or a group of people. The standard industry term "first party" means "made by the same people".

---

_Comment by @godlygeek on 2025-09-18 17:05_

To be specific about the change that I'd like to see:

I'd ask that what's currently detected as `first-party` by default instead be detected as `local-folder`, so that `local-folder` contains all imports for the module in the current folder, regardless of whether those imports are absolute or relative.

Then, I'd like the documentation for the 3 options to be changed to:

> known-local-folder
> A list of modules to consider being a local folder, regardless of whether they can be identified as such via introspection of the local filesystem.

> known-first-party
> A list of modules to consider first-party, meaning that they are not in the local folder but are maintained by the same people. By default, no other modules are considered first-party.

> known-third-party
> A list of modules to consider third-party. This is the default section for modules that are not recognized as being part of any other section.

That would fix things so that I could use `known-first-party = ["someorg"]` to put the imports of other modules maintained by my organization in a dedicated section, and would fix things so that all of the section names correspond to what's actually sorted into those sections.

---

_Referenced in [astral-sh/ruff#20673](../../astral-sh/ruff/issues/20673.md) on 2025-10-01 19:30_

---
