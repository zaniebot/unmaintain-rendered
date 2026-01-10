```yaml
number: 8976
title: add support for specifying conflicting extras
type: pull_request
state: merged
author: BurntSushi
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ag/conflicting-groups
created_at: 2024-11-09T18:11:43Z
updated_at: 2024-12-30T12:59:16Z
url: https://github.com/astral-sh/uv/pull/8976
synced_at: 2026-01-10T11:44:29Z
```

# add support for specifying conflicting extras

---

_Pull request opened by @BurntSushi on 2024-11-09 18:11_

This PR adds support for conflicting extras. For example, consider
some optional dependencies like this:

```toml
[project.optional-dependencies]
project1 = ["numpy==1.26.3"]
project2 = ["numpy==1.26.4"]
```

These dependency specifications are not compatible with one another.
And if you ask uv to lock these, you'll get an unresolvable error.

With this PR, you can now add this to your `pyproject.toml` to get
around this:

```toml
[tool.uv]
conflicting-groups = [
    [
      { package = "project", extra = "project1" },
      { package = "project", extra = "project2" },
    ],
]
```

This will make the universal resolver create additional forks
internally that keep the dependencies from the `project1` and
`project2` extras separate. And we make all of this work by reporting
an error at **install** time if one tries to install with two or more
extras that have been declared as conflicting. (If we didn't do this,
it would be possible to try and install two different versions of the
same package into the same environment.)

This PR does *not* add support for conflicting **groups**, but it is
intended to add support in a follow-up PR.

Closes #6981

Fixes #8024

Ref #6729, Ref #6830

This should also hopefully unblock
https://github.com/dagster-io/dagster/pull/23814, but in my testing, I
did run into other problems (specifically, with `pywin`). But it does
resolve the problem with incompatible dependencies in two different
extras once you declare `test-airflow-1` and `test-airflow-2` as
conflicting for `dagster-airflow`.

NOTE: This PR doesn't make `conflicting-groups` public yet. And in a follow-up PR, I plan to switch the name to `conflicts` instead of `conflicting-groups`, since it will be able to accept conflicting extras _and_ conflicting groups.

---

_Comment by @charliermarsh on 2024-11-09 18:19_

Epic!

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-09 18:35_

---

_Review requested from @konstin by @BurntSushi on 2024-11-09 18:35_

---

_Comment by @BurntSushi on 2024-11-09 18:35_

Reviewers should read this commit-by-commit. It should help a lot.

Also, I haven't made any attempt at hiding the schema yet, since I'm guessing we'll want to merge this without exposing it yet. cc @zanieb I know we talked about this a couple days ago.

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicting_groups.rs`:17 on 2024-11-09 18:46_

Nit: I might expect this to be called `empty`.

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicting_groups.rs`:190 on 2024-11-09 18:47_

Nit: "two" instead of "2", sorry!

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicting_groups.rs`:190 on 2024-11-09 18:47_

Also might be worth special-casing zero instead of saying "but found only 0".

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:631 on 2024-11-09 18:51_

To clarify: does this mean conflicting groups must be defined in the workspace root?

Could we instead have each project define its own conflicts, and then remove `package =` from the schema? I don't think it's necessary to allow arbitrary packages to be included in these groups, since we only allow _enabling_ extras for members in the workspace.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/environment.rs`:476 on 2024-11-09 18:52_

Still necessary?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2873 on 2024-11-09 18:54_

This could potentially lead to a lot of forks, but... so be it? Unavoidable.

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicting_groups.rs`:11 on 2024-11-09 18:56_

I am wondering if the terminology here should just around something more general, like "Conflicts", since it has to cover both extras and dependency groups. The use of "groups" here makes it somewhat overloaded.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:734 on 2024-11-09 18:56_

Where did this one go?

---

_@charliermarsh reviewed on 2024-11-09 18:57_

This generally looks good to me. A few questions.

---

_@T-256 reviewed on 2024-11-09 19:44_

---

_Review comment by @T-256 on `docs/reference/settings.md`:40 on 2024-11-09 19:44_

Can you add a test case for this? where packages are different.
I noticed all test cases use `package = "project"`. could you make it as default value to avoid explicitly writing?

---

_@BurntSushi reviewed on 2024-11-11 13:36_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:734 on 2024-11-11 13:36_

No longer needed! Since we should no longer generate these forks in the first place.

The key change (below) is from this:

```rust
       let mut forks = vec![Fork {
            dependencies: vec![],
            markers: MarkerTree::TRUE,
        }];
