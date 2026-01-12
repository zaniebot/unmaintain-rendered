```yaml
number: 14141
title: "handle an existing shebang in `uv init --script`"
type: pull_request
state: merged
author: oconnor663
labels:
  - bug
assignees: []
merged: true
base: main
head: jack/script_init_shebang
created_at: 2025-06-18T22:24:21Z
updated_at: 2025-06-19T21:47:24Z
url: https://github.com/astral-sh/uv/pull/14141
synced_at: 2026-01-12T16:11:03Z
```

# handle an existing shebang in `uv init --script`

---

_@oconnor663_

Closes https://github.com/astral-sh/uv/issues/14085.

---

_Review requested from @zanieb by @oconnor663 on 2025-06-18 22:24_

---

_Review comment by @oconnor663 on `crates/uv-warnings/src/lib.rs`:27 on 2025-06-18 22:26_

Without the double curlies here, putting two `warn_user!` invocations back-to-back doesn't compile. My instinct was that I wanted the "warning:" prefix on both lines above, but whether or not we stick with that, this version of these macros is probably more correct.

---

_@oconnor663 reviewed on 2025-06-18 22:26_

---

_@zanieb reviewed on 2025-06-18 22:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:949 on 2025-06-18 22:28_

As briefly mentioned above, you should consolidate these into a single multiline message instead of showing "two" warnings.

---

_Review comment by @zanieb on `crates/uv-scripts/src/lib.rs`:245 on 2025-06-18 22:29_

A comment here seems valuable.

---

_@zanieb reviewed on 2025-06-18 22:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:978 on 2025-06-18 22:30_

Hm, I thought the result should have uv in it?

---

_@zanieb reviewed on 2025-06-18 22:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:939 on 2025-06-18 22:30_

Is this whitespace intentional? I hadn't seen that before
```suggestion
    let contents = "#!/usr/bin/env python3\nprint(\"Hello, world!\")";
```

---

_@zanieb reviewed on 2025-06-18 22:30_

---

_@zanieb reviewed on 2025-06-18 22:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:975 on 2025-06-18 22:31_

A little manual indentation can go a long way here, since fmt can't handle the macro

---

_@oconnor663 reviewed on 2025-06-18 22:31_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:939 on 2025-06-18 22:31_

I've always been in the habit, and apparently it's always been supported: https://unix.stackexchange.com/a/276845

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:938 on 2025-06-18 22:32_

What's the child directory doing for us here?

---

_@zanieb reviewed on 2025-06-18 22:32_

---

_@oconnor663 reviewed on 2025-06-18 22:32_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:978 on 2025-06-18 22:32_

~~Oh I was never intending to _change_ the original shebang. My intention here was just not to trash it. I wonder if it would be too opinionated to change it to the one suggested in the warning / [our docs](https://docs.astral.sh/uv/guides/scripts/#using-a-shebang-to-create-an-executable-file).~~

Wait did I make a copy-paste mistake here? Hold that thought.

---

_@zanieb reviewed on 2025-06-18 22:33_

---

_Review comment by @zanieb on `crates/uv-scripts/src/lib.rs`:245 on 2025-06-18 22:33_

We might want to list a TODO, like providing a nice warning if your uv invocation isn't our recommended shebang.

---

_@oconnor663 reviewed on 2025-06-18 22:33_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:938 on 2025-06-18 22:33_

I definitely copy-pasted this from another test without thinking about it :sweat_smile: 

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:979 on 2025-06-18 22:34_

I'm not sure I love the newline without a leading comment character, I might include a `#`?

---

_@zanieb reviewed on 2025-06-18 22:34_

---

_@zanieb reviewed on 2025-06-18 22:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:939 on 2025-06-18 22:36_

(I find it perturbing, but.. if that's what you like :D)

---

_@zanieb reviewed on 2025-06-18 22:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:978 on 2025-06-18 22:36_

This comment is for the snapshot where the initial contents were

`let contents = "#!/usr/bin/env -S uv run --script\nprint(\"Hello, world!\")";`

---

_Label `bug` added by @zanieb on 2025-06-18 22:37_

---

_@oconnor663 reviewed on 2025-06-18 22:42_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:978 on 2025-06-18 22:42_

Ah I think I had this correct in my working copy but neglected to amend the commit before I pushed my branch. Will fix in just a sec.

---

_@oconnor663 reviewed on 2025-06-18 22:45_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:939 on 2025-06-18 22:45_

![image](https://github.com/user-attachments/assets/5b179723-c39a-4a25-944a-bf25517aac10)

---

_@oconnor663 reviewed on 2025-06-18 23:21_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/init.rs`:975 on 2025-06-18 23:21_

We chatted and decided not to worry too much about test macro formatting here.

---

_@zanieb reviewed on 2025-06-18 23:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:975 on 2025-06-18 23:31_

Yeah I'm a fool.

---

_@zanieb approved on 2025-06-18 23:31_

---

_Merged by @oconnor663 on 2025-06-19 21:47_

---

_Closed by @oconnor663 on 2025-06-19 21:47_

---

_Branch deleted on 2025-06-19 21:47_

---
