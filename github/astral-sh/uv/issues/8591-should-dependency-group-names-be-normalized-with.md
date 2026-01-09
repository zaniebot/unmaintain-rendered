---
number: 8591
title: Should dependency group names be normalized with underscores converted to hyphens?
type: issue
state: closed
author: drmikehenry
labels:
  - needs-decision
assignees: []
created_at: 2024-10-26T12:27:36Z
updated_at: 2025-04-16T00:40:54Z
url: https://github.com/astral-sh/uv/issues/8591
synced_at: 2026-01-07T13:12:18-06:00
---

# Should dependency group names be normalized with underscores converted to hyphens?

---

_Issue opened by @drmikehenry on 2024-10-26 12:27_

Should dependency group names be normalized with underscores converted to hyphens?

Example:

```
uv init example
cd example
uv add --group some_group ruff
grep 'some.group' pyproject.toml
```

with output:

```
some-group = [
```

I'm looking into porting a project using Poetry and Nox, where Nox session names and Poetry group names use underscores.  These names are exposed in `noxfile.py` and on the `nox` command line.  It's looking like these names might need to switch to using hyphens in order to use `uv` dependency groups.  Is this expected?

I'm running uv version 0.4.27 on Ubuntu 24.04.


---

_Comment by @zanieb on 2024-10-26 14:58_

We might want to preserve the literal name you provide but those two names should be equivalent everywhere per the [normalization rules](https://packaging.python.org/en/latest/specifications/name-normalization/#name-normalization). How is this affecting nox?

---

_Label `needs-decision` added by @zanieb on 2024-10-26 14:58_

---

_Comment by @drmikehenry on 2024-10-26 16:59_

Sorry for the lack of clarity.  I don't know of a direct problem with `nox` caused by this normalization.  The question arose while starting to port a project that uses Nox and which has Poetry dependency groups.  I was looking for a way to avoid repeating dependencies from `pyproject.toml` in `noxfile.py`.

As an example, the old `pyproject.toml` defines a `type_check` group:

```toml
[tool.poetry.group.type_check.dependencies]
mypy = "*"
types-toml = "*"
types-setuptools = "*"
```

The old `noxfile.py` defines a Nox session with the same name:

```python
@nox.session()
def type_check(s: nox.Session) -> None:
    s.install(".", "mypy", "types-toml", "types-setuptools")
    s.run("mypy", "src", "tests")
```

Users invoke this session via `nox -s type_check` to perform the defined action.

I was contemplating ways to avoid repeating the list of dependencies and had come up with a prototype based on a little code in `noxfile.py` that parses PEP 735 dependency groups in `pyproject.toml`, allowing something like this:

```python
@nox.session()
def type_check(s: nox.Session) -> None:
    s.install(".", *dep_groups("type_check"))
    s.run("mypy", "src", "tests")
```

This failed because `pyproject.toml` doesn't contains the original group name `type_check`, but instead contains the normalized name `type-check`.

Thanks for the link to the normalization rules.  I hadn't realized they applied to "extras" and, presumably, to dependency group names as well; I think this answers my original question.

I was imagining a convention where the Nox session name (necessarily without hyphens) would be used directly to lookup the dependency group name, but it's easy enough to normalize the name first using the rules you linked.

That's all I needed to know from this issue; I'll leave it to you to close since it's got a "needs-decision" tag.


---

_Comment by @vlcinsky on 2024-10-26 19:57_

Actually the current implementation does not conform to what [PEP735](https://peps.python.org/pep-0735/#specification) states about dependency group names:

> This PEP defines a new section (table) in pyproject.toml files named dependency-groups. The dependency-groups table contains an arbitrary number of user-defined keys, each of which has, as its value, a list of requirements (defined below). These **keys must be [valid non-normalized names](https://packaging.python.org/en/latest/specifications/name-normalization/#valid-non-normalized-names), and must be [normalized](https://packaging.python.org/en/latest/specifications/name-normalization/#normalization) before comparisons.**
> 
> Tools SHOULD prefer to present the original, non-normalized name to users by default. If duplicate names, after normalization, are encountered, tools SHOULD emit an error.

If the implementation would stick to the PEP735, the OP question

> Should dependency group names be normalized with underscores converted to hyphens?

would be answered "No - dependency group names are preserved as given by user" 

and this would be non-issue.

## Discussion
Existing implementation by `uv` has advantages:
- key names are predictable
- conflict resulting of two groups using non-normalized names with the same normalized name has clear resolution
- tools processing pyproject.toml can easily find exact key for any acceptable name and will not miss "My-Group" by trying to access it by "my-group" or "my.group" etc.

Anyway, the PEP states the requirements clearly and there is probably some reason for that. Could Stephen Rosen shed some light on it?

---

_Comment by @zanieb on 2024-10-27 01:02_

@drmikehenry no problem! Thanks for explaining your use case. Per the standards, that `dep_groups` function "MUST" normalize names when performing the lookup. So it's a technically a bug there that the group wasn't found.

@vlcinsky to be clear, I don't think we're in violation of the rules of PEP 735

> These keys must be [valid non-normalized names](https://packaging.python.org/en/latest/specifications/name-normalization/#valid-non-normalized-names), and must be [normalized](https://packaging.python.org/en/latest/specifications/name-normalization/#normalization) before comparisons.

This just means that the keys present in a file must follow the schema described for non-normalized names. It does not specify that we cannot write the normalized name (which is also a valid non-normalized name).

> Tools SHOULD prefer to present the original, non-normalized name to users by default.

This means that when presenting names we _read_ from the file, we should present them in their non-normalized form. We probably don't do this, we generally normalize all of the names we read immediately (e.g., for extras and packages too). Regardless, this isn't a "MUST" and I don't think it's necessarily applicable here since we are normalizing a name during a _write_ to the file which is a convenience we provide.

I believe this language matches other Python standards, which generally suggest showing the user the unnormalized name to avoid confusion about what you are referring to. For example, if uv says it "couldn't find package foo defined in group foo-bar" but the group is called "foo.bar" in the `pyproject.toml`, that could be confusing.

I could be convinced we should not normalize the name when creating a group during `uv add`, but I'd want to hear more reasons why first.

@sirosen is welcome to weigh in, though there is no obligation.



---

_Comment by @drmikehenry on 2024-10-27 19:20_

@zanieb Agreed regarding the requirement to normalize dependency group names for comparison purposes.  I've fixed my `dep_groups()` function to normalize before lookup (and I'm now also normalizing the dependency group names as they are read from `pyproject.toml` as well in case of editing by hand or by other tools that don't normalize).

---

_Comment by @sirosen on 2024-10-28 02:54_

> @sirosen is welcome to weigh in, though there is no obligation.

I'm happy to help if there's some ambiguity WRT the spec, but I think you put it all perfectly, so there's not much for me to add! 😁

But I'm happy to listen in and these sorts of threads help educate me more about how the spec is playing out in the wild (and where any mistakes may have been made).

---

_Referenced in [astral-sh/uv#8702](../../astral-sh/uv/issues/8702.md) on 2024-10-30 15:42_

---

_Comment by @drmikehenry on 2025-04-16 00:33_

I saw pull request #12586 (which closes #12585) went through, and it reminded me of this open ticket I submitted.  I wondered if this ticket may be closed now given the general desire to normalize names.  I'm happy with the explanation I received in this ticket, and I thought I'd mention it in case this is an easy ticket to close :-)


---

_Comment by @zanieb on 2025-04-16 00:40_

Thank you for following up!

---

_Closed by @zanieb on 2025-04-16 00:40_

---
