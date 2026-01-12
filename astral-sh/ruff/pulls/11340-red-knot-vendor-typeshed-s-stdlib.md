```yaml
number: 11340
title: "[red-knot] Vendor typeshed's stdlib"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: vendor-typeshed
created_at: 2024-05-08T14:45:48Z
updated_at: 2024-05-09T13:54:21Z
url: https://github.com/astral-sh/ruff/pull/11340
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Vendor typeshed's stdlib

---

_@AlexWaygood_

## Summary

This PR vendors typeshed!

- The first commit vendors the `stdlib` directory from typeshed into a new `crates/red_knot/vendored_typeshed` directory.
- The second commit adjusts various linting config files to make sure that the vendored code is excluded from typo checks, formatting checks, etc.
- The `LICENSE` and `README.md` files are also vendored, but all other directories and files (`stubs`, `scripts`, `tests`, `test_cases`, etc.) are excluded. We should have no need for them (except possibly `stubs/`, discussed in more depth below).
- Similar to the way pyright has a `commit.txt` file in its vendored copy of typeshed, to indicate which typeshed commit the vendored code corresponds to, I've also added a `crates/red_knot/vendored_typeshed/source_commit.txt` file in the third commit of this PR.

One open question is: should we vendor the `stdlib` _and_ `stubs` directories, or just the `stdlib` directory? The `stubs/` directory contains stubs for 162 third-party packages outside the stdlib. Mypy and [typeshed_client](https://github.com/JelleZijlstra/typeshed_client/tree/master)[^1] only vendor the `stdlib` directory; pyright and pyre vendor both the `stdlib` and `stubs` directories; pytype vendors the entire typeshed repo (`scripts/`, `tests/` and all).

In this PR, I've chosen to copy mypy and typeshed_client. Unlike vendoring the `stdlib`, which is unavoidable if we want to do typechecking of the stdlib, it's not _strictly_ necessary to vendor the `stubs` directory: each subdirectory in `stubs` is published to PyPI as a standalone stubs distribution that can be (uv)-pip-installed into a virtual environment. It _might_ be _useful_ for our users if we vendored those stubs anyway, but there are costs as well as benefits to doing so (apart from just the sheer amount of vendored code in the `ruff` repository), so I'd rather consider it separately.

It will be necessary to add infrastructure to keep our vendored copy of typeshed up to date with the upstream repo. I will add this in a followup PR.

## Test Plan

`pre-commit run -a`

[^1]: typeshed_client is used by Quora's type checker [pyanalyze](https://github.com/quora/pyanalyze).

---

_Review requested from @carljm by @AlexWaygood on 2024-05-08 14:45_

---

_Comment by @AlexWaygood on 2024-05-08 14:48_

@carljm I'm expecting a line-by-line review

---

_Comment by @github-actions[bot] on 2024-05-08 15:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-05-08 15:04_

---

_Comment by @MichaReiser on 2024-05-08 15:06_

> @carljm I'm expecting a line-by-line review

What would be helpful is if you could maybe add a few comments of the code that you want to get reviewed. 

---

_Comment by @charliermarsh on 2024-05-08 15:08_

I defer to you all, but I personally would wait to have a PR (with this PR as the base) that leverages these stubs before merging this into main. It ensures that we have everything wired up correctly, and also prevents against us merging these stubs only to have them never be used in the event that we reprioritize or make some other decision.


---

_Comment by @AlexWaygood on 2024-05-08 15:09_

> > @carljm I'm expecting a line-by-line review
> 
> What would be helpful is if you could maybe add a few comments of the code that you want to get reviewed.

Sorry, that was meant to be a joke :(

I tried to provide a guide to this in the PR description. I don't expect any code in the first commit to be reviewed, as that code is all vendored. I would appreciate a review on the design decisions I outlined in the PR description, and the (very small) configuration changes made in the second and third commit. (One question with the third commit is: did I miss anything? Do any other configuration files need to be updated?)

---

_Comment by @AlexWaygood on 2024-05-08 15:10_

> I defer to you all, but I personally would wait to have a PR (with this PR as the base) that leverages these stubs before merging this into main. It ensures that we have everything wired up correctly, and also prevents against us merging these stubs only to have them never be used in the event that we reprioritize or make some other decision.

We can wait; this isn't urgent. I discussed with Carl on Monday and we thought this might be an easy first step, but we can hold off with this for now, I think.

---

_Comment by @MichaReiser on 2024-05-08 15:15_

LGTM: I think we should add a READMe on how to update typeshed. For example, I wouldn't have known about the commit.txt file.

---

_Comment by @AlexWaygood on 2024-05-08 15:29_

> LGTM: I think we should add a READMe on how to update typeshed. For example, I wouldn't have known about the commit.txt file.

Sure... Where should I put those docs, do you think? Typeshed's README.md file is one of the few files that I'm vendoring as part of this PR that lies outside the `stdlib/` directory. Would you prefer that I replace that with a README.md of our own explaining that this is vendored code and how to update it? Or should I put those docs somewhere else? (`crates/red_knot/README.md`, maybe?)

As I mentioned in the PR description, I want to add automation to update typeshed on a regular basis as a followup to this PR. For example, mypy gets an automated PR like this every two weeks: https://github.com/python/mypy/pull/17201.

---

_Comment by @MichaReiser on 2024-05-08 16:00_

> crates/red_knot/README.md, maybe?)

Seems fine to me. 

---

_Comment by @zanieb on 2024-05-08 16:05_

Minor nit that I prefer `vendor/typeshed` to `vendored_typeshed`

---

_@carljm approved on 2024-05-08 16:12_

This looks fine to me. I think the main thing that's missing that I would probably want to have before we merge this is at least documentation, and ideally automation, for updating the snapshot of typeshed.

Personally with that done, I'd just go ahead and merge, otherwise whenever we come back to this it'll probably require another round of updating our own lint configs etc that have changed in the meantime.

There's really no "wiring-up" of it in this PR yet that might be wrong, it's just the files themselves. And I think it's very unlikely we don't use this (and if we don't, it's easy to pull out again.)

---

_Comment by @charliermarsh on 2024-05-08 17:13_

What if we decide that it needs to go somewhere else, e.g., in its own crate? Are these strong advantages to merging now vs. waiting to understand how this will actually get used and integrated?

---

_Comment by @AlexWaygood on 2024-05-08 17:35_

> Minor nit that I prefer `vendor/typeshed` to `vendored_typeshed`

Done

> I think the main thing that's missing that I would probably want to have before we merge this is at least documentation, and ideally automation, for updating the snapshot of typeshed.

Added some docs (including instructions for how to update manually) in `crates/red_knot/README.md` 

---

_Review comment by @carljm on `crates/red_knot/README.md`:11 on 2024-05-08 17:38_

Should this be prefixed with a `rm -r`? Otherwise it could result in stray files if a typeshed change removes a file (eg module changing to a package).

---

_@carljm reviewed on 2024-05-08 17:38_

---

_@AlexWaygood reviewed on 2024-05-08 17:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:11 on 2024-05-08 17:40_

good point!

---

_@AlexWaygood reviewed on 2024-05-08 17:49_

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:11 on 2024-05-08 17:49_

fixed in a force-push

---

_Comment by @MichaReiser on 2024-05-09 08:52_

> What if we decide that it needs to go somewhere else, e.g., in its own crate? Are these strong advantages to merging now vs. waiting to understand how this will actually get used and integrated?

I think we should just merge it. I don't see any real disadvantages that would justify not merging now and stacked PRs are still a pain, even if it's just marginal:

* What if we don't need it: I think that's extremely unlikely and is something that can be fixed easily, just delete the files ;) 
* What if we need to move the files. I'm sure Alex thought about where to place the files. But even if it turns out that we need to move the files because we learned something new, the fix is easy and a low effort PR. 


---

_@MichaReiser approved on 2024-05-09 08:53_

---

_Comment by @AlexWaygood on 2024-05-09 11:43_

> What if we decide that it needs to go somewhere else, e.g., in its own crate?

It would surprise me quite a lot if code outside of `red_knot` needed to analyse typeshed's stdlib stubs. I think that would be a sign of bad architecture from us if they did: code from other crates should use high-level APIs provided by red-knot to query the resolved types that red-knot reaches by parsing and analysing the stdlib stubs, rather than analysing the stubs directly from outside red-knot. In any case, I think it will be very easy to move the stubs somewhere else if we decide at a later point in time that it's necessary.

> Are these strong advantages to merging now

No strong advantages, no, other than being able to easily do work in followup PRs on the basis that this information is available to us inside the Ruff repository. And I'm also not sure I see any strong disadvantages. I don't think this locks us into having the stubs in one particular place, or precludes us from changing the way we vendor typeshed in the future. I'm happy to consider different ways of doing this (a PyPI package, or using a git submodule) in followups. But for now I think I'd rather land this and move onto followup work.

---

_Merged by @AlexWaygood on 2024-05-09 11:44_

---

_Closed by @AlexWaygood on 2024-05-09 11:44_

---

_Branch deleted on 2024-05-09 11:44_

---

_Comment by @Skylion007 on 2024-05-09 13:42_

Any reason we didn't vendor it as a git submodule?

---

_Comment by @zanieb on 2024-05-09 13:44_

@Skylion007 we discussed this, but it's simpler to start without a submodule as it introduces changes to contributors' git workflows.

---

_Comment by @AlexWaygood on 2024-05-09 13:50_

> Any reason we didn't vendor it as a git submodule?

I'm open to it, but not sure it's worth the extra complication. As far as I can tell, the chief advantage seems to be that you'd be able to easily see typeshed's history from inside ruff, which might make it easier to bisect specific issues to specific typeshed commits. But my experience at mypy has been that it's rare that you need to do that -- it's usually pretty easy to figure out what typeshed change is to blame for a specific issue if one arises.

Of the four major Python type checkers currently, only one (pytype) uses a git submodule for their vendored typeshed stubs -- that doesn't mean that we _shouldn't_ do something different, but it does suggest that it's not _essential_ to use a git submodule.

---

_Comment by @charliermarsh on 2024-05-09 13:53_

Just to respond: I still wouldn't have merged this personally (especially if it were my own change), since (as a rule of thumb) it's unused code without a defined integration point. Maybe the integration is straightforward, but the point is we can't really know until we've taken the next step. Similarly, maybe we change priorities, and then we have a vendored typeshed sitting around.

Regardless, correct for me to defer to others since I'm in the minority. I feel heard, no need to respond!


---

_Comment by @charliermarsh on 2024-05-09 13:54_

_Separately_ there are some unknowns to me that I'll be interested to watch, but that would _not_ affect merging: How will we ensure that it gets included in the compiled binary? Will these be embedded in the binary, or somehow installed separately on-disk? How will we expose these to the resolver, if they _are_ embedded in the binary? Etc.


---
