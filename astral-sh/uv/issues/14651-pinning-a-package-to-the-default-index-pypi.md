---
number: 14651
title: Pinning a package to the default index (PyPI)
type: issue
state: closed
author: dmrauch
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-07-16T13:32:22Z
updated_at: 2025-08-04T12:40:45Z
url: https://github.com/astral-sh/uv/issues/14651
synced_at: 2026-01-10T01:25:47Z
---

# Pinning a package to the default index (PyPI)

---

_Issue opened by @dmrauch on 2025-07-16 13:32_

### Question

Hi!

First of all, thanks so much for this amazing tool - managing Python projects has truly become a joy!

I couldn't find a closely related issue - hope that's not an oversight on my part.

I am using PyPI as the default index in combination with an additional private index. For one open source package (`package1`) that is provided via PyPI, we needed to create a one-off special version in our private index a few years ago. Of course, this version is far outdated, and we are now using a more up-to-date version from PyPI.

However, because there is one version of that package available on the private index - which when using the default `index-strategy = "first-index"` takes precedence over the default index - we are stuck with the outdated version.

I see two ways around this - but was wondering about a third strategy that I'd prefer over the other for reasons I'll state below:

1. Use `index-strategy = "unsafe-best-match"`: This works and we pick a more up-to-date version from PyPI - but this would open the door for dependency confusion attacks
2. Set `explicit = true` on the private index definition in `[[tool.uv.index]]`: We have >20 internal packages and tbh, I'd like to avoid having to pin all of them in every single `pyproject.toml`

So the possibility of pinning a package to a particular index via `[tool.uv.sources]` caught my attention and I tried
```toml
[tool.uv.sources]
package1 = { index = "default" }
```
but unfortunately this doesn't seem to work. What does seem to work, however, is configuring PyPI as a custom default index and then pinning the package to it:
```toml
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[tool.uv.sources]
package1 = { index = "pypi" }
```

My questions:

- Is this a correct and expected representation of the intended behaviour?
- Am I missing anything why `package1 = { index = "default" }` wouldn't work?
- How would you feel about allowing `package1 = { index = "default" }` to avoid having to re-configure PyPI as a custom default index?

Many thanks!

### Platform

macOS: Darwin 24.5.0 arm64

### Version

uv 0.7.20 (251420396 2025-07-09)

---

_Label `question` added by @dmrauch on 2025-07-16 13:32_

---

_Comment by @charliermarsh on 2025-07-16 13:42_

Thanks for the kind words. I think everything you've written here is correct. You might already be doing it, but I'd probably put the `[[tool.uv.index]]` for the default PyPI index _after_ your private index definition to make sure the private index is consulted first.

I'm a little hesitant to add `index = "default"` or similar as a special-case because there's some ambiguity to it. Does that always refer to PyPI? Or to whichever index is marked as `default = true`?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-16 13:42_

---

_Comment by @dmrauch on 2025-07-16 14:00_

Thanks for the quick follow-up!