```

to this:

```rust
        let mut forks = vec![Fork {
            dependencies: vec![],
            env: env.clone(),
        }];
```

where `env` contains the markers from the _parent fork_ instead of just using `MarkerTree::TRUE` and then trying to "combine" the markers with the parent fork at a higher level (which is removed below, in the `with_env` method). This awkward dance is ultimately what led to https://github.com/astral-sh/uv/issues/8676. This commit actually undoes [my fix](https://github.com/astral-sh/uv/pull/8759) and instead fixes it by simply not generating the forks in the first place.

I don't know why we didn't always just start with the parent's markers when creating forks. My very vague recollection is that, at the time, we didn't have a good marker implementation and it led to bugs because it would overall produce more "complicated" markers that our disjoint checking failed on.

---

_@BurntSushi reviewed on 2024-11-11 13:38_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicting_groups.rs`:17 on 2024-11-11 13:38_

Fine by me! Done.

---

_@BurntSushi reviewed on 2024-11-11 13:40_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicting_groups.rs`:190 on 2024-11-11 13:40_

Yeah, I totally get that style. I ended up using `2` intentionally for reasons of "parallelism," since "but found only {len}" would also be written numerically. So I think it would be weird to, e.g., have `at least two entries, but found only 1`.

However, if we're special casing the empty case to make it read a little nicer, we might as well special case the singleton case since those are the only two error cases related to the number of groups. In which case, we can just write the number out in English. :P 

So... long winded way of saying, SGTM. Lol.

---

_@BurntSushi reviewed on 2024-11-11 16:19_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2873 on 2024-11-11 16:19_

Yup. I don't see another path here.

And note that the way I went about this _should_ hopefully minimize forks if possible. For example, one way of dealing with conflicting extras/groups is to use it to create an initial set of forks initially in resolution. And then you don't have to do it here: you just need to rely on forks not including dependencies in the extra/group exclusions. But doing it this way means that _every_ marker-related fork gets duplicated according to the declared conflicts. But with this approach, _only_ the forks that contain a conflict end up getting multiplied. So if there's an optional dependency that only exists in one marker fork and there are ten marker forks, the conflicting exclusions only fork off of that one fork and not all of them. (If that makes sense.)

But it's not immediately obvious to me how to reduce this further or if it's possible.

---

_@BurntSushi reviewed on 2024-11-11 17:09_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:631 on 2024-11-11 17:09_

I think this makes sense to me. From the resolver's perspective, it definitely needs the package name, but from the user's perspective, I can see how it makes sense to keep the conflicting declaration local to where the corresponding extras/groups are themselves defined. And in that context, the package name is implicit.

I do wonder if there exists a conflict that does require the user to specify the package name, but I can't quite think of how that might manifest.

I'll plan to remove `package` from the schema.

---

_@BurntSushi reviewed on 2024-11-11 17:11_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:476 on 2024-11-11 17:11_

Nope. Good catch. Removed.

---

_@BurntSushi reviewed on 2024-11-11 17:13_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicting_groups.rs`:11 on 2024-11-11 17:13_

Yeah @zanieb suggested just using `conflicts`, so I think I'll switch to that. I don't like that it doesn't say _what_ is conflicting, but "extra" and "group" are already such general names that it's hard to think of something more general than that but still captures their essence. And in theory, we could add more types of conflicts in the future, although the use cases aren't quite clear.

---

_@BurntSushi reviewed on 2024-11-11 17:13_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:631 on 2024-11-11 17:13_

(But note that we still need `package` in the lock file.)

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:2192 on 2024-11-11 17:20_

can we use something less heavy than numpy? numpy is a compiled wheel and therefore rather large.

---

_@konstin reviewed on 2024-11-11 17:22_

---

_@BurntSushi reviewed on 2024-11-11 17:47_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:2192 on 2024-11-11 17:47_

Done! I switched over to `sortedcontainers`.

---

_@konstin reviewed on 2024-11-12 11:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:691 on 2024-11-12 11:35_

Can't comment that high, but now diverging packages can be empty and the debug message has a hole:

```
DEBUG Splitting resolution on dummy==0.1.0 over  into 3 resolution with separate markers
```

