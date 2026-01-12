```yaml
number: 4601
title: Allow configuring the toolchain fetch strategy
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-fetch-strategy
created_at: 2024-06-27T20:03:32Z
updated_at: 2024-07-02T01:54:26Z
url: https://github.com/astral-sh/uv/pull/4601
synced_at: 2026-01-12T16:06:20Z
```

# Allow configuring the toolchain fetch strategy

---

_@zanieb_

Adds a `toolchain-fetch` option alongside `toolchain-preference` with `automatic` (default) and `manual` values allowing automatic toolchain fetches to be disabled (replaces https://github.com/astral-sh/uv/pull/4425). When `manual`, toolchains must be installed with `uv toolchain install`.

Note this was previously implemented with `if-necessary`, `always`, `never` variants but the interaction between this and `toolchain-preference` was too confusing. By reducing to a binary option, things should be clearer. The `if-necessary` behavior moved to `toolchain-preference=installed`. See https://github.com/astral-sh/uv/pull/4601#discussion_r1657839633 and https://github.com/astral-sh/uv/pull/4601#discussion_r1658658755

---

_Label `configuration` added by @zanieb on 2024-06-27 20:03_

---

_Label `preview` added by @zanieb on 2024-06-27 20:03_

---

_Marked ready for review by @zanieb on 2024-06-27 21:32_

---

_@zanieb reviewed on 2024-06-27 21:33_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:86 on 2024-06-27 21:33_

I worry a little bit about the interaction between these two options being confusing, but I think it'd be worse to try to enumerate in a single setting.

---

_@zanieb reviewed on 2024-06-27 21:34_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:128 on 2024-06-27 21:34_

Should we... hide these? I feel like they're targeting the configuration file more than the CLI

---

_Comment by @zanieb on 2024-06-27 21:34_

I'm still not sure what our testing strategy is for toolchains since I don't want to fetch them in unit tests.

I tested various combinations of `--toolchain-preference` and `--toolchain-fetch` locally.

---

_Review requested from @BurntSushi by @zanieb on 2024-06-27 21:34_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-27 21:34_

---

_Review comment by @konstin on `crates/uv-toolchain/src/discovery.rs`:82 on 2024-06-28 11:43_

```suggestion
    /// Always fetch a managed toolchain if it can be used to fulfill a request.
```

---

_@konstin approved on 2024-06-28 11:48_

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:128 on 2024-06-28 12:18_

Is there an interaction between `--toolchain-preference` and `--toolchain-fetch` that should be documented here?

I like having the flags available on the CLI personally. Unless there's a specific downside of using them on the CLI.

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:128 on 2024-06-28 13:01_

Ah I see the interactions are documented on the options. Do those show up in `--help`?

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:86 on 2024-06-28 13:03_

Yeah hmmm. Would it help to tie them back to a specific user experience? Like, "You might want to use `only-system` because <blah>, and so setting this to `always` has no effect." Honestly not sure though.

---

_@BurntSushi approved on 2024-06-28 13:03_

---

_@zanieb reviewed on 2024-06-28 14:34_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:128 on 2024-06-28 14:34_

Yeah they show up in `--help` for every command since they're global options. The interactions aren't explained actually in `--help`, I guess because they're on a subsequent line. I could add some notes to the CLI flag itself though?

I don't mind having them be available in the CLI but I'm tempted to omit them from the help.

---

_@BurntSushi reviewed on 2024-06-28 14:38_

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:128 on 2024-06-28 14:38_

Aye yeah that sounds good. I think I'd still prefer to have them in the `--help` personally. Although I see how it might be a bit of a bummer to have them in _every_ help since they're global _and_ because yeah, I agree, these probably won't get used much on the CLI. So I defer to you!

---

_@zanieb reviewed on 2024-06-28 14:52_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:86 on 2024-06-28 14:52_

Idk. Maybe warnings would be sensible? Do you think they make sense as separate options?

---

_@zanieb reviewed on 2024-06-28 14:54_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:86 on 2024-06-28 14:54_

Like, an alternative is `toolchain-fetch-allowed` as a bool and `toolchain-preference` has `only-managed`, `only-system`, `managed`, `system`, `installed` (default), where `installed` prefers any installed toolchain over any not-yet-downloaded one  i.e. replacing `if-necessary`.

---

_@BurntSushi reviewed on 2024-06-28 14:56_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:86 on 2024-06-28 14:56_

When I was reading the docs, it did take me a minute to put the pieces together. But after having understood it, yeah, it does feel like they belong as two different options. They seem _almost_ orthogonal, but not quite.

I'd be in favor of a warning for things like `pref=only-system` and `strat=always` combinations that are probably a mistake.

---

_Merged by @zanieb on 2024-07-02 01:54_

---

_Closed by @zanieb on 2024-07-02 01:54_

---

_Branch deleted on 2024-07-02 01:54_

---
