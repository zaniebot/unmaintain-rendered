```yaml
number: 14335
title: Update the tilde version specifier warning to include more context
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/tilde-spec
created_at: 2025-06-27T21:44:31Z
updated_at: 2025-07-02T14:08:46Z
url: https://github.com/astral-sh/uv/pull/14335
synced_at: 2026-01-12T16:11:09Z
```

# Update the tilde version specifier warning to include more context

---

_@zanieb_

Follows https://github.com/astral-sh/uv/pull/14008

---

_Label `internal` added by @zanieb on 2025-06-27 21:44_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/requires_python.rs`:79 on 2025-06-27 21:45_

I found this hard to follow and there's a lot of indirection that can be avoided for the case we care about.

---

_@zanieb reviewed on 2025-06-27 21:45_

---

_Comment by @zanieb on 2025-06-27 22:03_

cc @aaron-ang — curious if you have any thoughts here

---

_@aaron-ang reviewed on 2025-06-27 22:36_

---

_Review comment by @aaron-ang on `crates/uv-pep440/src/version_specifier.rs`:861 on 2025-06-27 22:36_

@zanieb could you briefly explain why this is needed? I'm guessing it is to handle edge cases. If so, should we also check for post and local version?

---

_Comment by @aaron-ang on 2025-06-27 22:41_

> cc @aaron-ang — curious if you have any thoughts here

I was trying to reuse as much code as possible., but your implementation is definitely much cleaner!

---

_@zanieb reviewed on 2025-06-27 23:08_

---

_Review comment by @zanieb on `crates/uv-pep440/src/version_specifier.rs`:861 on 2025-06-27 23:08_

Yeah just guarding against edge-cases.

We might want to check if `specifier.version() == specifier.version.only_release()`? I'm not sure if that's costly.

---

_Marked ready for review by @zanieb on 2025-06-27 23:12_

---

_Comment by @potiuk on 2025-07-01 19:11_

I think this warning added in #14008 is quite confusing and @zanieb maybe you can fix it here?

First if all it is very unclear what version it relates to - especially when you have a workspace.
We started to receive this warning in `uv sync` recently in airflow:

![image](https://github.com/user-attachments/assets/82df9c32-011d-4dbf-aaa1-1e841e405d89)

I looked at it and it really makes no sense `~=3.10` is perfectly valid specification, meaning `~=3.10,<4` and this is really what we mean in our specification of providers in our workspace, because our providers should be compatible with generally any future version of Python. Unless we specifically forbid it, anyhow in Airflow we upper bind Airflow itself, and it won't have upper-binding moved until we either check that all provider work with the new upper-binidng or we apply upper-binding to them. Also the message does not say how to fix it ... Is it `~=3.10,<4` explicitly or `>=3.10,<4` -> we are not even sure what we are asked for.

So I thought (creatively) it is about something else - namly `~=3.10,<3.13` we had - because I thought it's about unnecessary `<4` implicltly added. But I replaced all of those with `>=3.10,<3.13` and we still got the same warning. So I am pretty sure it's about `~=3.10` in our provider's pyproject.tomls (deep into our workspace and many of those). Those are the only places left .. But it still does not seem to make a lot of sense -   is this really the intention to warn when someone specified `~=3.10` ? What would be a "better" way of writing it (this one is nice and short). 

Or maybe it's simply a but in the logic?

Or maybe I completely missed something ? (could be ... I was staring at the numbers for a while).

---

_Comment by @zanieb on 2025-07-01 19:25_

Thanks for the feedback! I'll take a deeper look, but briefly... the intent is to warn on `~=3.10` because _most_ users will assume that's locking to `>=3.10,<3.11` not `>=3.10,<4` — for the former, you'd want `~=3.10.0` and for the latter you'd want it to remain the same (but it's probably better to disambiguate with the longer-form specification of `>=3.10,<4`). I can't think of a case where you'd want to write `~=3.10,<4`, as that would expand to `>=3.10,<4,<4`?

---

_Comment by @potiuk on 2025-07-01 20:06_

> because most users will assume that's locking to >=3.10,<3.11 not >=3.10,<

Interesting  - I never had that impression to be honest `~=3.10` is quite clearly specified in https://packaging.python.org/en/latest/specifications/version-specifiers/#compatible-release with good examples and I personally never had any confusion or problem with it. And to be honest it's first time I hear it is confusing, so I am not sure if the `most` statement is correct :D. Do you have some non-anecdotal data to back the `most` statement up?

> for the latter you'd want it to remain the same 

Well, Yeah. Actually I **would* prefer to be able to still say `~=3.10` as it is very nice, short and concise. And the `~=` is weird enough a syntax to raise an eyebrow of people to consider what it is.  And it does seem like departing from the agreed/standard scheme to suggest to change it . Seems like even if someone **really** wants to keep it (I\'ve seen it already used in quite a number of packages, so that calls for them to change and adapt their processes). For one - in our case that version is automatically generated and we would have to redo some of our release processes (including some tests and automation) to change it - plus the specs will still remain in the old packages so that might create even more confusion to be honest.

Is there a way to silence it ? 


---

_Comment by @potiuk on 2025-07-01 20:07_

And BTW. Regardless from the result of the discussion - at the very least - the message should explain what the problem is and how to solve it :)

---

_Comment by @zanieb on 2025-07-01 21:24_

We have a couple comments, e.g.:

- https://github.com/astral-sh/uv/issues/7426#issuecomment-2374979747
- https://github.com/astral-sh/uv/issues/7682#issuecomment-2373547351

Part of the problem is that the `.0` is implied for some uses, e.g., `==3.13` means `==3.13.0`, but this does not apply for the tilde operator. Additionally, the behavior is not consistent across ecosystems, e.g., in Cargo, `~1.2` is equivalent to `>=1.2.0, <1.3.0` (see https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#tilde-requirements).

I'll certainly be considering reducing the visibility of the warning, but we'd need to hear more feedback that it's doing more harm than good.

---

_Comment by @zanieb on 2025-07-01 22:02_

I'm also updating the error message to be more informative

---

_Renamed from "Refactor tilde version specifier handling" to "Update the tilde version specifier error message" by @zanieb on 2025-07-01 22:02_

---

_Label `internal` removed by @zanieb on 2025-07-01 22:02_

---

_Label `enhancement` added by @zanieb on 2025-07-01 22:02_

---

_Renamed from "Update the tilde version specifier error message" to "Update the tilde version specifier warning to include more context" by @zanieb on 2025-07-01 22:02_

---

_@zanieb reviewed on 2025-07-01 22:05_

---

_Review comment by @zanieb on `crates/uv-pep440/src/version_specifier.rs`:895 on 2025-07-01 22:05_

Alas I wrote this as using a borrowed version specifier then wanted to use `bounding_specifiers` alongside a `with_patch` utility to improve error messages and needed the ability to own the specifier. It's not a big deal, but that's why it is this way.

---

_@zanieb reviewed on 2025-07-01 22:06_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/requires_python.rs`:70 on 2025-07-01 22:06_