But just to be sure about the docs - [Defining an index](https://docs.astral.sh/uv/concepts/indexes/#defining-an-index):

> The default index is always treated as lowest priority, regardless of its position in the list of indexes.

So the order in which I specify the indices in the `pyproject.toml` shouldn't matter, given that I configure PyPI with `default = true`?
```toml
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "private-index"
url = "https://some.where/simple"
```

Regarding the aspect of ambiguity - yeah, I get that. Personally, I'd feel the natural thing to do would be to use whichever index effectively is the default index, keeping in mind that e.g. `uv sync` has a `--default-index` argument. This in fact is how why I kind of expected `index = "default"` to work - since I somehow had the impression that *default* was a kind of "reserved word" for this special status already. But that's perhaps just my intuition and not self-evident.

---

_Comment by @dmrauch on 2025-07-17 15:53_

Unfortunately, this doesn't seem to work reliably. Despite pinning `package1` to the custom default index `pypi`, I often get errors such as
```
  × No solution found when resolving dependencies for split (python_full_version >= '3.13'):
  ╰─▶ Because only package1==5.7.0 is available and private-package>=1.11.1 depends on package1==5.9.4, we can conclude that private-package>=1.11.1 cannot be used.
```
What makes me wonder is that `package1==5.7.0` is precisely the version of the one-off artifact that is available in our private package index. So uv seems to be finding `package1` in the private index - despite the fact that I configured
```
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "private-index"
url = "https://some.where/simple"

[tool.uv.sources]
package1 = { index = "pypi" }
```
So in my understanding it should never look for that package in the private index at all.

---

_Comment by @charliermarsh on 2025-07-17 16:41_

Apologies, the docs are right and I was wrong. The order shouldn't matter if one is marked as `default = true`.

Are you able to create a reproducible example? You can use `https://test.pypi.org/` in lieu of your private index if necessary.

---

_Label `needs-mre` added by @charliermarsh on 2025-07-21 01:48_

---

_Comment by @dmrauch on 2025-08-02 17:16_

Hi @charliermarsh - sorry this took a while! Thanks for your hint with `test.pypi.org` - I believe I have found a situation that reproduces what I describe above:
- `dvc-pandas` has versions 0.0.1 - 0.3.3 on `pypi.org` and only versions 0.0.1 - 0.0.5 on `test.pypi.org`.
  - Not pinning it to `pypi.org` (explicitly defined and configured as the default index) produces the expected and desired error message warning against dependency confusion attacks
  - Pinning it to `pypi.org` works fine with `dvc-pandas = { index = "pypi" }` - so all seems to be good for direct dependencies
- `dvc-pandas` depends on `appdirs`, which has version 1.4.4 on [`test.pypi.org`](https://test.pypi.org/simple/appdirs/) (appears to be yanked, though) and versions 1.1.0 - 1.4.4 on [`pypi.org`](https://pypi.org/simple/appdirs/)

Now, when I run `uv sync` (uv version 0.8.4) with the following `pyproject.toml`:
```
[build-system]
requires = ["setuptools", "setuptools-scm"]
build-backend = "setuptools.build_meta"

[project]
name = "additional-package-index"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = ["dvc-pandas ==0.3.3"]

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "pypi-test"
url = "https://test.pypi.org/simple"
explicit = false

[tool.uv.sources]
appdirs = { index = "pypi" }
dvc-pandas = { index = "pypi" }
```
I get
```
  × No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  ╰─▶ Because only appdirs==1.4.4 is available and appdirs==1.4.4 was yanked, we can conclude that appdirs==1.4.4 cannot be used.
      And because dvc-pandas==0.3.3 depends on appdirs==1.4.4 and your project depends on dvc-pandas==0.3.3, we can conclude that your project's requirements are unsatisfiable.

      hint: `appdirs` was found on https://test.pypi.org/simple, but not at the requested version (appdirs>1.4.4). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By default, uv will only consider versions
      that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy unsafe-best-match` to consider all versions from all indexes, regardless of the
      order in which they were defined.
```
So it appears that uv still tries to get `appdirs` from `test.pypi.org`.

---

_Comment by @charliermarsh on 2025-08-02 20:13_

Ohh, we don't support index pinning for indirect dependencies. Maybe that's what you're running into? See https://github.com/astral-sh/uv/issues/8253.

---

_Comment by @dmrauch on 2025-08-03 17:42_

Yeah, it indeed seems like it. I had already found that adding version vetos for the packages that have one-off versions on the private index mitigated the situation in my case. Just mentioning the packages in a dedicated dependency group (without forbidding the affected versions) works just as fine. Thanks for the pointer to the other ticket, I'll leave a +1 there.

And many thanks for your support, greatly appreciated! 🙏 

---

_Comment by @charliermarsh on 2025-08-04 12:40_

Thank you, merging into that other issue!

---

_Closed by @charliermarsh on 2025-08-04 12:40_

---
