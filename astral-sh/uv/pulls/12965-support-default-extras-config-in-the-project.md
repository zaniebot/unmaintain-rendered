```yaml
number: 12965
title: "Support `default-extras` config in the project interface."
type: pull_request
state: open
author: blueraft
labels:
  - enhancement
assignees: []
draft: true
base: main
head: default-extras
created_at: 2025-04-18T12:41:20Z
updated_at: 2025-06-18T16:40:43Z
url: https://github.com/astral-sh/uv/pull/12965
synced_at: 2026-01-10T11:10:40Z
```

# Support `default-extras` config in the project interface.

---

_Pull request opened by @blueraft on 2025-04-18 12:41_

## Summary

Depends on https://github.com/astral-sh/uv/pull/12964. Closes #8607. Closes #13174.

## Test Plan

`cargo test`.

Note: [The draft PEP 771](https://peps.python.org/pep-0771/) uses the term `default-optional-dependency-keys`, but I've gone with `default-extras` here, since something like `--no-default-optional-dependency-keys` felt a bit too cumbersome to type. Let me know if we'd prefer sticking with the PEP’s naming instead.

---

_Review requested from @Gankra by @zanieb on 2025-04-21 20:16_

---

_Review requested from @konstin by @zanieb on 2025-04-21 20:16_

---

_Label `enhancement` added by @konstin on 2025-04-25 08:25_

---

_Comment by @Gankra on 2025-04-25 14:16_

Apologies for the delay, reviewing now!

---

_Comment by @Gankra on 2025-04-25 20:05_

I need to go back and read the PEP for what they do and don't specify but I wanna call out this fact right away: the `--only` semantics you've implemented here are in fact substantially different from the ones for dependency-groups.

To be clear, I think you're probably *right* to do so, given what I know of extras, but I want to make sure we're going eyes wide open into that fact.

With `--only-group` *it also disables the actual package and its normal dependencies*. This makes perfect sense for dependency-groups, because they're often "some devtools or whatever" and so "install the devtools but not the actual project" is indeed something you might want.

But "install only the extras for a package, and not the package" would generally be a nonsense thing to ask for, right? So we indeed don't want that?

If so then `ExtrasSpecificationInner::prod` should be removed, as the distinction it's intended to track is meaningless, and it's essentially unused in your implementation. The one place it *is* used is in `ExtrasSpecificationInner::is_empty` where I don't think it's really necessary (inclined to say "incorrect" but not 100% confident of that). By contrast the equivalent DependencyGroup API is used in several places, where it makes us throw out a ton of packages.

At that point it's also a question of whether `--only-extra foo` is carrying its weight, as it would *just* be sugar for `--no-default-extras --extra foo`.

---

_Comment by @Gankra on 2025-04-25 20:06_

The impl otherwise looks reasonable (more detailed review of the main refactor in the other PR).

---

_@Gankra reviewed on 2025-04-25 20:07_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:2148 on 2025-04-25 20:07_

rad that you added all these tests, it would be nice to include tests for the other commands you added support to as well.

---

_Comment by @blueraft on 2025-04-26 13:44_

> But "install only the extras for a package, and not the package" would generally be a nonsense thing to ask for, right? So we indeed don't want that?

Yes I think so. 

> At that point it's also a question of whether --only-extra foo is carrying its weight, as it would just be sugar for --no-default-extras --extra foo.

Good point! I think we could skip adding a `--only-extra` argument here.

That said, the draft [PEP 771](https://peps.python.org/pep-0771/#overriding-default-extras) proposes the pip interface so that specifying an extra would implicitly disable default extras. If the PEP gets accepted, we will have to make some changes. 😅

---

_@blueraft reviewed on 2025-04-26 13:44_

---

_Review comment by @blueraft on `crates/uv/tests/it/sync.rs`:2148 on 2025-04-26 13:44_

yes I'll add them! 

---

_@blueraft reviewed on 2025-04-26 13:45_

---

_Review comment by @blueraft on `crates/uv/tests/it/sync.rs`:2148 on 2025-04-26 13:45_

should I add them for the default groups for those commands too? They weren't there for the other commands last time I checked

---

_@Gankra reviewed on 2025-04-28 14:47_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:2148 on 2025-04-28 14:47_

I wouldn't complain if you did but no biggie if you don't.

---

_Comment by @Gankra on 2025-04-28 17:18_

Ok reading the PEP more closely, notable details:

> If extras are explicitly given in a dependency specification, the default extras are ignored. Otherwise, the default extras are installed.

> If the same package is specified multiple times in an installation command or dependency tree, the default extras must be installed if any of the instances of the package are specified without extras. 

> Note that package[] would continue to be equivalent to package and would not be provided as a way to install without default extras (see the [Rejected Ideas](https://peps.python.org/pep-0771/#rejected-ideas) section for the rationale).

> We also note that some tools such as [pip](https://github.com/wheel-next/pip/tree/pep_771) currently ignore unrecognized extras, and emit a warning to the user to indicate that the extra has not been recognized, e.g:
> 
>     $ pip install package[non-existent-extra]
>     WARNING: package 3.0.0 does not provide the extra 'non-existent-extra'
> 
> 
> For tools that behave like this (rather than raising an error), if an extra is recognized as invalid in a dependency specification, it should be ignored and treated as if the user has not passed an explicit extra. If none of the provided extras are valid, default extras should be installed.

> In some cases, package maintainers may want to facilitate installing packages without any default extras. In this case, as will be shown in more detail in [Examples](https://peps.python.org/pep-0771/#examples), the best approach is to define an extra which could be called e.g. minimal or nodefault (the naming would be up to the package maintainer) which would be an empty set of dependencies. If this extra is specified, no default extras will be included, so that e.g. package[minimal] would include only required dependencies and no extras. Note that this requires no additional specification and is a natural consequence of the rule described in [Overriding default extras](https://peps.python.org/pep-0771/#overriding-default-extras).

> The most significant backward-compatibility aspect is related to assumptions packaging tools make about extras – specifically, this PEP changes the assumption that extras are no longer exclusively additive in terms of adding dependencies to the dependency tree, and specifying some extras can result in fewer dependencies being installed (they go on to detail a case where `pip freeze` simplifies `package[minimal]` to `package`, which then results in defaults still applying when you `pip install -r requirements.txt`).

---

_Comment by @charliermarsh on 2025-04-28 17:19_

> But "install only the extras for a package, and not the package" would generally be a nonsense thing to ask for, right? So we indeed don't want that?

Just confirming that, yes, extras are purely additive whereas groups are designed to be sensical "without the package". So only installing a given extra (but not the base package) is probably not something we should support.


---

_Comment by @Gankra on 2025-04-28 17:20_

The pep also links a draft pip implementation https://github.com/pypa/pip/compare/main...wheelnext:pip:pep_771

---

_Comment by @Gankra on 2025-04-28 18:12_

It's interesting because a lot of the PEP's semantics are for when talking about the package as a dependency, but the scope of this PR, by nature of it being a uv extension, means it has no(?) application to the package as a dependency.

So it's really more like dependency-groups where it's purely a "developer of this project" experience improvement. Very interesting, I'm not sure if the CLI flags want to reproduce the behaviour of the PEP.

Although really all the difference is that `--extra foo` becomes `--only-extra foo`?

---

_Comment by @blueraft on 2025-04-28 18:24_

> So it's really more like dependency-groups where it's purely a "developer of this project" experience improvement. Very interesting, I'm not sure if the CLI flags want to reproduce the behaviour of the PEP.

Yes, I think since many of these changes involve package metadata, the resolver will need to handle them. But if the PEP is accepted, I expect it will also influence local development workflows. For example, if a package defines `default-optional-dependency-keys`, we’d likely want to install those by default. However, would we want to maintain two separate fields for it then?

> Although really all the difference is that --extra foo becomes --only-extra foo?

As far as I can tell, yes that seems to be the key difference for us. We could choose to handle this differently in the project interface though.

---

_Comment by @Gankra on 2025-04-28 18:29_

> However, would we want to maintain two separate fields for it then?

We would do the same thing we did with our old non-standard support for dependency-groups, where we take both sets of fields and merge them intelligently, with the understanding that everyone will want to move to the standard system when it's available (the maintenance burden is minimal).

---

_Comment by @Gankra on 2025-04-28 18:43_

Flagging "do we want `--extra foo` to have the semantics of `--only-extra foo`" as a design blocker on landing this.

Context: the (draft) PEP for default-extras specifies that `mypackage` installs the default extras, while `mypackage[foo]` actually disables the defaults and installs only `foo` (the user can do something like `mypackage[recommended, foo]` to recover the defaults, or the developer of the package can make `foo` inherit `recommended` if they don't like this behaviour).

If you want `uv sync --extra foo` to be "as if" you installed `mypackage[foo]` it would follow that `--extra foo` is in fact `--only-extra foo`.

---

_Comment by @sglbl on 2025-05-19 07:22_

I think also related: [10360](https://github.com/astral-sh/uv/issues/10360)

---

_Comment by @zanieb on 2025-06-11 02:07_

I think I'll read the PEP and try to follow-up here to try to unblock this work.

> If you want uv sync --extra foo to be "as if" you installed mypackage[foo] it would follow that --extra foo is in fact --only-extra foo.

I'm not sure we need `--extra foo` to match the semantics of `package[foo]`. Naively, I think I'm fine with `--extra foo` being additive on top of extras and using `--only-extra foo` to disable default extras. I think it'd be confusing otherwise. There's no way to express `--only-extra` in the `[<name>]` syntax, which is why they need that default behavior.

Anyway, I think I should read the PEP before making more claims :)

---

_Assigned to @zanieb by @zanieb on 2025-06-11 02:07_

---

_Assigned to @Gankra by @zanieb on 2025-06-11 02:07_

---

_Comment by @konstin on 2025-06-11 11:45_

The PEP is renaming the key: https://github.com/python/peps/pull/4459

---

_Comment by @blueraft on 2025-06-11 14:02_

> There's no way to express --only-extra in the [<name>] syntax, which is why they need that default behavior.

There were some discussions on introducing a new syntax for this, which was ultimately rejected https://peps.python.org/pep-0771/#syntax-for-deselecting-extras. But I agree with `--extra` being additive, since we are not bound by that limitation.

I've rebased and fixed the conflicts!

---

_Converted to draft by @konstin on 2025-06-18 15:53_

---

_Review request for @konstin removed by @konstin on 2025-06-18 16:40_

---
