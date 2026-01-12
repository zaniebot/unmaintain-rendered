```yaml
number: 1517
title: Bump version to 0.0.1a26
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: 0.0.1a26
created_at: 2025-11-10T16:52:31Z
updated_at: 2025-11-10T19:10:59Z
url: https://github.com/astral-sh/ty/pull/1517
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1a26

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2025-11-10 16:52_

---

_@MichaReiser reviewed on 2025-11-10 16:53_

---

_Review comment by @MichaReiser on `pyproject.toml`:43 on 2025-11-10 16:53_

I had to update rooster to the latest stable release and I sort of found it easier to just update the version in the `scripts/release.sh`, similar to what we do in ruff. 

---

_@AlexWaygood reviewed on 2025-11-10 17:06_

---

_Review comment by @AlexWaygood on `pyproject.toml`:43 on 2025-11-10 17:06_

That seems reasonable to me. Though I think in that case there's no longer any point in having a uv.lock file if there's nothing pinned in it? Though it also doesn't do any harm it being there, I guess.

---

_@MichaReiser reviewed on 2025-11-10 17:09_

---

_Review comment by @MichaReiser on `pyproject.toml`:43 on 2025-11-10 17:09_

I pinged our core ty maintainer @zanieb to get their blessing/guidance 

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-11-10 17:10_

```suggestion
- Language server: For [semantic tokens](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_semanticTokens), fix range filtering for tokens starting at the end of the requested range ([#21193](https://github.com/astral-sh/ruff/pull/21193))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-11-10 17:11_

```suggestion
- Fix merging of `--exclude` CLI flag and `src.exclude` config-file setting ([#21341](https://github.com/astral-sh/ruff/pull/21341))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2025-11-10 17:14_

not exactly a great user-facing entry but I don't know what to suggest here 🙃

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:24 on 2025-11-10 17:15_

```suggestion
- Do not promote `Literal` types when solving type variables in contravariant positions ([#21164](https://github.com/astral-sh/ruff/pull/21164), <https://github.com/astral-sh/ruff/pull/21171>))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:25 on 2025-11-10 17:15_

```suggestion
- Fix lookup of `__new__` methods on instances ([#21147](https://github.com/astral-sh/ruff/pull/21147))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2025-11-10 17:15_

```suggestion
- Use the top materialization of classes for narrowing in class patterns for `match` statements ([#21150](https://github.com/astral-sh/ruff/pull/21150))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:27 on 2025-11-10 17:16_

```suggestion
- Improve understanding of disjointness for `@final` classes ([#21167](https://github.com/astral-sh/ruff/pull/21167))
```

---

_Review comment by @MichaReiser on `pyproject.toml`:43 on 2025-11-10 17:17_

Okay, what I'm doing is bad... but fixing it is non trivial (at least for a packaging noob like me) because of:

```
Because the requested Python version (>=3.8) does not satisfy Python>=3.11,<4.0 and all versions of rooster-blue depend on Python>=3.11,<4.0, we can conclude that all versions of rooster-blue cannot
      be used.
```

I might land this PR as is, depending on if it gets accepted before I get rescued by @zanieb 

---

_@MichaReiser reviewed on 2025-11-10 17:17_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:28 on 2025-11-10 17:17_

```suggestion
- Fix the inferred signature of the synthesized `__init__` method of a non-dataclass inheriting from a generic dataclass ([#21159](https://github.com/astral-sh/ruff/pull/21159))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2025-11-10 17:17_

```suggestion
- Use the declared attribute type when inferring union attribute assignments ([#21170](https://github.com/astral-sh/ruff/pull/21170))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-11-10 17:18_

```suggestion
- Use declared attribute types as type context when solving type variables ([#21143](https://github.com/astral-sh/ruff/pull/21143))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:35 on 2025-11-10 17:18_

```suggestion
- Don't union in the inferred type of a parameter's default value when inferring the type of an annotated parameter ([#21208](https://github.com/astral-sh/ruff/pull/21208))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:40 on 2025-11-10 17:18_

```suggestion
- Don't provide completions when in a class or function definition ([#21146](https://github.com/astral-sh/ruff/pull/21146))
```

---

_@zanieb reviewed on 2025-11-10 17:18_

---

_Review comment by @zanieb on `pyproject.toml`:43 on 2025-11-10 17:18_

https://docs.astral.sh/uv/concepts/projects/dependencies/#group-requires-python

---

_Review comment by @zanieb on `pyproject.toml`:43 on 2025-11-10 17:19_

I thought we had a hint for this error 🤔 

---

_@zanieb reviewed on 2025-11-10 17:19_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:42 on 2025-11-10 17:19_

```suggestion
- Favor symbols defined in the current file over imported symbols ([#21194](https://github.com/astral-sh/ruff/pull/21194)) and builtin symbols ([#21285](https://github.com/astral-sh/ruff/pull/21285))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:47 on 2025-11-10 17:19_

```suggestion
- Don't assume in diagnostic messages that a `TypedDict` key error is about subscript access ([#21166](https://github.com/astral-sh/ruff/pull/21166))
```

---

_@zanieb reviewed on 2025-11-10 17:20_

---

_Review comment by @zanieb on `pyproject.toml`:43 on 2025-11-10 17:20_

tldr is that we should not drop the `uv.lock` for rooster because doing so gets us locking of all the transitive dependencies both reducing attack surface and brittleness

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:52 on 2025-11-10 17:20_

```suggestion
- Discover the `site-packages` directory from the environment that ty is installed in ([#21286](https://github.com/astral-sh/ruff/pull/21286)), improving the ergonomics of `uvx ty check`
```

---

_@AlexWaygood reviewed on 2025-11-10 17:21_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:54 on 2025-11-10 17:21_

```suggestion
- Use "cannot" consistently over "can not" in diagnostics ([#21255](https://github.com/astral-sh/ruff/pull/21255))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:55 on 2025-11-10 17:23_

```suggestion
- Resolve `from foo import bar` to the `foo.bar` submodule rather than using the `__getattr__` function in `foo/__init__.py` (in situations where they both exist)([#21260](https://github.com/astral-sh/ruff/pull/21260))
```

---

_@AlexWaygood reviewed on 2025-11-10 17:23_

---

_@MichaReiser reviewed on 2025-11-10 17:24_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:10 on 2025-11-10 17:24_

Yeah. I think the alternative is; "Fix panic" 😆 

---

_@MichaReiser reviewed on 2025-11-10 17:25_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:26 on 2025-11-10 17:25_

I was secretely hoping that you had a suggestion for how to eliminate top-materialization in the user facing changelog 😅 

---

_@MichaReiser reviewed on 2025-11-10 17:27_

---

_Review comment by @MichaReiser on `pyproject.toml`:43 on 2025-11-10 17:27_

Thanks @zanieb for saving me!

---

_@AlexWaygood reviewed on 2025-11-10 17:27_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2025-11-10 17:27_

lol. okay. ummm maybe this?

```suggestion
- Fix narrowing of generic classes in class patterns for `match` statements ([#21150](https://github.com/astral-sh/ruff/pull/21150))
```

---

_Closed by @MichaReiser on 2025-11-10 17:28_

---

_Reopened by @MichaReiser on 2025-11-10 17:28_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2025-11-10 17:29_

```suggestion
- Sync vendored typeshed stubs ([#21178](https://github.com/astral-sh/ruff/pull/21178)). [Typeshed diff](https://github.com/python/typeshed/compare/d6f4a0f7102b1400a21742cf9b7ea93614e2b6ec...bf7214784877c52638844c065360d4814fae4c65)
```

---

_@AlexWaygood reviewed on 2025-11-10 17:29_

---

_@AlexWaygood approved on 2025-11-10 17:29_

---

_@AlexWaygood reviewed on 2025-11-10 17:31_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2025-11-10 17:31_

here's a version without "top materialization" for you 😄

```suggestion
- Fix narrowing of generic classes in class patterns for `match` statements ([#21150](https://github.com/astral-sh/ruff/pull/21150))
```

---

_Merged by @MichaReiser on 2025-11-10 17:45_

---

_Closed by @MichaReiser on 2025-11-10 17:45_

---

_Branch deleted on 2025-11-10 17:45_

---

_Comment by @MatthewMckee4 on 2025-11-10 18:48_

What do you use to (i assume) generate these version bumps? Is it all just `rooster`?

---

_Comment by @AlexWaygood on 2025-11-10 18:59_

> What do you use to (i assume) generate these version bumps? Is it all just `rooster`?

Basically! We follow the process described in https://github.com/astral-sh/ty/blob/main/CONTRIBUTING.md#releasing-ty

---

_Comment by @MatthewMckee4 on 2025-11-10 19:10_

Thanks!

---
