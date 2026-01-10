```yaml
number: 2582
title: "WIP: feat: uv pip list --outdated"
type: pull_request
state: closed
author: timleslie
labels: []
assignees: []
base: main
head: timl/pip-list-outdated
created_at: 2024-03-21T05:34:36Z
updated_at: 2024-04-09T16:30:14Z
url: https://github.com/astral-sh/uv/pull/2582
synced_at: 2026-01-10T14:43:31Z
```

# WIP: feat: uv pip list --outdated

---

_Pull request opened by @timleslie on 2024-03-21 05:34_

## Summary

This PR adds support for `uv pip list --outdated`

https://pip.pypa.io/en/stable/cli/pip_list/#cmdoption-o

Fixes https://github.com/astral-sh/uv/issues/2150

## WIP

This PR is currently a work in progress, and I am seeking feedback on the approach 🙏

I have left some comments in the code and would appreciate feedback on both the large scale approach and also any small-scale implementation details.

I have got to the point where running the command correctly identifies those packages which have an update available.

```sh
➜  uv git:(feat/pip-list-outdated) cargo r --bin uv -- pip list --outdated
Resolved 370 packages in 2.33s
Package                           Version
--------------------------------- --------------
aenum                             3.1.11
aiodns                            3.0.0
...
```

* [ ] The output formatting needs updating in order to match `pip`

```sh
➜  uv git:(timl/pip-list-outdated) pip list --outdated                    
Package                           Version        Latest         Type
--------------------------------- -------------- -------------- -----
aenum                             3.1.11         3.1.15         wheel
aiodns                            3.0.0          3.1.1          wheel
...
```

## Test Plan

WIP

---

_Review comment by @timleslie on `crates/uv/src/commands/pip_list.rs`:108 on 2024-03-21 05:35_

We build up a list of requirements by taking all the currently installed packages and constructing `RequirementsSource::Package(name)` with no further version information, implying we want to resolve the latest version.

---

_@timleslie reviewed on 2024-03-21 05:35_

---

_@timleslie reviewed on 2024-03-21 05:36_

---

_Review comment by @timleslie on `crates/uv/src/commands/pip_list.rs`:266 on 2024-03-21 05:36_

Finally, we filter `results` based on whether there is a more recent version of the package available.

---

_@timleslie reviewed on 2024-03-21 05:37_

---

_Review comment by @timleslie on `crates/uv/src/commands/pip_list.rs`:115 on 2024-03-21 05:37_

We crib a large chunk of code from `pip_install` in order to resolve our list of requirements down to a list of packages which are available to install. This is the part where I'm not sure if this is the best approach, or if there's a way to achieve this without so much cribbed/duplicated code.

---

_@timleslie reviewed on 2024-03-21 05:39_

---

_Review comment by @timleslie on `crates/uv/src/commands/pip_list.rs`:101 on 2024-03-21 05:39_

In order to run the package resolver, we set up quite a lot of defaults. In `pip_install`, these are all available as arguments on the command line. If we go with the approach of using the pip-install style resolve it implies we should probably add all of these as options for `pip list` 🤔 This strikes me as perhaps less than ideal.

I wonder if that's the path we want to go down?

---

_@zanieb reviewed on 2024-03-21 14:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_list.rs`:101 on 2024-03-21 14:36_

Yeah that seems less than ideal.

I wonder if `pip install --upgrade --report` should be used instead of `pip list --outdated` for your use-case... hm.

In general it seems weird to run a resolver in `pip list`.

---

_Comment by @samypr100 on 2024-03-23 01:02_

Awesome to see this coming!

One small general though on this one. Feel free to ignore.

I believe this is one command that could use an enhancement over what pip currently has.
1. Supporting uv's `--resolution` modes on this,  I think would be fantastic to leverage this pre-existing functionality.
   * Right now I only see support only for `highest`.
2. Supporting semver-like matching, I think we can leverage the existing versioning spec for this. (see below)

For point `#2`, I'm thinking something like `--target major,minor,patch` very similar to what [pip-check-updates](https://github.com/zehengl/pip-check-updates) does. These could be even incorporated into `--resolution` itself in the future if it's deemed stable enough.

