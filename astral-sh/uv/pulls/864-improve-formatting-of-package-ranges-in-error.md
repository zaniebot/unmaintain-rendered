```yaml
number: 864
title: Improve formatting of package ranges in error messages
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/clause-fmt
created_at: 2024-01-10T00:20:03Z
updated_at: 2024-01-11T09:04:07Z
url: https://github.com/astral-sh/uv/pull/864
synced_at: 2026-01-12T16:04:14Z
```

# Improve formatting of package ranges in error messages

---

_@zanieb_

Closes #810
Closes https://github.com/astral-sh/puffin/issues/812
Requires https://github.com/zanieb/pubgrub/pull/19 and https://github.com/zanieb/pubgrub/pull/18

- Always pair package ranges with names e.g. `... of a matching a<1.0` instead of `... of a matching <1.0`
- Split range segments onto multiple lines when not a singleton as suggested in [#850](https://github.com/astral-sh/puffin/pull/850#discussion_r1446419610)
- Improve formatting when ranges are split across multiple lines e.g. by avoiding extra spaces and improving wording

Note review will require expanding the hidden files as there are significant changes to the report formatter and snapshots.

Bear with me here as these are definitely not perfect still.

The following changes build on top of this independently for further improvements:
- #868 
- #867 
- #866 
- #871 

---

_@zanieb reviewed on 2024-01-10 00:30_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:182 on 2024-01-10 00:30_

I'm not enthused about this pattern, but I'm not sure what else to do since all of these may or may not split onto multiple lines.

---

_@zanieb reviewed on 2024-01-10 00:30_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:194 on 2024-01-10 00:30_

From PubGrub's implementation... should remove.

---

_@zanieb reviewed on 2024-01-10 00:53_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:445 on 2024-01-10 00:53_

It's not clear where this additional newline is coming from, but it's not good.

---

_@zanieb reviewed on 2024-01-10 01:18_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:182 on 2024-01-10 01:18_

It might be better to perform the splitting outside of `PackageRange` instead

---

_Marked ready for review by @zanieb on 2024-01-10 05:18_

---

_Label `error messages` added by @zanieb on 2024-01-10 15:50_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 16:54_

I think I like the space after the `,`. :zipper_mouth_face: 

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/report.rs`:167 on 2024-01-10 17:03_

I was trying to figure out what this was doing based on how it was used, but I couldn't find any uses of it. Are they used in another PR?

---

_@BurntSushi approved on 2024-01-10 17:03_

I think the error messages here are greatly improved!

---

_@zanieb reviewed on 2024-01-10 17:05_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 17:05_

Happy to restore that — although it's not common in requirements files. We could also use ` and ` or ` && `

---

_@zanieb reviewed on 2024-01-10 17:06_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:167 on 2024-01-10 17:06_

See https://github.com/zanieb/pubgrub/pull/19 this is an internal method of the PubGrub error reporter pulled out into a trait.

---

_@zanieb reviewed on 2024-01-10 17:12_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 17:12_

Although I think we'd only want it in the multiline display, it'd be different when it's just a single segment.

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 17:13_

e.g. https://github.com/astral-sh/puffin/pull/870

---

_@zanieb reviewed on 2024-01-10 17:13_

---

_@BurntSushi reviewed on 2024-01-10 17:16_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 17:16_

Yeah I confess I do like just `and` or `&&`. I think `,` is common in math circles for short-hand for "and," but I don't think it has such an obvious meaning outside of that.

---

_@BurntSushi reviewed on 2024-01-10 17:18_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/report.rs`:167 on 2024-01-10 17:18_

Hmmm okay I see. I think it's my unfamiliarity with pubgrub that is biting me here. But makes sense at a high level!

---

_@zanieb reviewed on 2024-01-10 17:23_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-10 17:23_

https://github.com/astral-sh/puffin/pull/871

---

_@zanieb reviewed on 2024-01-10 17:24_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:167 on 2024-01-10 17:24_

Yeah I think eventually we might want to rewrite the whole reporter, it's kind of awkward as-is.

---

_Merged by @zanieb on 2024-01-10 20:16_

---

_Closed by @zanieb on 2024-01-10 20:16_

---

_Branch deleted on 2024-01-10 20:16_

---

_@konstin reviewed on 2024-01-11 09:04_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_install_scenarios.rs`:161 on 2024-01-11 09:04_

I agree in general, but for python specifically the comma comes from PEP 440 / PEP 508 so users will have e.g. written `a>1.0.0,<2.0.0` in their own requirements and tend to be familiar with that syntax, while `and` would introduce and additional format for the same data

---
