---
number: 14258
title: How to manage dependencies when multiple git source package depends on another ?
type: issue
state: closed
author: patacoing
labels:
  - question
assignees: []
created_at: 2025-06-25T10:27:34Z
updated_at: 2025-06-25T13:04:56Z
url: https://github.com/astral-sh/uv/issues/14258
synced_at: 2026-01-10T01:25:43Z
---

# How to manage dependencies when multiple git source package depends on another ?

---

_Issue opened by @patacoing on 2025-06-25 10:27_

### Question

Hello everyone,

I'm currently building a lot of python packages (let's call them `Xs`) using uv that will be hosted on github under private repos. Every python package depends on another self-made python package hosted on github (let's call it `P`).

In `pyproject.toml` files I've set up dependencies using git sources.

I have another python repository which depends on `Xs` and on `P` (this one will use all `Xs` packages and uses `P` for type hinting).

Here's an example : 

example of one `X`'s pyproject.toml
```toml
[tool.uv.sources]
connectors-toolkit = { git = "ssh://git@github.com/patacoing/example-shared-package.git",  tag = "0.3.1" }
```

Repository's pyproject.toml
```toml
[tool.uv.sources]
connectors-toolkit = { git = "ssh://git@github.com/patacoing/example-shared-package.git",  tag = "0.3.1" }
```

I think that in order to use that repository, every version of the the `P` package must be the same in every `X`package , doesn't it ?

And this is annoying because it means that if I want to make modifications on `P`, I cannot simply upgrade its version in one of `X` packages, instead I have to upgrade the version of `P` in all `Xs` packages and in the repository as well to be able to use it (or simply being able to run `uv sync`).

Is there any workaround to this ? The best thing would be that different versions of `P` could coexist within the environment of the repository.
Would the usage of a private pypi server provide me the ability to specify a 'minimal' version and would solve the problems ?

In case of different versions of `P` used in the repository and any of `Xs` packages, I run into : 
````
error: Requirements contain conflicting URLs for package `example-shared-package` in split `python_full_version >= '3.13'`:
- git+ssh://git@github.com/patacoing/example-shared-package.git@0.3.1
- git+ssh://git@github.com/patacoing/example-shared-package.git@chore/new-feature-branch
````
After running `uv sync`. 

From the code above, my repository depends on `example-shared-package@chore/new-feature-branch` while `Xs` packages depends on `example-shared-package@0.3.1`

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

---

_Label `question` added by @patacoing on 2025-06-25 10:27_

---

_Comment by @patacoing on 2025-06-25 13:04_

Well I just saw https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides and managed to fix my problem using  dependency overriding. I had to update my repository's pyproject.toml.

It can now depends on the latest version of the common package

```toml
[tool.uv.sources]
example-shared-package = { git = "ssh://git@github.com/oversoc/example-shared-package.git", branch = "chore/case-insensitive-enum" }
package-depending-on-example-shared-package = { git = "ssh://git@github.com/oversoc/package-depending-on-example-shared-package.git" , branch = "main" }

[tool.uv]
override-dependencies = [
    "connectors-toolkit @ git+ssh://git@github.com/oversoc/example-shared-package.git@chore/case-insensitive-enum"
]
```

While the `package-depending-on-example-shared-package` package depends on version `0.3.1` for example

---

_Closed by @patacoing on 2025-06-25 13:04_

---
