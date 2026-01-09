---
number: 8530
title: "\"uv build --all\" command not working?"
type: issue
state: closed
author: lucas-labs
labels:
  - bug
assignees: []
created_at: 2024-10-24T15:11:02Z
updated_at: 2024-10-25T19:06:47Z
url: https://github.com/astral-sh/uv/issues/8530
synced_at: 2026-01-07T13:12:17-06:00
---

# "uv build --all" command not working?

---

_Issue opened by @lucas-labs on 2024-10-24 15:11_

I don't know if this is a bug or if I'm doing something wrong, but I can't get the `uv build --all` command to work.

According to the `uv build --help` command, the `--all` flag should build all packages in the workspace.

```diff
$ uv build --help
...

Options:
      --package <PACKAGE> Build a specific package in the workspace
+     --all               Builds all packages in the workspace
```

I have a workspace with two packages, `lib1` and `lib2`.

```
workspace
├── packages/
│   ├── lib1/
│   │   ├── hello.py
│   │   ├── pyproject.toml
│   │   └── README.md
│   └── lib2/
│       ├── hello.py
│       ├── pyproject.toml
│       └── README.md
└── pyproject.toml
```



In the root `pyproject.toml`, I have the following configuration to define the members of the workspace (I also tried specifying the path to each package individually, like `members = ["packages/lib1", "packages/lib2"]` but it didn't work either):

```toml
...

[tool.uv.workspace]
members = ["packages/*"]
```

When I run `uv build --all`, the following error is raised:

```sh
$ uv build --all
error: No packages found in workspace
```

If I run `uv build --package lib1`, it works as expected, but we are forced to build each package individually.

```sh
$ uv build --package lib1
Building source distribution...
...
Successfully built dist\lib1-0.1.0.tar.gz and dist\lib1-0.1.0-py3-none-any.whl

$ uv build --package lib2
Building source distribution...
...
Successfully built dist\lib2-0.1.0.tar.gz and dist\lib2-0.1.0-py3-none-any.whl
```

Is this a bug or am I missing something / misinterpreting what the `--all` flag is supposed to do?

Thanks!

---

_Renamed from "" uv build --all" command not working?" to ""uv build --all" command not working?" by @lucas-labs on 2024-10-24 15:11_

---

_Comment by @charliermarsh on 2024-10-24 15:34_

Interesting... this works as expected for me:

```
❯ uv init --package foo
Initialized project `foo` at `/Users/crmarsh/workspace/uv/foo`

❯ cd foo

❯ uv init --package bar
Adding `bar` as member of workspace `/Users/crmarsh/workspace/uv/foo`
Initialized project `bar` at `/Users/crmarsh/workspace/uv/foo/bar`

❯ uv init --package baz
Adding `baz` as member of workspace `/Users/crmarsh/workspace/uv/foo`
Initialized project `baz` at `/Users/crmarsh/workspace/uv/foo/baz`

❯ uv build --all
[bar] Building source distribution...
[baz] Building source distribution...
[foo] Building source distribution...
[bar] Building wheel from source distribution...
[baz] Building wheel from source distribution...
[foo] Building wheel from source distribution...
Successfully built dist/bar-0.1.0.tar.gz and dist/bar-0.1.0-py3-none-any.whl
Successfully built dist/baz-0.1.0.tar.gz and dist/baz-0.1.0-py3-none-any.whl
Successfully built dist/foo-0.1.0.tar.gz and dist/foo-0.1.0-py3-none-any.whl
```

---

_Comment by @charliermarsh on 2024-10-24 15:35_

Oh, do your projects have a `[build-system]` field? If not, they're probably being skipped by `--all` (but unintentionally working with `--package`).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-24 15:41_

---

_Comment by @charliermarsh on 2024-10-24 15:41_

I'm going to add some error messages to fix this up.

---

_Label `bug` added by @charliermarsh on 2024-10-24 15:41_

---

_Comment by @lucas-labs on 2024-10-24 15:50_

> Oh, do your projects have a `[build-system]` field? If not, they're probably being skipped by `--all`

Nope they didn't have a `[build-system]` 😅, they are working now after adding a build system. Makes sense.

> (but unintentionally working with `--package`).
> I'm going to add some error messages to fix this up.

Yes, that would be great, in order to avoid confusion.

Thanks for your help again, Charlie!

---

_Referenced in [astral-sh/uv#8537](../../astral-sh/uv/pulls/8537.md) on 2024-10-24 18:01_

---

_Closed by @charliermarsh on 2024-10-25 19:06_

---

_Closed by @charliermarsh on 2024-10-25 19:06_

---