---

_Comment by @konstin on 2024-11-12 13:29_

Sharing this from our conversation:Currently, the following example leads to an install without `proxy-fork-1` but with `anyio==4.2.0`, while it should conflict, the conflict is unconditional.

```toml
[project]
name = "dummy"
version = "0.1.0"
requires-python = "==3.12.*"
dependencies = [
  "proxy-fork-1[project1,project2]"
]

[tool.uv.sources]
proxy-fork-1 = { path = "../proxy-fork-1" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```toml
[project]
name = "proxy-fork-1"
version = "0.1.0"
requires-python = "==3.12.*"
dependencies = []

[project.optional-dependencies]
project1 = ["anyio==4.1.0"]
project2 = ["anyio==4.2.0"]
```

---

_Comment by @BurntSushi on 2024-11-12 19:24_

@konstin Aye, I think that should be resolved now. Whenever a requirement is seen with an unconditional dependency on a conflicting extra, we error. This is perhaps overly conservative, but if we figure out a way to relax it (and a use case that wants it), we can do so in the future.

---

_@charliermarsh approved on 2024-11-12 20:15_

---

_@konstin reviewed on 2024-11-13 10:28_

---

_Review comment by @konstin on `crates/uv/tests/it/show_settings.rs`:3118 on 2024-11-13 10:28_

The comment is out of date, and i think the example, too? (dev conflicting with dev sounds like it should warn/error)

---

_@konstin reviewed on 2024-11-13 11:31_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:497 on 2024-11-13 11:31_

outdated example

---

_@konstin reviewed on 2024-11-13 11:33_

---

_Review comment by @konstin on `docs/reference/settings.md`:37 on 2024-11-13 11:33_

outdated example

---

_Label `enhancement` added by @konstin on 2024-11-13 12:45_

---

_Comment by @BurntSushi on 2024-11-13 13:48_

@konstin found another interesting case. In this case, resolution fails, even though it should be possible to declare conflicting extras in a way that makes it work. To reproduce, create a `pyproject.toml` with this:

```toml
[project]
name = "dummy"
version = "0.1.0"
requires-python = "==3.12.*"

[project.optional-dependencies]
project1 = [
  "proxy-fork-1[project1]",
  # "dummysub[project1]" # Fails with or without this
]
project2 = [
  "proxy-fork-1[project2]"
  # "dummysub[project2]" # Fails with or without this
]

[tool.uv.sources]
proxy-fork-1 = { path = "./proxy-fork-1" }
dummysub =  { workspace = true }

