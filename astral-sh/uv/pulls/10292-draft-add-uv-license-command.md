```yaml
number: 10292
title: "Draft: Add `uv license` command"
type: pull_request
state: open
author: ryanleary
labels: []
assignees: []
draft: true
base: main
head: add-license-v3
created_at: 2025-01-03T16:34:45Z
updated_at: 2025-07-09T17:50:35Z
url: https://github.com/astral-sh/uv/pull/10292
synced_at: 2026-01-12T16:09:13Z
```

# Draft: Add `uv license` command

---

_@ryanleary_

## Summary

Add a new command, `uv license`, to uv to assist with dependency license audit needs in response to #8156. Design of command arguments loosely informed by https://crates.io/crates/cargo-license. 

Example invocation:
```bash
$ cat pyproject.toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "scikit-learn==1.4.1.post1"
]
classifiers = [
    "License :: OSI Approved :: MIT License"
]
$
$ uv license --universal
project: 0.1.0, MIT License
scikit-learn: 1.4.1.post1, BSD License
joblib: 1.3.2, BSD License
numpy: 1.26.4, BSD License
scipy: 1.12.0, BSD License
threadpoolctl: 3.4.0, BSD License
```

As of right now, the implementation is very rough, with this PR intended to gather feedback on the approach, with the initial focus on pulling license information from Trove classifiers. A refined implementation would pivot to following [PEP-639](https://peps.python.org/pep-0639/) guidelines, return license information as SPDX expressions, and gracefully fall back to Trove, and finally the (legacy?) license field.

I'd like feedback on whether or not the current approach:
* is a relatively expensive `license()` method on `Package` objects to trigger calls to `DistributionDatabase.get_or_build_wheel_metadata` sensible?  is a better way that I am missing that would be minimally disruptive to the overall `uv` codebase?
* There are many `Metadata`-related structs within the codebase which explicitly comment that they contain only the subset of fields required for dependency resolution. Adding additional optional fields here breaks this constraint and I'm not sure if is desirable or would be acceptable to the maintainers.

Note: the display code is entirely copied from `uv tree` and cut down. I would rewrite to enable display organized by package or license/serialization to machine-readable summary, etc.

Any feedback appreciated. I am new to both the uv codebase and Rust, but will continue on this if we can agree on an approach.


---

_Converted to draft by @ryanleary on 2025-01-03 18:10_

---

_Comment by @ranjithrajv on 2025-01-16 17:06_

Maybe we could consider using `uv tree --show-licences`, similar to `uv python list --show-urls`.
Just a suggestion, feel free to let me know what you think

---

_Comment by @ryanleary on 2025-01-16 17:49_

I had actually started with that, but abandoned it because you may not want to show a tree. You may want to get a list of all dependencies and their licenses, or you may want to group by licenses, etc. I am not opposed if that's a consensus, the bigger issue is getting the data.

With all that said, until PEP-639 is ratified and becomes commonplace, this will necessarily be a bit of a minefield, and we would have to be very careful about what guarantees are made about the data returned (none), and deal with the fact that licenses may be displayed in inconsistent ways depending on the source project.

---

_Comment by @ryanleary on 2025-01-21 16:24_

@charliermarsh would you mind weighing in on the approach and if you would be supportive to the inclusion of this if cleaned up?

---

_Assigned to @zanieb by @zanieb on 2025-01-22 03:41_

---

_Comment by @zanieb on 2025-01-22 03:42_

I can own taking a look at the broad place of this in our interface.

---

_Comment by @ryanleary on 2025-02-13 15:20_

Thanks @zanieb! 

Two questions:
(1) is this directionally correct? Do you see the addition of something akin to `uv license` as sensible?
(2) any opinions on the scope of the reporting to go after initially? The license information is Python is a mess right now. I'm not sure what "good enough" for a v0 would realistically look like.

---

_Comment by @zanieb on 2025-02-13 15:45_

I'm all for some sort of command to query license information, but there are other, similar use-cases like auditing dependencies for vulnerabilities. I don't want to have a new top-level command for each thing, so this is loosely blocked on the design of a top-level `uv <something>` command which performs queries or audits of your dependencies.

Related 

- https://github.com/astral-sh/uv/issues/9189
- https://github.com/astral-sh/uv/pull/10886

---

_Comment by @ryanleary on 2025-02-13 15:47_

Fair enough. I'll push forward with the machinery of the license data collection then. We could potentially separate this into two efforts:
(1) internal ability to gather license information
(2) exposing that ability via a CLI/public API that is expected to be relatively stable.

---

_Comment by @marhoy on 2025-03-10 15:49_

We need a tool that can check our dependencies for copyleft licenses like GPL.
How close is this PR to being merged?


---

_Comment by @zanieb on 2025-03-10 17:12_

> How close is this PR to being merged?

Not close, because we need a broader coherent design for this kind of interface and that work takes a lot of engagement from our team. We're focusing on other design work right now, like #5903. We're thinking about this though.

---

_Comment by @marhoy on 2025-03-10 21:29_

@zanieb Thanks for the reply! I'm glad to hear that you are thinking about it 😊👍

---

_Comment by @ryanleary on 2025-04-22 18:43_

Now that PEP-639 is final, any suggestions on how to proceed here? There are two questions, I think:
1. how to expose this capability within `uv`, and
2. how should we handle versions that are specified prior to Metadata v2.4, which is far more parseable/reliable, if at all

---

_Comment by @eyalballa on 2025-06-11 06:08_

Hey folks, this is something that could really help me! Any update on the status of this PR or a rough ETA on when it might be merged?
anything I can pitch in to move this forward?

---

_Comment by @ryanleary on 2025-07-09 16:18_

> we need a broader coherent design for this kind of interface and that work takes a lot of engagement from our team

@zanieb any further thoughts on this? would the team be willing to entertain adding this feature in some kind of 'experimental' capacity, with the interface subject to change based on feedback/further refinement? 

it would be great to mainline the core contributions, unblock those who are needing this kind of tool, and further iterate

---

_Comment by @ryanleary on 2025-07-09 16:39_

Also tagging @konstin for input as someone who contributing code related to license parsing since this draft was first written

---

_Comment by @danieleades on 2025-07-09 17:50_

> Now that PEP-639 is final, any suggestions on how to proceed here? There are two questions, I think:
> 
> 1. how to expose this capability within `uv`, and
> 2. how should we handle versions that are specified prior to Metadata v2.4, which is far more parseable/reliable, if at all

To that last point, it would be acceptable (for me) if only metadata v2.4+ was parsed and if older metadata is encountered a warning (and package list) was surfaced. This would allow manual review of the older packages.

For bonus points, you could allow manually configuring the licenses for these packages in a config file. There's prior art for this in cargo-deny. It would need to account for hashing the license information or pinning to a specific version to account for the fact the licence might change between versions.

Obviously going back further than that would be great, but a lack of support for <2.4 need not be a blocker for incremental delivery of this feature. Even a partial implementation would add immediate value

---
