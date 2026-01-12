```yaml
number: 7009
title: fix regression in error message for incompatible dependencies in some cases
type: issue
state: open
author: BurntSushi
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-09-04T12:21:47Z
updated_at: 2024-09-04T13:37:52Z
url: https://github.com/astral-sh/uv/issues/7009
synced_at: 2026-01-12T15:59:09Z
```

# fix regression in error message for incompatible dependencies in some cases

---

_@BurntSushi_

To reproduce, create a `pyproject.toml` with these contents:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "datasets < 2.19",
    "datasets >= 2.19 ; python_version > '3.10'"
]
```

The purpose of this test was, I believe, just to check that conflicting dependencies---where one has a marker---would fail to resolve. In particular, on `python_version > '3.10'`, the requirements are clearly incompatible. We cannot satisfy both `datasets < 2.19` and `datasets >= 2.19`.

So if we lock it, we expect an error, and we get one:

```
$ uv-main lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies:
  `-> Because only datasets<2.19 is available and your project depends on datasets>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

To see what the error message used to look like, build uv on commit `4a6bea532104d4c34524df4deef7067da1dafd95` (which is just before #6268 landed). And re-run `uv lock`:

```
$ uv-4a6bea53 lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies:
  `-> Because your project depends on datasets<2.19 and datasets>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

The key difference between these messages is that the latter reports on the fundamental incompatibility between the dependencies, which is arguably a more specific and relatable message. In contrast, the status quo reports that the requirements aren't satisfiable because `datasets>=2.19` isn't actually available. And given our `--exclude-newer` above, this is actually true!

Now if we use `2.18` instead of `2.19`:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "datasets < 2.18",
    "datasets >= 2.18 ; python_version > '3.10'"
]
```

our error message in the status quo changes:

```
$ uv-main lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies:
  `-> Because only datasets<=2.18.0 is available and your project depends on datasets<2.18, we can conclude that your project and datasets>=2.18.0 are incompatible.
      And because your project depends on datasets>=2.18, we can conclude that your project's requirements are unsatisfiable.
```

Where as our error message from `4a6bea53` remains unchanged:

```
$ uv-4a6bea53 lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies:
  `-> Because your project depends on datasets<2.18 and datasets>=2.18, we can conclude that your project's requirements are unsatisfiable.
```

This shows that the poor error message isn't _just_ because `datasets>=2.19` isn't available. That is, there is something else going on here.

Now let's try going back to our original `pyproject.toml`, but change the `requires-python` to something else:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.8"
dependencies = [
    "datasets < 2.19",
    "datasets >= 2.19 ; python_version > '3.10'"
]
```

This is still unresolvable for the same reason, but the error messages on both `main` and `4a6bea53` change such that they are the same! First, current `main`:

```
$ uv-main lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies for split (python_full_version >= '3.11'):
  `-> Because only datasets{python_full_version >= '3.11'}<2.19 is available and your project depends on datasets{python_full_version >= '3.11'}>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

And `4a6bea53`:

```
$ uv-4a6bea53 lock --exclude-newer '2024-03-25T00:00:00Z'
Using Python 3.12.1
  x No solution found when resolving dependencies for split (python_full_version >= '3.11'):
  `-> Because only datasets{python_full_version >= '3.11'}<2.19 is available and your project depends on datasets{python_full_version >= '3.11'}>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

Moreover, if we remove the `--exclude-newer`, then the error messages remain the same, but become even worse (because more versions of `datasets >= 2.19` are available):

```
$ uv-main lock
Using Python 3.12.1
  x No solution found when resolving dependencies for split (python_full_version >= '3.11'):
  `-> Because only the following versions of datasets{python_full_version >= '3.11'} are available:
          datasets{python_full_version >= '3.11'}<=2.19.0
          datasets{python_full_version >= '3.11'}==2.19.1
          datasets{python_full_version >= '3.11'}==2.19.2
          datasets{python_full_version >= '3.11'}==2.20.0
          datasets{python_full_version >= '3.11'}==2.21.0
      and your project depends on datasets<2.19, we can conclude that your project and datasets{python_full_version >= '3.11'}>=2.19.0 are incompatible.
      And because your project depends on datasets{python_full_version >= '3.11'}>=2.19, we can conclude that your project's requirements are unsatisfiable.

$ uv-4a6bea53 lock
Using Python 3.12.1
  x No solution found when resolving dependencies for split (python_full_version >= '3.11'):
  `-> Because only the following versions of datasets{python_full_version >= '3.11'} are available:
          datasets{python_full_version >= '3.11'}<=2.19.0
          datasets{python_full_version >= '3.11'}==2.19.1
          datasets{python_full_version >= '3.11'}==2.19.2
          datasets{python_full_version >= '3.11'}==2.20.0
          datasets{python_full_version >= '3.11'}==2.21.0
      and your project depends on datasets<2.19, we can conclude that your project and datasets{python_full_version >= '3.11'}>=2.19.0 are incompatible.
      And because your project depends on datasets{python_full_version >= '3.11'}>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

What this suggests to me is that the underlying cause of poorer error messages existed before #6268, but something in #6268 changed things to exacerbate it.

In debugging this problem, one thing I observed is that, in `4a6bea53`, the marker on `datasets >= 2.19 ; python_version > '3.10'` is simplified away, because of `requires-python = ">=3.11"`, leaving just `datasets < 2.19` and `datasets >= 2.19`. Both of these are represented as a `PubGrubPackage::Package`. But in #6268, part of its purpose was to stop simplifying markers away inside resolution and instead only do it at the "boundaries" of uv (e.g., when serializing a lock file). So that `python_version > '3.10'` marker (which is equivalent to `python_version >= '3.11'`) ends up staying there. This in turn means that we have a `PubGrubPackage::Package` for `datasets < 2.19` and a `PubGrubPackage::Marker` for `datasets >= 2.19 ; python_version > '3.10'`. When these gets fed into PubGrub, they are treated differently than if they were two `PubGrubPackage::Package` values. And instead of noticing the incompatibility immediately, PubGrub asks for the dependencies of `datasets >= 2.19`, and this takes it down a different error path.

---

_Label `bug` added by @BurntSushi on 2024-09-04 12:21_

---

_Label `error messages` added by @zanieb on 2024-09-04 13:37_

---
