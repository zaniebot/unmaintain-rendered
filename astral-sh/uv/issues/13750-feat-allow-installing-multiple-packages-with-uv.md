```yaml
number: 13750
title: "feat: Allow installing multiple packages with `uv tool install`"
type: issue
state: open
author: Mr-Sunglasses
labels:
  - enhancement
assignees: []
created_at: 2025-05-31T07:20:34Z
updated_at: 2025-06-02T00:30:31Z
url: https://github.com/astral-sh/uv/issues/13750
synced_at: 2026-01-12T16:01:36Z
```

# feat: Allow installing multiple packages with `uv tool install`

---

_@Mr-Sunglasses_

### Summary

uv:

```
kanishk@anton:~$ uv tool install black isort
error: unexpected argument 'isort' found

Usage: uv tool install [OPTIONS] <PACKAGE>

For more information, try '--help'.
```

pipx:

```
kanishk@anton:~$ pipx install isort black
  installed package isort 6.0.1, installed using Python 3.12.3
  These apps are now globally available
    - isort
    - isort-identify-imports
done! ✨ 🌟 ✨
  installed package black 25.1.0, installed using Python 3.12.3
  These apps are now globally available
    - black
    - blackd
done! ✨ 🌟 ✨
```


`uv tool install` support installing one package at a time only, so if have to install 3 packages say `black`, `flake8`, `isort`. I have to install them one by one. It is a good feature to add installing multiple packages ex. `uv tool install black flake8 isort` , this will install all three of them. `pipx` support installation of multiple packages.

Also while researching for this, I see a related issue which is opened in past #6571 , there it is a question for the `--with` flag expectation, so I think we can design that in a way that if:

```
uv tool install black isort
```

it is passed in this way, it will install `black` and `isort`

and if it is given in this way

```
uv tool install black --with isort flake8
```

it will install `black` standalone and `flake8` with `isort`

and if user passed it like this

```
uv tool install black --with isort
```

it will throw the error as there is no package provided which we can install additional dependency `isort` with.

> ps. Thanks astral team for creating uv, it is an amazing tool

---

_Label `enhancement` added by @Mr-Sunglasses on 2025-05-31 07:20_

---

_Comment by @charliermarsh on 2025-06-02 00:30_

I can see the use-case though I'd personally prefer not to support this given that it greatly complicates the semantics around `--with`.

---