[tool.uv.workspace]
members = ["dummysub"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
conflicting-groups = [
  [
    { extra = "project1" },
    { extra = "project2" },
  ],
]
```

And another at `dummysub/pyproject.toml`:

```toml
[project]
name = "dummysub"
version = "0.1.0"
requires-python = "==3.12.*"

[project.optional-dependencies]
project1 = [
  "proxy-fork-1[project1]",
]
project2 = [
  "proxy-fork-1[project2]"
]

[tool.uv.sources]
proxy-fork-1 = { path = "../proxy-fork-1" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
conflicting-groups = [
  [
    { extra = "project1" },
    { extra = "project2" },
  ],
]
```

And finally, another one that isn't part of the workspace but is just an "external" package:

```toml
[project]
name = "proxy-fork-1"
version = "0.1.0"
requires-python = "==3.12.*"
dependencies = []

[project.optional-dependencies]
project1 = ["anyio==4.1.0"]
project2 = ["anyio==4.2.0"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

In this case, we have `project1` and `project2` declared as conflicting extras in _both_ `dummy` and `dummysub`. But they manifest as two different sets of conflicting extras. In order to make this example work, I believe they need to all be in one set, because they are all mutually conflicting. But since these extras span two different packages, the easiest way to do this is to bring back `package` (but we can make it optional). That is, in the root `pyproject.toml`, we have this:

```toml
[tool.uv]
conflicting-groups = [
  [
    { extra = "project1" },
    { extra = "project2" },
    { package = "dummysub", extra = "project1" },
    { package = "dummysub", extra = "project2" },
  ],
]
```

And we can remove `conflicting-groups` from `dummysub/pyproject.toml`. Once done, this makes resolution succeed.

The only alternative I can see here, AFAIK, is to automatically combine conflicting groups among workspace members, so that every set of conflicts is combined with every other set of conflicts. But I think this leads to a combinatorial explosion, which would be bad juju. So I think we are left with allowing `package` but making it optional. (And now I think we've come full circle to @zanieb's suggestion. :P)

---

_Comment by @BurntSushi on 2024-11-13 14:14_

The above solution means you can't install both `dummy[project1]` and `dummysub[project1]` at the same time. So I think you actually have to list out all pairs of possible conflicts:

```toml
[tool.uv]
conflicting-groups = [
  [
    { extra = "project1" },
    { extra = "project2" },
  ],
  [
    { package = "dummysub", extra = "project1" },
    { package = "dummysub", extra = "project2" },
  ],
  [
    { extra = "project1" },
    { package = "dummysub", extra = "project2" },
  ],
  [
    { package = "dummysub", extra = "project1" },
    { extra = "project2" },
  ],
]
```

And I do believe you need all four entries here, since you need to declare, e.g., that `dummy[project1]` and `dummysub[project2]` are conflicting.

---

_@BurntSushi reviewed on 2024-11-13 14:40_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/show_settings.rs`:3118 on 2024-11-13 14:40_

Aye, fixed. I'll do some more validation in a follow-up PR.

---

_Merged by @BurntSushi on 2024-11-13 14:52_

---

_Closed by @BurntSushi on 2024-11-13 14:52_

---

_Branch deleted on 2024-11-13 14:52_

---

_Comment by @charliermarsh on 2024-11-13 15:23_

@BurntSushi is it then any different than taking all pairs of groups? I don't fully see the advantage.

---

_Comment by @BurntSushi on 2024-11-13 15:31_

> @BurntSushi is it then any different than taking all pairs of groups? I don't fully see the advantage.

I think the advantage is that not all conflicting extras are mutually exclusionary. So the question becomes then, in what circumstances do you take all pairs? I don't think you want to do it in all cases.

---

_@BurntSushi reviewed on 2024-11-13 16:42_

---

_Review comment by @BurntSushi on `docs/reference/settings.md`:40 on 2024-11-13 16:42_

Ah yeah, I'll add one of these in a follow-up PR. Originally I thought we were going to get rid of `package`, but now it's optional. I'll add the test from the comments below, which should cover "conflicts with distinct `package` values" case.

---

_@BurntSushi reviewed on 2024-11-13 16:52_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:691 on 2024-11-13 16:52_

Ah I missed this. I'll have this fixed in a follow-up PR.

---

_Comment by @BurntSushi on 2024-11-13 16:53_

And also, the above isn't all pairs. For example, `dummy[project1]` and `dummysub[project1]` aren't marked as conflicting. You very specifically want to be able to install both of those at the same time.

---

_Comment by @BurntSushi on 2024-11-13 19:59_

NOTE: I accidentally squash-merged this PR. For anyone looking for better history in the future, my apologies.

---

_Comment by @martinitus on 2024-12-23 12:07_

Hey  folks, I have a followup question to the "conflicting extras / groups" features that were merged the last weeks.

I would like to declare conflicting extras - however these dependencies have no _inherent_ conflict in the package or version. Instead they happen to use the same module namespace  - `databricks-connect` and `pyspark` both use `pyspark` as module names.

What I tried so far was

```toml

[project.optional-dependencies]
databums = ["databricks-connect>=16.0.0",]
vanilla = ["pyspark>=3.5.4",]

[tool.uv]
conflicts = [
    [
        {extra = "databums" },
        {extra = "vanilla" },
    ]
]
```

However, when I run `uv lock` or `uv pip install -v -e  .[databums,vanilla]` or even just `uv pip -e .` it happily installs both packages (and screws up the site-package directory afaik).

I presume the current implementation only prevents scenarios where the dependency resolution would have failed due to conflicting optional dependencies. Whereas what I want is explicitly declaring dependencies as conflicting?

Let me know if I missed something and this should actually be possible!

BTW: I also tried it with optional groups, but groups are not exposed to pip (yet, afaik). As I want to be able to install my package with pip on downstream systems, dependency groups seem to not be an option yet.

-- Edit --
I opened #10238

---

_Comment by @zanieb on 2024-12-23 17:05_

@martinitus I don't think we support that, but it does seem interesting.

Could you open a new issue please?

---
