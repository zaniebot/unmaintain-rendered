```yaml
number: 3154
title: "Add `--python-platform` to `sync` and `install` commands"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/install
created_at: 2024-04-20T00:16:36Z
updated_at: 2024-04-23T01:49:57Z
url: https://github.com/astral-sh/uv/pull/3154
synced_at: 2026-01-10T14:43:31Z
```

# Add `--python-platform` to `sync` and `install` commands

---

_Pull request opened by @charliermarsh on 2024-04-20 00:16_

## Summary

pip supports providing a `--platform` to `pip install`, which can be used to seed an environment (e.g., for use in a container or otherwise). This PR adds `--python-platform` to our commands to support a similar workflow. It has some caveats, which are documented on the CLI.

Closes #2079.


---

_Label `enhancement` added by @charliermarsh on 2024-04-20 00:16_

---

_Label `cli` added by @charliermarsh on 2024-04-20 00:16_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-20 00:16_

---

_Review requested from @konstin by @charliermarsh on 2024-04-20 00:16_

---

_Marked ready for review by @charliermarsh on 2024-04-20 00:16_

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:866 on 2024-04-22 12:11_

I think this was mentioned in #3111, but how can we provide end users with a list of valid triples they can use? I don't think we should just link to the Rust docs unless we specifically want to guarantee a correspondence there. Basically, my concern here is that users won't know what they're allowed to put here.

Honestly, I have a similar concern with pip's `--platform` flag. Its docs are:

```
  --platform <platform>       Only use wheels compatible with <platform>.
                              Defaults to the platform of the running system.
                              Use this option multiple times to specify
                              multiple platforms supported by the target
                              interpreter.
```

After reading that, I have no clue whatsoever what I'm allowed or even supposed to use as a value for this flag.

(And now I'm wondering what the difference is between pip's `--platform` flag and our `--python-platform` flag...)

---

_@BurntSushi approved on 2024-04-22 12:14_

I was having deja vu here, because I thought I had reviewed a change like this already. I did some digging and:

* #3111 added initial support. It added it as `--platform` to _only_ `uv pip compile`.
* #3146 renamed the flag from `--platform` to `--python-platform`, since `pip` also has a `--platform` flag and it doesn't match how ours behaves.
* #3147 adds `--python-platform` to the configuration.
* _This_ PR adds `--python-platform` to the `uv pip sync` and `uv pip install` commands.

So with all that context, I think this change makes sense, but:

* Can we add a test covering this flag for the `uv pip sync` and `uv pip install` commands? I think we have a test for `uv pip compile`, but it would be good to cover the other two as well.
* I mentioned this in a review comment, but what is the difference between pip's `--platform` and this `--python-platform` flag? And does there need to be a difference? Can we match what `pip` does instead?

---

_@konstin approved on 2024-04-22 14:37_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:866 on 2024-04-22 14:53_

We already do! Clap shows all the variants automatically along with their Rustdoc.

---

_@charliermarsh reviewed on 2024-04-22 14:53_

---

_Comment by @zanieb on 2024-04-22 14:55_

> Can we add a test covering this flag for the uv pip sync and uv pip install commands? I think we have a test for uv pip compile, but it would be good to cover the other two as well.

Fwiw we've been intentionally _not_ covering flags across every command. No comment on whether or not that's best.

---

_@BurntSushi reviewed on 2024-04-22 14:56_

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:866 on 2024-04-22 14:56_

Oh sweet. Perfect.

---

_Comment by @charliermarsh on 2024-04-22 15:01_

(I'll answer your other question after goal setting, it's a good one to record in GitHub.)

---

_Comment by @charliermarsh on 2024-04-22 23:31_

> ...but what is the difference between pip's --platform and this --python-platform flag? And does there need to be a difference? Can we match what pip does instead?

Okay, so pip supports `--platform`, `--abi`, and `--python-version`. When provided, it basically takes those, and generates all the compatible target tags by generating combinations of the tree. If you provide `--platform`, you _must_ either run with `--no-deps` or `--only-binary :all:`. And as far as I can tell, pip's `--platform` does not affect _resolution_ (i.e., it does not change the markers; only the wheel tags).

So our `--python-platform` argument is slightly different. It's "higher-level", in that you describe a platform and then we generate the relevant wheel tags and markers. And we ship with fewer limitations, but with the risk that users may misuse the flag and create broken environments.


---

_Merged by @charliermarsh on 2024-04-22 23:31_

---

_Closed by @charliermarsh on 2024-04-22 23:31_

---

_Branch deleted on 2024-04-22 23:31_

---

_Review comment by @danielhollas on `crates/uv/src/cli.rs`:866 on 2024-04-23 01:36_

This last sentence is confusing given the preceding paragraph, perhaps you meant

```suggestion
    /// platform. This option is not intended for cross-compilation and other advanced use cases.
```
?

---

_@danielhollas reviewed on 2024-04-23 01:36_

---

_@charliermarsh reviewed on 2024-04-23 01:45_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:866 on 2024-04-23 01:45_

I think "cross-compilation" is the wrong term. It _is_ intended for advanced use-cases, but "cross-compilation" emphasizes building from source in the wrong way.

---

_@danielhollas reviewed on 2024-04-23 01:49_

---

_Review comment by @danielhollas on `crates/uv/src/cli.rs`:866 on 2024-04-23 01:49_

Ah, okay. Yeah, I was definitely also mislead by the preceding sentence that specifically talks about building from source.

---
