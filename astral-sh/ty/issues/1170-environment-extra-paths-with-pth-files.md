```yaml
number: 1170
title: "`environment.extra-paths` with `.pth` files"
type: issue
state: closed
author: mattangus
labels:
  - question
assignees: []
created_at: 2025-09-11T10:21:27Z
updated_at: 2025-09-11T10:47:08Z
url: https://github.com/astral-sh/ty/issues/1170
synced_at: 2026-01-10T02:06:25Z
```

# `environment.extra-paths` with `.pth` files

---

_Issue opened by @mattangus on 2025-09-11 10:21_

### Question

Is it possible to have a list of generated search paths (e.g. from bazel) to be consumed by `ty`? If not, are there any workarounds?

The files themselves are just a list of folder location to search.

My `ty.toml` looks like this
```toml
[environment]
extra-paths = [
    "./python_path/"
]
```
and the `./python_path` is just a folder with `paths.pth` which contains an list of absolute paths. e.g.

```bash
tree ./python_path
python_path
└── paths.pth

cat ./python_path/paths.pth
/path_to_bazel_cache/bazel/some_hash/external/example_python_dep/site-packages
...
```

Having a look at the code it seems that the `extra-paths` are added to a static import list and not searched for `.pth` files. But I might be missing something.

### Version

ty 0.0.1-alpha.20

---

_Label `question` added by @mattangus on 2025-09-11 10:21_

---

_Comment by @AlexWaygood on 2025-09-11 10:28_

Would you be able to put the `.pth` file in the Python environment's `site-packages` directory? If you can do that, we'll automatically add every entry in the `.pth` file as a "dynamic" search path

---

_Comment by @mattangus on 2025-09-11 10:47_

That seemed to have done the trick! thanks!

---

_Closed by @mattangus on 2025-09-11 10:47_

---
