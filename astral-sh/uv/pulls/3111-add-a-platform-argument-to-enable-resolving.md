```yaml
number: 3111
title: "Add a `--platform` argument to enable resolving against a target platform"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/target
created_at: 2024-04-18T03:02:48Z
updated_at: 2024-04-24T08:18:43Z
url: https://github.com/astral-sh/uv/pull/3111
synced_at: 2026-01-12T16:05:26Z
```

# Add a `--platform` argument to enable resolving against a target platform

---

_@charliermarsh_

## Summary

I've wanted to try this for a long time, so decided to give it a shot. The basic idea is that you can provide a target triple (e.g., `--platform x86_64-pc-windows-msvc`) and resolve against that platform, rather than the currently-running platform. It's functionally similar to `--python-version`, though a bit simpler since there's no need to engage with `Requires-Python`.

Our infrastructure is well-setup for this and so, in the end, it's actually pretty straightforward: for each triple, we just need to override the markers and platform tags.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-18 03:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-18 03:04_

---

_Review requested from @konstin by @charliermarsh on 2024-04-18 03:04_

---

_Marked ready for review by @charliermarsh on 2024-04-18 03:04_

---

_@charliermarsh reviewed on 2024-04-18 03:04_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:45 on 2024-04-18 03:04_

I don't know that this is actually the representation we want to use, but it... kind of works well? And it lets us avoid inventing our own format.

---

_@charliermarsh reviewed on 2024-04-18 03:05_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-18 03:05_

All of these values are based on the Tier 1 and Tier 2 definitions in the Rust project.

---

_Label `enhancement` added by @charliermarsh on 2024-04-18 03:07_

---

_@charliermarsh reviewed on 2024-04-18 03:12_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:138 on 2024-04-18 03:12_

Per the spec, you're allowed to leave these empty to indicate "unknown".

It looks like this is sometimes used though: https://github.com/search?type=code&q=%22platform_release%22+path%3Arequirements.txt. E.g., sometimes as: `sys_platform == "win32" and platform_release == "10"`.


---

_@charliermarsh reviewed on 2024-04-18 03:13_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:125 on 2024-04-18 03:13_


This really doesn't seem to be used: https://github.com/search?type=code&q=%22platform_version%22+path%3Arequirements.txt.

On my machine:

```python
>>> import platform
>>> platform.version()
'Darwin Kernel Version 23.4.0: Fri Mar 15 00:12:37 PDT 2024; root:xnu-10063.101.17~1/RELEASE_ARM64_T6031'
```

The one common case (guessing it's cargo culted) is using `'ARM' in platform_version # Mac M-chips` vs. `'ARM' not in platform_version # Mac Intel chips`.


---

_Comment by @zanieb on 2024-04-18 04:51_

Looks weirdly easy. I think this makes sense, will leave approval to one of the other too though.

---

_Comment by @charliermarsh on 2024-04-18 04:53_

I think it actually is easy. Though there are probably some decisions around whether we want to allow users to provide more specific markers (like specific manylinux versions or whatever).

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:491 on 2024-04-18 09:23_

Can we link the list of target triples here?

---

_Review comment by @konstin on `crates/uv/src/target.rs`:56 on 2024-04-18 09:25_

This is also manylinux2014, so it should match what is often present

---

_Review comment by @konstin on `crates/uv/src/target.rs`:125 on 2024-04-18 09:27_

`platform_version` leaks so much detail of the current platform i wish we could remove it entirely, or agree to always set it to an empty string across tools.

---

_@konstin approved on 2024-04-18 09:29_

nice work!

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7893 on 2024-04-18 13:45_

It looks like the test here doesn't change based on the platform? Or maybe my eyes missed it. Can we add a more "exciting" test where there is a change based on the platform?

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:491 on 2024-04-18 13:48_

I think it would also be nice to elaborate on what is meant by "target triple" here. Coming from Rust/C/C++/whatever, the notion of the target triple makes intuitive sense. But in Python land, I might know that the things that make up a target are more involved than a standard target triple. I think calling that difference out in the docs will help understanding. And as a concrete example, it might help to answer the question of, "what if I want to specify a different Python implementation?" That isn't captured by the target triple. (And I think the suggestion in that case is to actually use the different Python implementation.)

Maybe a succinct way to explain things here is that the target triples are a heuristic to specify a _part_ of the build parameters, but that they don't capture everything that can be in a "marker environment."

---

_@BurntSushi approved on 2024-04-18 13:48_

w00t! This is awesome.

---

_@charliermarsh reviewed on 2024-04-18 13:52_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:491 on 2024-04-18 13:52_

Do you think "target triple" is the right abstraction to use?

---

_@charliermarsh reviewed on 2024-04-18 14:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:7893 on 2024-04-18 14:56_

It does, colorama gets included / excluded!

---

_@charliermarsh reviewed on 2024-04-18 14:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:7893 on 2024-04-18 14:56_

I'll add some more though. Do you have good examples from your work?

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:491 on 2024-04-18 15:06_

I'm honestly not sure. I am skeptical. With that said, it _does_ seem very useful. I feel like if we present it as a "shortcut" to specifying the most common parts of a marker environment that you might to specify, then it would be useful. (And we can say that there isn't yet a way to specify more parts of the marker environment than what the target triple does.)