This moved up a level, where was have access to the project and group names the specifiers come from

---

_Review requested from @konstin by @zanieb on 2025-07-01 22:07_

---

_Comment by @potiuk on 2025-07-02 07:20_

> Part of the problem is that the .0 is implied for some uses, e.g., ==3.13 means ==3.13.0, but this does not apply for the tilde operator. Additionally, the behavior is not consistent across ecosystems, e.g., in Cargo, ~1.2 is equivalent to >=1.2.0, <1.3.0 (see https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#tilde-requirements).

Yep. I understand the potential ambiguity, and that some people might get confused when they are writing their own specification and that's fine and warning by default is good. This is good for people who switch between ecosystems for sure and for newcomers. So I am perfectly fine with having a warning with more explanation in this case  by default (and pointing to the right place where it happens).

But also I think  you have to consider the ripple effect it might have for "seasoned" people - who will suddenly get the warning in their projects, and the only option they have is to change their specification (which might go well into many thousands workflows - It would be worth doing some checks on Github/PyPi to see how many packages already specify ~X.Y and my educated guess will be **A LOT**. So I think a good solution would be to allow - by uv configuration in pyproject.toml file (ideally in the workspace one) to silence the warning - AKA "We know what we are doing". 

That would be way easier to handle by the "seasoned" maintainers (especially if the warning message will hint at the possibilty of disabling it for those who know what they do) - rather than trying to "force ambiguity correction on all maintainers" :). Souds a bit "harsh" - especially that it will affect contributors. If some maintainers do not use uv for development environment, and some of their contributors will - this will cause a tension, because uv users will have warning where non-uv users will not and maintainers will be "pressured" to change their specifications if if they know what they do and chose it consciously.


---

_Comment by @ashb on 2025-07-02 10:03_

I personally don't think the difference in behaviuor across ecosystems is a factor here -- we are dealing with Python dependency specifiers so people need to know how Python ecosystem behaves here, which to me should follow the behaviour set out in the [PyPA-maintained Version Specifiers - § Compatible release](https://packaging.python.org/en/latest/specifications/version-specifiers/#compatible-releases)

> For example, the following groups of version clauses are equivalent:
>
> ```
> ~= 2.2
> >= 2.2, == 2.*
>
> ~= 1.4.5
> >= 1.4.5, == 1.4.*
> ```



---

_Review comment by @konstin on `crates/uv-pep440/src/version_specifier.rs`:895 on 2025-07-02 11:55_

imo we can just always clone those outside the resolver, it's not worth the complexity

---

_@konstin approved on 2025-07-02 11:58_

---

_Review comment by @zanieb on `crates/uv-pep440/src/version_specifier.rs`:895 on 2025-07-02 13:28_

The annoying part is that I didn't want to take an owned value for the `None` case and didn't want to hide a clone. I think if I knew I'd need an owned value later I would have just done it anyway, but I don't expect it to be a big deal to maintain now that it's there.

---

_@zanieb reviewed on 2025-07-02 13:28_

---

_Merged by @zanieb on 2025-07-02 14:08_

---

_Closed by @zanieb on 2025-07-02 14:08_

---

_Branch deleted on 2025-07-02 14:08_

---
