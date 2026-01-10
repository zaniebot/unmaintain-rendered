```yaml
number: 3404
title: Workspace support
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-05-06T13:48:29Z
updated_at: 2024-05-28T07:41:55Z
url: https://github.com/astral-sh/uv/issues/3404
synced_at: 2026-01-10T05:31:37Z
```

# Workspace support

---

_Issue opened by @konstin on 2024-05-06 13:48_

Workspaces are a feature aimed at supporting multiple packages in the same repository. It makes large projects easier to manage by splitting them into smaller packages with independent dependencies. They handle editable installs and publishing automatically.

They are a commonly supported feature in other ecosystems, see the [Review of large projects on github](https://gist.github.com/konstin/3dce73923cd4e963a580c1a17e27fdf6) and [Review of packaging tools](https://gist.github.com/konstin/ec2e7f6cd351c7123b22b1f4c32a5691). Our implementation will be modeled after cargo and rye.

## Usage

There a two main usage patterns: A root package and helpers, and the flat workspace.

Root package and helpers:

```
albatross
├── packages
│   ├── provider_a
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── provider_a
│   │           ├── __init__.py
│   │           └── foo.py
│   └── provider_b
│       ├── pyproject.toml
│       └── src
│           └── provider_b
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```

Flat workspace

```
albatross
├── packages
│   ├── albatross
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── albatross
│   │           ├── __init__.py
│   │           └── foo.py
│   ├── provider_a
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── provider_a
│   │           ├── __init__.py
│   │           └── foo.py
│   └── provider_b
│       ├── pyproject.toml
│       └── src
│           └── provider_b
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
└── uv.lock

```

## Structure

A workspace consists of a *workspace root* and *members* in the workspace.

The *workspace root* is a directory with a `pyproject.toml`, all members need to be below that directory. The workspace root defines *members* and *exclusions*. All packages below it must either be a member or excluded. The workspace root can be a package itself or it is a *virtual manifest*.

Each member is a directory with a `pyproject.toml` that contains a `[project]` section. Each member is a python package, with a name, a version and dependencies. Workspace members can depend on other workspace members. You can consider the workspace another package source or index, similar to `--find-links`.

## Workspace discovery

Currently in the `uv pip` interface, you have to explicitly provide the input files to use for dependencies. In the post-pip interface, uv will be based around workspaces with packages. For that, we need to find the relevant `pyproject.toml` files.

- Go from the current directory up until we find a `pyproject.toml`. This is the current package.
- If the package `pyproject.toml` contains a `tool.uv.workspace` section, it’s also the workspace root.
- If not, go up again until we find a `pyproject.toml`
    - If it exists and contains a `tool.uv.workspace` section:
        - If we’re in the `members`, it’s the workspace root.
        - If we’re in the `excludes`, the package pyproject.toml is implicitly the workspace root, don’t go any further up.
        - If we’re in neither, error, that’s not allowed.
    - If it does not exist (we end up at the filesystem root), the package `pyproject.toml` is implicitly the workspace root (like cargo)
    - If it exists, but does not contain a `tool.uv.workspace`, ignore, our package in the data or test files of some other package.
- Check: Go up further and check that there isn’t another invalid workspace root above ours that includes us.
- Walk through the directories in the `members` and collect all other workspace member packages
- Check: Between our workspace root and either member package directory, there isn’t any other stray `pyproject.toml`
- Collect all entrypoint/scripts from the entire workspace, so we can run them no matter in which directory we are.
- Lower the requirements of our target package using the information about the available workspace packages. By default, a required workspace package is installed as editable.

## Features beyond MVP
- [ ] Support mixing editables and non-editables in workspaces. Would be mostly solved by https://github.com/astral-sh/uv/issues/3733
- [ ] Proper support for depending on packages in other workspaces. The basic structure is there, but i'm sure i'll find bugs when writing proper tests.
- [ ] Validate design on a large, popular open source project (ping me if you're interested)
- [ ] Integration with bluejay lockfiles. Requires https://github.com/astral-sh/uv/issues/3700
- [ ] Support selecting the current package while in the workspace root, like `cargo run -p`.

---

_Label `preview` added by @konstin on 2024-05-06 13:48_

---

_Comment by @hauntsaninja on 2024-05-06 22:57_

In our monorepo at work we have a fairly flexible layout, with more nesting / categorisation of projects. The rules we have for package discovery is that a) name in pyproject.toml must match folder name (limiting how much we have to read pyproject.toml files to do anything), b) a package cannot have another package inside it (limiting how much directory walking we need to do).

---

_Comment by @zanieb on 2024-05-17 17:47_

cc @potiuk regarding if this would work in Airflow

---

_Comment by @potiuk on 2024-05-20 02:18_

> cc @potiuk regarding if this would work in Airflow

Generally yes. There are of course some details - particularly about optionality of those providers.

1. What we really would need in this case are those use cases:

a) power user (or CI) needs to install "albatross" package and all "providers" together and be able to hack on them together.

b) provider contributor (or CI) should be able to install one (or few, or all) of the "providers" separately in editable mode - but wiith the "albatross"  package installed from PyPi, or URL rather than from sources. this should allow for example to run the tests for main version of "provider_a" against past released version of "albatross" (or specifig tag / branch of "albatross") 

c) albatross contributor should be able to install in editable mode just "albatross" and do not care about all other "providers"

2. Also an important use for airflow is that the providers **might** share the same top-level Python package with the main project, so part of their package structure is overlapping:


```
albatross
├── packages
│   ├── provider_a
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── albatross
│   │           └── providers
│   │                └── provider_a
│   │                   ├── __init__.py
│   │                   └── foo.py
│   └── provider_b
│       ├── pyproject.toml
│       └── src
│   │       └── albatross
│   │           └── providers
│   │                └── provider_b
│   │                   ├── __init__.py
│   │                   └── foo.py
│├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```

I guess that will make things quite a bit difficult - because that means that when you do smth like thatin proivder_a:

`from albatross.providers.provider_b import foo`

It should import foo from provider b.

3) Another important thing is that this should nicely work with IDEs (free PyCharm for sure). The free PyCharm has limitation to only be able to have one "project" - no modules within project so you will not be able to open and hack together on provider_a and _b  or provider_a and albatross (which is very useful to be able to edit both.


---

_Closed by @konstin on 2024-05-28 07:41_

---
