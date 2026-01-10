---
number: 12749
title: Support reading project dependency metadata
type: issue
state: open
author: kevinli1993
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-08T16:50:19Z
updated_at: 2025-04-08T18:25:09Z
url: https://github.com/astral-sh/uv/issues/12749
synced_at: 2026-01-10T01:25:24Z
---

# Support reading project dependency metadata

---

_Issue opened by @kevinli1993 on 2025-04-08 16:50_

### Summary

I would like a command that reports whether a package `X` is specified in the pyproject.toml of a project. 

In its basic form, it might look something like this:
```
$ uv some-magic-command X
$ echo $?   # 0 for "yes"  1 for "no"
```
Conceptually, this is reading and parsing `pyproject.toml`, but with all the corner cases that `uv` handles:
* platform dependence
* `uv` niceties such as `uv --directory`, `uv --package`, and other `UV_*` environment variables.

The exact semantics could be defined like this:
```
uv some-magic-command dependency
```
and the return code would be
* `0` if the project has`dependency` in its pyproject.toml and would be satisfied by the version specified
* `1` if the project has `dependency` in its pyproject.toml and would _not_ be satisfied by the version specified
* `2` if the project _does not_ have  `dependency` in its pyproject.toml

#### Some examples 

(See example section.)

-----

In some ways, this request is spiritually a generalization of the [popular version request](https://github.com/astral-sh/uv/issues/6298), in that I'm hoping to expose some of UV's internal logic to the end-user.  

The use-case is in using `uv` in CLI scripts. 



### Example

```
uv some-magic-command numpy>=1.2
```
Result:
* `0`: if the project has `numpy` in its pyproject.toml, and `numpy==X` would satisfy it for some version `X>=1.2`
* `1`: if the project has `numpy` in its pyproject.toml, but is incompatible with `numpy>=1.2`
* `2`: if the project does not specify `numpy` in its pyproject.toml

```
uv some-magic-command numpy==1.2.26
```
Result:
* `0`: if the project has `numpy` in its pyproject.toml, and `numpy==1.2.26` would satisfy it
* `1`: if the project has `numpy` in its pyproject.toml, but is incompatible with `numpy==1.2.26`
* `2`: if the project does not specify `numpy` in its pyproject.toml

```
uv some-magic-command numpy
```
Result:
* `0`: if the project has `numpy` in its pyproject.toml, and `numpy==X` would satisfy it for some `X`. (Basically, not putting a specifier means `==*`.)
* `1`: if the project has `numpy` in its pyproject.toml, but no version of `numpy` can satisfy it. (In this case, it implies that the project cannot be successfully resolved)
* `2`: if the project does not specify `numpy` in its pyproject.toml


---

_Label `enhancement` added by @kevinli1993 on 2025-04-08 16:50_

---

_Comment by @zanieb on 2025-04-08 17:27_

What's the use-case for this?

---

_Label `wish` added by @zanieb on 2025-04-08 17:27_

---

_Comment by @kevinli1993 on 2025-04-08 17:51_

I maintain a set of (private) packages `A`, `B`, `C`, ..., and need to do some shell scripting to know who depends on who, either directly or transitively.  

The set of dependencies forms a DAG, and I want to do more computations over this DAG; unfortunately can't share details , but the idea is that I want to compute this DAG in the first place, and I would like to use `uv` for that (e.g. instead of parsing by hand).

---

_Comment by @zanieb on 2025-04-08 18:07_

Can you use `uv tree` / `uv pip tree`?

---

_Comment by @kevinli1993 on 2025-04-08 18:12_

`uv tree` is "kind-of" what I want, but not quite.  This is because `uv tree` requires the lockfile `uv.lock` to exist, and creating that might require Internet/intranet access (to get meta information from some index). 

I have a use-case where `uv.lock` is not yet created or not up to date, and I don't have network access; so the source of truth should be the `pyproject.toml`.   

Put another way, for this use-case, I want `pyproject.toml` to be (the only) source of truth, and not `uv.lock`.

(So what I want is kind of like `uv tree --depth 1`, except that `--depth 1` still uses `uv.lock`, not `pyproject.toml`.)

---

_Comment by @zanieb on 2025-04-08 18:18_

> I have a use-case where uv.lock is not yet created or not up to date, and I don't have network access; so the source of truth should be the pyproject.toml.

This probably isn't something we'll support tbh. It adds a lot of complexity. We can consider it if there are more asks though.

---

_Comment by @kevinli1993 on 2025-04-08 18:25_

Got it - I just figured the underlying logic must be in `uv` somewhere (the logic is required to resolve and generate `uv.lock` to begin with), but I understand the added complexity and maintenance burden if `uv` were to expose it.

Anyway, thanks and I appreciate your thoughts - will keep this issue open for others to find/search for.

---
