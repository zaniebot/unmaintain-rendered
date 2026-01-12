```yaml
number: 2797
title: "Add warning for packages without lower bound with `--resolution=lowest`"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-04-03T04:34:06Z
updated_at: 2024-07-18T14:56:49Z
url: https://github.com/astral-sh/uv/issues/2797
synced_at: 2026-01-12T15:58:40Z
```

# Add warning for packages without lower bound with `--resolution=lowest`

---

_@zanieb_

per https://github.com/astral-sh/uv/issues/1718#issuecomment-1954420636 we should throw a warning in this case since without a lower bound we will take the _oldest_ version of a package which is actually quite unlikely to be compatible.

---

_Label `help wanted` added by @zanieb on 2024-04-03 04:34_

---

_Label `error messages` added by @zanieb on 2024-04-03 04:34_

---

_Comment by @ottaviohartman on 2024-04-23 22:30_

@zanieb I'd like to try working on this! It's basically my first time in this codebase, so if there's any top-level guidance you can give that would be amazing.

---

_Comment by @zanieb on 2024-04-23 23:06_

Hi! You've picked kind of a hard one for a first task, I'm not sure how to do this off the top of my head.

My first thought is to go to the `Resolver` in `crates/uv-resolver/src/resolver/mod.rs` and do something like

```rust
        if matches!(
            self.selector.resolution_strategy(),
            ResolutionStrategy::Lowest
        ) {
            for requirement in self.requirements {
                if Some(VersionOrUrl::Version(version)) = requirement.version_or_url {
                    if <the version specifier has no lower bound> {
                        warn_user_once!(...)
                    }
                }
            }
        }
```

There are a few open questions:

- Is this the right place to do this? Should we just write a utility and call it from each CLI command implementation?
- How do we check if there is no lower bound on a version specifier?
- This doesn't account for `self.constraints` etc. but needs to, as the bound could be introduced there 

Let me know if you want more than that I can look a bit further.

---

_Comment by @zanieb on 2024-04-23 23:23_

This also only seems sufficient for `LowestDirect` — since these are the direct dependencies. For `Lowest`, I'm not sure when we will know that there is no lower bound present. cc @charliermarsh ?

---

_Comment by @ottaviohartman on 2024-04-25 01:07_

Thanks for the guidance @zanieb ! I really appreciate it.

> Is this the right place to do this? Should we just write a utility and call it from each CLI command implementation?

I've been playing around to see how feasible this is. The option you suggest of adding it to `resolver/mod.rs` is pretty straightforward. Moving into a utility function is a little more clean in the sense that it doesn't touch the inner machinery of resolver, however it will be much more complicated since it needs to constrain requirements (again).

> How do we check if there is no lower bound on a version specifier?

In my testing, this seems to work
```rust
if *dep_version == (Range::full()) {
```

> This doesn't account for self.constraints etc. but needs to, as the bound could be introduced there

One option is to add the warning or utility function [here](https://github.com/astral-sh/uv/blob/3b6e16bb0ed7b6b4384fc06cf53d4da91e33f1e3/crates/uv-resolver/src/resolver/mod.rs#L822-L830) and in similar transitive locations. Would this work?

I'll upload a draft PR to show what I mean.

---

_Comment by @ottaviohartman on 2024-04-25 01:11_

Here's a draft PR with debug logs where the warning might go: https://github.com/ottaviohartman/uv/pull/2/files

---

_Comment by @ottaviohartman on 2024-06-04 01:47_

Opened #4006 , let me know if this is the right track. If not I don't mind closing and handing this task to someone more knowledgeable!

---

_Comment by @zanieb on 2024-07-18 14:33_

I think this is closed in https://github.com/astral-sh/uv/pull/5142 now

---

_Closed by @zanieb on 2024-07-18 14:33_

---

_Comment by @ottaviohartman on 2024-07-18 14:54_

Sorry for being AWOL on this!

---

_Comment by @zanieb on 2024-07-18 14:56_

No problem, it happens to all of us :)

---
