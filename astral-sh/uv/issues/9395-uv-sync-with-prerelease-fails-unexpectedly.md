```yaml
number: 9395
title: "`uv sync` with  `--prerelease` fails unexpectedly"
type: issue
state: open
author: dariocurr
labels:
  - question
  - resolver
assignees: []
created_at: 2024-11-24T13:49:11Z
updated_at: 2025-11-12T17:03:23Z
url: https://github.com/astral-sh/uv/issues/9395
synced_at: 2026-01-10T03:23:53Z
```

# `uv sync` with  `--prerelease` fails unexpectedly

---

_Issue opened by @dariocurr on 2024-11-24 13:49_

I’m working with the following setup:
	1.	library A: currently in development with version `0.2.0.dev2`
	2.	library B: expands the functionality of library A and depends explicitly on `A==0.2.0.dev2`. Library B has version `0.1.0.dev1`
	3.	application: relies on library B and specifies the dependency `B==0.1.0.dev1`

When running `uv sync`, all `--prerelease` options (`if-necessary`, `explicit`, `if-necessary-or-explicit`) fail unless the `allow` one.
The issue arises because there’s a pre-release version in a third-party library dependency.

Since library B explicitly specifies `A==0.2.0.dev2`, I would expect the `explicit` option to handle this scenario.
My intent is to avoid globally allowing pre-release versions (`allow`) across all dependencies and instead restrict it to cases where they are explicitly required, even if these are third-party libraries.

This behaviour works as expected with `pip`, where such explicit versioning doesn’t cause any issues.

uv version: `0.5.4`

---

_Renamed from "Uv sync `--prerelease` missing option" to "Uv sync with  `--prerelease` fails unexpectedly" by @dariocurr on 2024-11-24 13:49_

---

_Comment by @charliermarsh on 2024-11-24 16:27_

I believe this is working as intended. `A==0.2.0.dev2` enables pre-releases for `A`, but not for whatever the transitive dependency is. I think you'd want to either use `allow`, or add the transitive dependency as a first-party dependency with a pre-release marker.

---

_Label `question` added by @charliermarsh on 2024-11-24 16:27_

---

_Label `resolver` added by @samypr100 on 2024-11-24 19:53_

---

_Comment by @dariocurr on 2024-11-25 06:58_

Using allow would mean enabling pre-release versions for all dependencies, which introduces unintended side effects and it's very dangerous.
For instance, if library A has a dependency like `X>=1.1,<1.2`, this approach could select `1.1.5.dev5` instead of the latest stable version `1.1.4 for X`.
These _all-or-nothing_ options of pre-releases are not viable solutions in our scenario.

I strongly believe the `if-necessary` option should address this use case (as `pip` does)

Alternatively, `uv` could consider adding a new option, such as `if-necessary-with-third-party`, to explicitly handle situations where third-party dependencies have **explicit** pre-release requirements (`==`) specified in the dependency chain. If I explicitly specified it, I am explicitly accepting it

Such an enhancement would ensure that explicit versioning requirements are respected without globally relaxing constraints for all dependencies

---

_Comment by @notatallshaw on 2024-11-25 15:37_

I do think the uv option is badly named, to better describe what it does it should be something like `if-only-prereleases`.

---

_Comment by @dariocurr on 2025-01-07 08:21_

Any updates on this? This is critical for us to ship `uv` in our pipelines

---

_Comment by @zanieb on 2025-01-07 18:45_

Why can't you add the pre-release dependency as a first-party requirement to explicitly opt-in?

---

_Comment by @dariocurr on 2025-01-07 18:53_

> Why can't you add the pre-release dependency as a first-party requirement to explicitly opt-in?

Because that's not the way it should be, and I don't want to force my team to do something that shouldn't be done.
Again, this is the way `pip` works. This is the way `cargo` works. I only encountered this behavior when working with `uv`

---

_Comment by @zanieb on 2025-01-07 18:57_

Sorry, but "shouldn't be done" is subjective and `pip` is not a standard. Our motivation and stance this is explained in detail at https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility

---

_Comment by @notatallshaw on 2025-01-07 19:13_

Well, I will point out that uv *isn't* following the standards here, which do state a prerelease should be selected when there is no other option. But this discrepancy is documented in the compatibility guide, and the standards are only well defined for a single specifier, not a resolution.

Also pip doesn't follow the standard here fully either, as it inherits this behavior from packaging and I've found packaging hasn't consistently implemented this: https://github.com/pypa/packaging/issues/856

P.s. let's not get into a discussion on standards here, they're not very clearly laid out or worded, so it can be easy to jump to wrong conclusions for them and this isn't the right forum. If you're interested in the latest thinking on pre-releaees in the standards see: https://discuss.python.org/t/proposal-intersect-and-disjoint-operations-for-python-version-specifiers/71888

I'm hoping we get a new PEP proposal on this soon where we can have a productive discussion on this.



---

_Comment by @neamaddin on 2025-03-15 13:20_

Hey everyone. Can we add another option (we can discuss the name) that would allow installing a specific **subdependency** as a pre-release if it is explicitly specified in the package (e.g., `some-package==0.0.1-pre1`), while installing all other dependencies as usual? If you’re okay with this approach, I’d like to take on this task. I can’t promise it will be done quickly, but it’s better than nothing.

