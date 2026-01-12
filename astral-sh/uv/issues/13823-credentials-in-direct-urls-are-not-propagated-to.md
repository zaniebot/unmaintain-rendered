```yaml
number: 13823
title: Credentials in direct URLs are not propagated to transitive URL dependencies
type: issue
state: open
author: zanieb
labels:
  - needs-mre
assignees: []
created_at: 2025-06-03T16:12:06Z
updated_at: 2025-06-09T00:27:43Z
url: https://github.com/astral-sh/uv/issues/13823
synced_at: 2026-01-12T16:01:37Z
```

# Credentials in direct URLs are not propagated to transitive URL dependencies

---

_@zanieb_

> ```
> [build-system]
>     requires      = ["setuptools>=64", "setuptools_scm>=8"]
>     build-backend = "setuptools.build_meta"
> 
> [project]
>     name = "xxx"
>     description = "xxx"
>     dynamic = ["version"]
>     readme = "README.md"
>     requires-python = ">=3.12"
>     dependencies = [
>         "jax[cuda,tpu]==0.6.1",
>         "pandas>=2.2.3",
>         "xxx @ https://<token>@gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl" # this will download but the subsequent authenticated wheel url deps will not work and raise the error
>     ]
> .....
> ``` 

 _Originally posted by @rivershah in [#13791](https://github.com/astral-sh/uv/issues/13791#issuecomment-2935411863)_

---

_Comment by @zanieb on 2025-06-03 16:13_

@rivershah how are the URLs specified in the dependencies? Do they also hard-code the token in the `pyproject.toml`? Or they just have a URL and you want the token to be propagated from the parent `pyprojec.toml`?

---

_Label `bug` added by @zanieb on 2025-06-03 16:13_

---

_Label `needs-mre` added by @zanieb on 2025-06-03 16:13_

---

_Comment by @zanieb on 2025-06-03 17:08_

Okay after a bunch of time setting up a reproduction, I cannot reproduce this

```
uv pip install --reinstall --no-cache 'uv-private-pypackage-parent[wheel] @ https://__token__:*****@gitlab.com/api/v4/projects/70458383/packages/pypi/files/f8484dc92c86ed902a0a3fb3c5ea64eb2c9c6186544e63cb6373cd1dca3e5600/uv_private_pypackage_parent-0.1.0-py3-none-any.whl'
```

where the `uv-private-pypackage-parent` declares the optional dependency 

```
wheel = ["uv-private-pypackage @ https://gitlab.com/api/v4/projects/70458383/packages/pypi/files/33081a82c974308042ae703e1a6978b8c705bcf657b7af3afdbfd5231cb4eb20/uv_private_pypackage-0.1.0-py3-none-any.whl"]
```

---

_Comment by @zanieb on 2025-06-03 17:12_

Similarly,

```
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = [
    "uv-private-pypackage-parent[wheel] @ https://__token__:*****@gitlab.com/api/v4/projects/70458383/packages/pypi/files/f8484dc92c86ed902a0a3fb3c5ea64eb2c9c6186544e63cb6373cd1dca3e5600/uv_private_pypackage_parent-0.1.0-py3-none-any.whl"
]
```

works with `uv sync`

---

_Comment by @rivershah on 2025-06-06 04:34_

@zanieb Thanks for setting this up. Could I please get the token via dm or other means so I can illustrate the exact failure command line? With `--no-cache` flag removed I do get it with our internal packages but I need to be sure we can reproduce via the MRE private packages you have setup. 

---

_Label `bug` removed by @charliermarsh on 2025-06-09 00:27_

---
