```yaml
number: 17018
title: "`uv add --bounds none` and a corresponding environment variable"
type: issue
state: open
author: jklaiho
labels:
  - enhancement
assignees: []
created_at: 2025-12-07T14:42:24Z
updated_at: 2025-12-12T14:19:47Z
url: https://github.com/astral-sh/uv/issues/17018
synced_at: 2026-01-10T03:11:35Z
```

# `uv add --bounds none` and a corresponding environment variable

---

_Issue opened by @jklaiho on 2025-12-07 14:42_

### Summary

Before migrating to uv, we used pip-compile, manually adding new dependencies in `pyproject.toml` files.  Since most of our projects are Django projects, we tended to only ever add an explicit version specification for Django itself as the "linchpin dependency". Every other dependency we'd just add without a specifier, then run pip-compile a couple of times to generate separate requirement files with pinned versions of everything for dev and prod. (We would use optional depencies for this in an unconventional way that nonetheless worked for us. With uv's single lock file, we've moved to dependency groups.)

Only in the case of problematic versions would we need to add a specifier to any other dependency. This keeps `pyproject.toml` more readable, and the presence of a version specifier on a dependency becomes a meaningful bit of information for the reader: "We specifically need a version below x.y of library z, otherwise the specifier wouldn't be here. Take extra care with this one around upgrades!"

A similar model still works with `uv.lock`, but it involves adding the dependency to `pyproject.toml` manually with no specifier and then running `uv lock`, or remembering to use `uv add --raw` each time.

We would really appreciate the addition of `uv add --bounds none` and, most crucially, an environment variable like `UV_BOUNDS` that would support all the `--bounds` options, enabling a default to be set. This would save us from always having to use `--raw`, an option that has wider implications than merely version specifiers anyway.

Additionally, `uv add --bounds none "somepackage<5.1"` would install and pin in `uv.lock` the most recent 5.0.x version of `somepackage`, but it would still show up as `"somepackage"` in `project.dependencies`, just as if `uv add --bounds none somepackage` had been run when that 5.0.x version happened to be the latest version available.

I realize there are edge cases here to consider (e.g. what happens when using `--bounds none` on a dependency that already exists in `project.dependencies` with a version specifier), but I'm sure those can be worked out.

### Example

`uv add --bounds none ipython`

would be equivalent to having `UV_BOUNDS=none` set somewhere, then running

`uv add ipython`

This would cause `"ipython"` to end up in `project.dependencies` with no version specifier, with the current version at the time of running the command pinned in `uv.lock`.

`uv add --bounds none somepackage<5.1` locks 5.0.x but records it as `"somepackage"` in `project.dependencies`.

---

_Label `enhancement` added by @jklaiho on 2025-12-07 14:42_

---

_Comment by @zanieb on 2025-12-08 12:21_

Adding lower bounds to dependencies can really help with resolution preventing all sorts of undesirable backtracking situations. What is the downside to having them?

---

_Comment by @jklaiho on 2025-12-08 13:05_

I'm not even trying to imply an actual downside to lower bounds, apart from a visually messier `dependencies` list. It's just that for years, we've had zero problems with not having version specifiers on anything but "linchpin" stuff—except when we've actually ran into a real problem on a non-linchpin dependency being too recent. In those cases, we've typically specified an exact older version for it, re-run `pip-compile` and rebuilt our containers with the new requirements file.

All our existing pre-uv `pyproject.toml` files are like this, so it's also a matter of consistency across older and newer projects.

Dependencies listed without version specifiers are present in the Python Packaging User Guide, using them is fully valid, and the guide does not discourage their use. A `none` option to `--bounds` would seem to fit in with the existing ones, especially with the extra comfort of a hypothetical `UV_BOUNDS` environment variable. The current lack of `none` makes uv implicitly somewhat opinionated against unspecified dependencies. It's your prerogative, but I don't see them harmful in a way that would necessitate that opinion.

I appreciate that most people would not choose to use `none`, and I see how uv docs would not actively recommend the option and might indeed remind the user about backtracking concerns associated with it, but I think its inclusion is still a valid request.

---

_Comment by @konstin on 2025-12-11 11:36_

We regularly get bug reports that eventually turn out to be cases where a user got a bad resolution due to missing lower bounds. Even if there is no explicit lower bound in the requirements file, there is already an implicit lower bound in the code through the symbols that are used. Not stating a bound means that choosing any version of the package is valid, even an ancient one that breaks the code that's using it; Notably there's no guarantee that the current working resolution will keep working in the future when more versions are published, due to potentially changed backtracking behavior, or even a change in the resolver's behavior. Adding lower bounds to the requirements file makes the implicit requirement explicit and prevents the packaging tool from violating that assumption.

The Python Packaging User Guide recommends using lower bounds as best practice (e.g., https://packaging.python.org/en/latest/discussions/install-requires-vs-requirements/#install-requires). Where it uses examples without lower bounds, we should add those missing bounds. Some of the examples may predate proper backtracking resolvers, which are mainly affected by this problem (https://pip.pypa.io/en/latest/user_guide/#changes-to-the-pip-dependency-resolver-in-20-3-2020).

---

_Comment by @jklaiho on 2025-12-12 14:19_

Yeah, I'm not going to argue _too_ too much for what is admittedly a niche preference. Upper and exact bounds are supported and can also be footguns, and not having bounds is not always one. My assumption is that anyone actively opting to use `--bounds none`/`UV_BOUNDS=none` would know what they are doing and why, and would accept the (possibly real, possibly theoretical, depending on their situation) risk involved.

In a dream world that conformed to my particular aesthetic preferences I'd have two environment variables: `UV_BOUNDS=none`and `UV_NO_DOWNGRADES=true`, then just use `uv add` to get unbounded dependencies in `pyproject.toml`. If that command were ever to cause a downgrade of a dependency, the process would stop and tell me to determine how I want to resolve the situation by specifying version boundaries manually, or by opting not to use the incoming dependency at all if the version conflict was irreconcilable.

I think I've said all I have to say on the topic; I'll accept whatever the verdict is.

---