Rationale, I often find myself wondering what are the "patch", "minor", or "major" updates available (for packages that follow semver conventions). In many cases, I might want to update to the latest minor even if there's a major available. In the same vein, I also find myself often wanting to know this answer to direct dependencies over trasitive so `direct` support plays a major role in such situations.



---

_Comment by @zanieb on 2024-03-23 02:29_

It sounds like this deserves its own dedicated command — it doesn't really feel like it deserves to be tacked on as a `pip list` feature. I feel weird about adding all the resolver options to command intended to list the current environment.

---

_Comment by @zanieb on 2024-03-23 02:32_

@samypr100 I like the `--target` idea, could you open a new issue describing that functionality? I don't see why we wouldn't add that to `pip compile` and `pip install`. We should probably discuss it in general.

---

_Comment by @timleslie on 2024-03-24 22:07_

> It sounds like this deserves its own dedicated command — it doesn't really feel like it deserves to be tacked on as a `pip list` feature. I feel weird about adding all the resolver options to command intended to list the current environment.

I agree. The sketch of that approach in this PR I think highlights how out of place this command is under `pip list`. I think the overall use case still stands. I tried a few of the things you suggested and they come close, but have slightly different semantics to what I'm after (I want to know what the latest version of my packages are, whether or not those packages are consistent with my current pyproject.toml/requirements.txt).

I'll not take this PR any further for now. @zanieb Not sure of the protocol, but feel free to close the PR when appropriate.


---

_Comment by @samypr100 on 2024-03-24 22:16_

> @samypr100 I like the --target idea, could you open a new issue describing that functionality? I don't see why we wouldn't add that to pip compile and pip install. We should probably discuss it in general

@zanieb Good point, I'll re-post as an enhancement issue once I'm able, currently struck down with the flu 😷

---

_Comment by @Daverball on 2024-03-25 09:38_

> It sounds like this deserves its own dedicated command — it doesn't really feel like it deserves to be tacked on as a `pip list` feature. I feel weird about adding all the resolver options to command intended to list the current environment.

@zanieb I agree, for the current approach, where it's reusing the package resolver. 

The real way this is supposed to work however, it does make sense for it to be a part of `pip list`, since you are not using a resolver at all (you may be reusing the part of the resolver that talks to the package index however, pip is calling this a `PackageFinder` internally), you are listing the currently installed version of all the packages in the environment alongside the latest version available on the package index (ignoring all pins/overrides), unless the package is already up to date. It's thus just a different way to list the current environment.

This is important information for evaluating your current dependencies. Some of this information would be available in the compiled `requirements.txt`, but it's easier to work with (especially for humans) with output from this command.

At first glance, it looks like here it might be possible to leverage `DistFinder` from `uv-resolver` to accomplish something similar, although it would need a different, more simplistic `select` strategy, that just takes a small list of preferences into account (like e.g. whether you want to prefer wheels over sdists, or whether you want to allow wheels/sdists at all or whether you want to allow pre-releases or yanked releases or want to ignore `requires-python`).

---

_Comment by @rainzee on 2024-04-04 05:37_

What is the progress so far? ❤️ 

---

_Comment by @anudit on 2024-04-04 18:52_

Looking forward to this getting merged

---

_Comment by @zanieb on 2024-04-04 18:56_

Please just upvote the issue if you are interested in this feature. Comments should be reserved for substantive discussion.

As we've noted in the discussion here, we're not sure this is a good path forward for this feature. We will need to do design work to determine a new way to fulfill this use-case.

---

_Comment by @zanieb on 2024-04-09 14:51_

Going to close for book-keeping. Thanks for the poc! I'll be working on a design for this in the future.

---

_Closed by @zanieb on 2024-04-09 14:51_

---

_Comment by @rpodgorny on 2024-04-09 16:30_

> Going to close for book-keeping. Thanks for the poc! I'll be working on a design for this in the future.

please be sure to post link when you start working on this one. i'm really interested in this feature and i don't want to miss any progress on this. ;-) thanks!

---
