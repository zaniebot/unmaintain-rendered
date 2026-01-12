```yaml
number: 1109
title: Encode prefixes in README headings not just in TOC
type: pull_request
state: merged
author: phillipuniverse
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2022-12-06T16:11:24Z
updated_at: 2022-12-07T14:35:40Z
url: https://github.com/astral-sh/ruff/pull/1109
synced_at: 2026-01-12T15:55:05Z
```

# Encode prefixes in README headings not just in TOC

---

_@phillipuniverse_

I know this is not quite complete as I just realized the categories are auto-generated from `generate_rules_tables.rs`. Before I went and changed the generator to encode the prefix in the title, I wanted to see if this is something we wanted to do.

Here's why I want it - I want to be able to link to a particular ruleset and that link match up to the prefix. I keep losing track of the prefix categories themselves and always forget to look up in the TOC. It's sliiightly more obtuse to divine the categories from the individual rules themselves.

---

_Converted to draft by @phillipuniverse on 2022-12-06 16:11_

---

_Comment by @charliermarsh on 2022-12-06 16:26_

Yeah this looks useful to me -- happy to accept a change to the generator, if you're up for it?

---

_Comment by @phillipuniverse on 2022-12-06 16:27_

Yup no problem!

---

_Review comment by @charliermarsh on `README.md`:96 on 2022-12-07 03:52_

We definitely need to auto-generate this too, it keeps getting stale :)

(Not asking you to do that, or for it to be part of this PR, just an observation!)

---

_@charliermarsh reviewed on 2022-12-07 03:52_

---

_Comment by @phillipuniverse on 2022-12-07 03:59_

@charliermarsh you're fast!!

I like where things landed here personally, but this is my first usage of Rust! Would love additional feedback on my ruStAceAN-ese if you have it!

![image](https://user-images.githubusercontent.com/684275/206084911-074a0def-fc75-47b3-9895-f1d42cc6b790.png)

---

_Marked ready for review by @phillipuniverse on 2022-12-07 03:59_

---

_@phillipuniverse reviewed on 2022-12-07 04:00_

---

_Review comment by @phillipuniverse on `README.md`:96 on 2022-12-07 04:00_

No doubt!!

I can do that in this PR or split, your call. Rust is kind of fun!!

---

_Review requested from @charliermarsh by @phillipuniverse on 2022-12-07 04:00_

---

_@charliermarsh reviewed on 2022-12-07 04:14_

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:10 on 2022-12-07 04:14_

This file is actually auto-generated. Can you add `Display` to the list of derives in `generate_check_code_prefix.rs`? You're looking for this code block:

```rs
// Create the `CheckCodePrefix` definition.
let mut gen = scope
    .new_enum("CheckCodePrefix")
    ...
```

---

_@charliermarsh reviewed on 2022-12-07 04:19_

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:10 on 2022-12-07 04:19_

Actually, instead of `Display`, can we use `AsRefStr` from `strum_macros`? You'll see that `CheckCode` derives that.


---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:31 on 2022-12-07 04:19_

And then, if you use `AsRefStr`, this line will become:

```
check_category.codes().iter().map(std::convert::AsRef::as_ref).join(", ")
```

---

_@charliermarsh reviewed on 2022-12-07 04:19_

---

_@JonathanPlasse reviewed on 2022-12-07 04:20_

---

_Review comment by @JonathanPlasse on `README.md`:75 on 2022-12-07 04:20_

There is `I25`, so `I` should be `I00`

---

_@charliermarsh reviewed on 2022-12-07 04:20_

---

_Review comment by @charliermarsh on `README.md`:96 on 2022-12-07 04:20_

Awesome! Separate PR would be best if you're up for it. (It can probably be part of `generate-rules-table` -- you can follow the logic there for how we find-and-replace sections within the README using these `BEGIN_PRAGMA` and `END_PRAGMA` constructs.)

---

_@charliermarsh reviewed on 2022-12-07 04:21_

---

_Review comment by @charliermarsh on `README.md`:75 on 2022-12-07 04:21_

(I want to rename `I25` to, like, `TID250` or something.)

---

_Review comment by @phillipuniverse on `README.md`:75 on 2022-12-07 12:40_

Done

---

_@phillipuniverse reviewed on 2022-12-07 12:40_

---

_@phillipuniverse reviewed on 2022-12-07 12:41_

---

_Review comment by @phillipuniverse on `src/checks_gen.rs`:10 on 2022-12-07 12:41_

Done. Just for my own edification, why is `AsRefStr` better? Or just consistency with `CheckCode`?

---

_@phillipuniverse reviewed on 2022-12-07 12:42_

---

_Review comment by @phillipuniverse on `ruff_dev/src/generate_check_code_prefix.rs`:174 on 2022-12-07 12:42_

Re-ordered to be formatter-compatible by default.

---

_Review requested from @charliermarsh by @phillipuniverse on 2022-12-07 12:42_

---

_Review request for @charliermarsh removed by @phillipuniverse on 2022-12-07 12:42_

---

_Review requested from @JonathanPlasse by @phillipuniverse on 2022-12-07 12:42_

---

_Review request for @JonathanPlasse removed by @phillipuniverse on 2022-12-07 12:42_

---

_Review requested from @JonathanPlasse by @phillipuniverse on 2022-12-07 12:42_

---

_Review request for @JonathanPlasse removed by @phillipuniverse on 2022-12-07 12:42_

---

_Review requested from @charliermarsh by @phillipuniverse on 2022-12-07 12:42_

---

_Review request for @charliermarsh removed by @phillipuniverse on 2022-12-07 12:42_

---

_Review requested from @JonathanPlasse by @phillipuniverse on 2022-12-07 12:42_

---

_Review request for @JonathanPlasse removed by @phillipuniverse on 2022-12-07 12:43_

---

_Review requested from @charliermarsh by @phillipuniverse on 2022-12-07 12:43_

---

_@JonathanPlasse reviewed on 2022-12-07 12:46_

---

_Review comment by @JonathanPlasse on `README.md`:75 on 2022-12-07 12:46_

So `I00`, can be `I` as it would have no conflict with other rules.

---

_@phillipuniverse reviewed on 2022-12-07 12:54_

---

_Review comment by @phillipuniverse on `README.md`:75 on 2022-12-07 12:54_

@JonathanPlasse ok, so change it back to `I`? It currently does have a conflict with `I25` (tidy imports), the `TID250` change hasn't happened yet

---

_@charliermarsh reviewed on 2022-12-07 14:21_

---

_Review comment by @charliermarsh on `README.md`:75 on 2022-12-07 14:21_

Yeah I believe what you have here is right.

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:10 on 2022-12-07 14:22_

It's really just for consistency. I believe `AsRefStr` also avoids an allocation: https://github.com/Peternator7/strum/blob/e772e203dabd58630730c1520a2e2110eaf13fc2/strum_macros/src/lib.rs#L125.

---

_@charliermarsh reviewed on 2022-12-07 14:22_

---

_Merged by @charliermarsh on 2022-12-07 14:24_

---

_Closed by @charliermarsh on 2022-12-07 14:24_

---

_Comment by @charliermarsh on 2022-12-07 14:24_

Thanks for your contribution! Glad to have you involved!

---

_Branch deleted on 2022-12-07 14:28_

---

_@phillipuniverse reviewed on 2022-12-07 14:35_

---

_Review comment by @phillipuniverse on `README.md`:96 on 2022-12-07 14:35_

TOC handled in #1121

---