---

_Comment by @notatallshaw on 2025-03-15 14:02_

I am working on that for pip right now: https://github.com/pypa/pip/issues/13221. With the interface mirroring selecting wheels or sdists. 

Once I was a bit further along I was going to ask the astral folks if they had any bike shedding input on the names of the options. But I only work on this in my spare time and it's on a list of different things I am working on, so for pip the likely earliest this will be ready is in a few months, whereas uv seems to get stuff done in a few hours once they agree to it. 

---

_Comment by @foxmulder900 on 2025-08-13 21:09_

> add the transitive dependency as a first-party dependency with a pre-release marker.

Highlighting this as a workable solution for testing purposes, thanks!


---

_Renamed from "Uv sync with  `--prerelease` fails unexpectedly" to "uv sync with  `--prerelease` fails unexpectedly" by @dariocurr on 2025-08-14 08:24_

---

_Renamed from "uv sync with  `--prerelease` fails unexpectedly" to "`uv sync` with  `--prerelease` fails unexpectedly" by @dariocurr on 2025-08-14 08:24_

---

_Comment by @superlevure on 2025-11-12 11:46_

This is a blocker for our company to adopt `uv` unfortunately

> Why can't you add the pre-release dependency as a first-party requirement to explicitly opt-in?

In our case, many of our first-party dependencies are depending on pre-releases of their own dependencies, and having to bring those upward adds a maintenance burden that is not acceptable: we would end up maintaining in sync pre-releases requirements in many different repo and the build CI would break whenever the requirements are not in synced anymore. 

Having a `--prerelease=explicit-third-party-too` (badly named but you get it) would enable `uv` adoption for us. We can't use `--prelease=allow` for the reasons mentioned above. 

Thank you for the work on this amazing tool that is `ruff`, we are looking forward to finally replacing `pip` 

---

_Comment by @dariocurr on 2025-11-12 11:49_

> This is a blocker for our company to adopt `uv` unfortunately
> 
> > Why can't you add the pre-release dependency as a first-party requirement to explicitly opt-in?
> 
> In our case, many of our first-party dependencies are depending on pre-releases of their own dependencies, and having to bring those upward adds a maintenance burden that is not acceptable: we would end up maintaining in sync pre-releases requirements in many different repo and the build CI would break whenever the requirements are not in synced anymore.
> 
> Having a `--prerelease=explicit-third-party-too` (badly named but you get it) would enable `uv` adoption for us. We can't use `--prelease=allow` for the reasons mentioned above.
> 
> Thank you for the work on this amazing tool that is `ruff`, we are looking forward to finally replacing `pip`

I strongly agree with everything you said.
This is a blocker for our company too

---

_Comment by @zanieb on 2025-11-12 14:52_

Unfortunately this isn't just a trivial "add a new option" problem, uv's resolver needs to know what packages pre-releases are enabled for upfront. 

> we would end up maintaining in sync pre-releases requirements in many different repo and the build CI would break whenever the requirements are not in synced anymore.

I'm not sure I understand? You don't need to pin specific pre-releases, just include a pre-release specifier in the dependency. If we added something like

```
[tool.uv]
prerelease-packages = ["...", "..."]
```

would that address your use-case?

---

_Comment by @superlevure on 2025-11-12 15:42_

Thanks for the reply. 

Unfortunately no because we would still end up maintaining a list of all of our third-party dependencies' pre-release dependencies in `prerelease-packages`. We don't want to go that route because suddenly Package A has to explicitly know about Package B dependencies and that doesn't come without issues. For example, when one of the third party package switches from a pre-release to a normal release for one of its dependency, the `prerelease-packages` field you suggest would go obsolete without us noticing. This is the kind of "maintaining requirements in sync" issue that is not acceptable for us. 

---

_Comment by @zanieb on 2025-11-12 16:39_

What's the cost of that becoming obsolete? We could emit a warning (or in the future, a lint diagnostic) if a package in the list isn't being used though.

---

_Comment by @superlevure on 2025-11-12 17:03_

> What's the cost of that becoming obsolete?

Not only it would not adhere to our quality standard, it would actually be dangerous since it would allow pre-releases for a dependency even though not explicitly asked by any third party packages. 

```
# Day 1
package-a  # prerelease-packages = ["package-c"]
├─ package-b # requires "package-c==2.0.0a1"
│  ├─ package-c@2.0.0a1  # <- good: gets installed as expected

# Day 2 - `package-c@2.0.0` gets released
package-a  # prerelease-packages = ["package-c"]  (obsolete now)
├─ package-b # now requires "package-c~=2.0"
│  ├─ package-c@2.0.0  # <- good: gets installed as expected

# Day 3 - `package-c@2.1.0a1` gets released
package-a  # prerelease-packages = ["package-c"]  (obsolete now)
├─ package-b # still requires "package-c~=2.0"
│  ├─ package-c@2.1.0.a1  # <- bad /!\ : gets installed even though not explicitly specified by `package-b`
```



---