I think the weird part of this is that the marker environment derived from the target triple is sort of a frankenstein monster. It's part static information fully determined by the triple and part dynamic information based on the current environment. I don't have enough experience to say how that's going to fall over, but it doesn't seem right to me. For example, target files for rustc don't have any dynamic information in them at all: https://github.com/rust-lang/rust/blob/eff958c59e8c07ba0515e164b825c9001b242294/tests/run-make/target-specs/my-x86_64-unknown-linux-gnu-platform.json

So I wonder if it makes sense to remove the dynamic parts here and, for example, hard-code `cpython`. And for folks that want to use pypy (or whatever), then maybe in the future we can provide a way for end users to specify the complete marker environment.

---

_@BurntSushi reviewed on 2024-04-18 15:06_

---

_@BurntSushi reviewed on 2024-04-18 15:09_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7893 on 2024-04-18 15:09_

Oh nice! I've mostly been working with `anyio`, although I think that's mostly dependent on `python_version`. Otherwise I made up my own packse scenarios.

---

_@charliermarsh reviewed on 2024-04-19 02:53_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:491 on 2024-04-19 02:53_

I'm gonna run with it for now... Let's see where it takes us.

> It's part static information fully determined by the triple and part dynamic information based on the current environment.

The only things that are dynamic are the Python-oriented tags: the Python version, the implementation version, etc. So it seems not-horrible to me.

---

_Merged by @charliermarsh on 2024-04-19 02:57_

---

_Closed by @charliermarsh on 2024-04-19 02:57_

---

_Branch deleted on 2024-04-19 02:57_

---

_@fellhorn reviewed on 2024-04-23 16:17_

---

_Review comment by @fellhorn on `crates/uv/src/target.rs`:56 on 2024-04-23 16:17_

Do you think there should be a way to override these for the user?

I just tried the `--python-platform` feature (works great, thx!) but failed with packages built against a newer version of `glibc`.

---

_@charliermarsh reviewed on 2024-04-23 16:24_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-23 16:24_

I think we should add variants for the other manylinux versions. (By "failed", you mean, "didn't download those wheels"? Or, how did it fail?)

---

_@neumann-nico reviewed on 2024-04-23 20:17_

---

_Review comment by @neumann-nico on `crates/uv/src/target.rs`:56 on 2024-04-23 20:17_

e.g. [open3d](https://pypi.org/project/open3d/#files) uses `glibc` 2.27 and it fails like this:

```bash
❯ uv pip compile requirements.in --python-platform=linux
  x No solution found when resolving dependencies:
  `-> Because open3d==0.18.0 is unusable because no wheels are available with a matching Python implementation
   and you require open3d==0.18.0, we can conclude that the requirements are unsatisfiable.
```

---

_@charliermarsh reviewed on 2024-04-23 20:21_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-23 20:21_

Thanks.

---

_@charliermarsh reviewed on 2024-04-23 20:25_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-23 20:25_

Filed here: https://github.com/astral-sh/uv/issues/3222. I have an idea for the format.

---

_@neumann-nico reviewed on 2024-04-23 20:32_

---

_Review comment by @neumann-nico on `crates/uv/src/target.rs`:56 on 2024-04-23 20:32_

For completeness:
`uv pip compile requirements.in --python-platform=macos` also fails for `open3d` because the ARM64 wheels are only available for MacOS 13.0+ (or universal for 10.15+) while `uv` specifies MacOS 11.0 for ARM64.

---

_@charliermarsh reviewed on 2024-04-23 20:35_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-23 20:35_

In that case, should it be able to pick out the universal wheels? Wonder why it's not.

---

_@neumann-nico reviewed on 2024-04-23 21:07_

---

_Review comment by @neumann-nico on `crates/uv/src/target.rs`:56 on 2024-04-23 21:07_

Hmm good question, I think so?, because it would match the platform.


---

_@charliermarsh reviewed on 2024-04-23 22:20_

---

_Review comment by @charliermarsh on `crates/uv/src/target.rs`:56 on 2024-04-23 22:20_

I'll look into that separately.

---

_@fellhorn reviewed on 2024-04-24 08:18_

---

_Review comment by @fellhorn on `crates/uv/src/target.rs`:56 on 2024-04-24 08:18_

Yes, with `failed` I meant [PEP 600](https://peps.python.org/pep-0600/) `manylinux_x_y` packages not being selected during `uv pip compile` due to a glibc version mismatch. Of course this behavior is expected, though an 11 year old version does effectively create constraints.

For us a few more allowed options for the `python-platform` string would probably already work, thx :+1: 

---
