```yaml
number: 10956
title: "Clean up `[tool.uv.sources]` after `uv remove --dev <foo>`"
type: issue
state: closed
author: kajic
labels:
  - question
assignees: []
created_at: 2025-01-25T09:18:21Z
updated_at: 2025-01-29T20:39:31Z
url: https://github.com/astral-sh/uv/issues/10956
synced_at: 2026-01-12T16:00:24Z
```

# Clean up `[tool.uv.sources]` after `uv remove --dev <foo>`

---

_@kajic_

### Question

First of all, thank you for your work on uv!

Working a with uv's `add` and `remove` to sometimes promote a dependency `foo` as editable (promote because it's initially listed under `dependencies`), I found that:
```
uv add --dev --editable editable-foo/
```
brings in the following tables into `pyproject.toml`:
```
[dependency-groups]
dev = [
    "foo",
]

[tool.uv.sources]
foo= { path = "editable-foo", editable = true }
```

But then, doing:
```
uv remove --dev foo
```

I end up with:
```
[dependency-groups]
dev = []

[tool.uv.sources]
foo = { path = "editable-foo", editable = true }
```

(nit: It would have been nice to have `[dependency-groups]` go away here.) 
I'm surprised that `foo = { path = "editable-foo", editable = true }` hangs around. Now, for `editable-foo` not to sneak back in, I seem to have to run all subsequent `uv` commands with `--no-sources`.

The workaround I found is to run:
```
uv remove --dev foo
uv remove foo
uv add foo
```

which brings us back almost to where we started from:
```
dependencies = [
    "foo>=0.1.3",
]

[dependency-groups]
dev = []
```

I’m guessing `foo = { path = "editable-foo", editable = true }` is left behind because of the original `foo` entry under `dependencies`, but that doesn’t seem to truly warrant keeping it around since it only got added  to track the editable version of that dependency. Is there some way to remove `foo = { path = "editable-foo", editable = true }` with the initial `uv remove --dev foo` invocation? 

### Platform

Ubuntu 22.04.5 LTS

### Version

0.5.24

---

_Label `question` added by @kajic on 2025-01-25 09:18_

---

_Comment by @charliermarsh on 2025-01-25 13:29_

Hmm, this seems like the right behavior to me. These commands aren't stateful -- we can't know that you only added the editable source for the `--dev` dependency, but not the dependency in `[project.dependencies]`. We should only be removing the source if the dependency itself is no longer referenced in the `pyproject.toml`. Otherwise, we're changing the behavior and semantics of your dependencies without justification.

---

_Comment by @kajic on 2025-01-25 22:26_

That makes sense. 
I suppose that this would be impossible to fix unless the new entry in [uv.tool.sources] held onto the fact that it is for foo in the dev dependency group, and not for the default foo dependency. Right now it’s ambiguous and without user intervention it’s impossible to know what the right action is (remove or keep). 
However, don’t you know (at the time of invoking add) that I only added it for dev since I passed —dev? Or is that not how to think about the semantics of the —dev or —group option? I realize that since this information is not recorded in [uv.tool.sources] there’s no way to go back when remove is used.

---

_Comment by @charliermarsh on 2025-01-29 20:39_

Yeah, the declared source applies to _all_ mentions of the dependency, so it's not specific to the instance of the dependency in the `dev` section. Once you run `uv add --dev --editable editable-foo`, uv now considers the source to be relevant to the new and existing declarations of the dependency.

---

_Closed by @charliermarsh on 2025-01-29 20:39_

---
