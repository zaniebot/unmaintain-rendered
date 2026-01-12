```yaml
number: 3225
title: Combine dependency clauses with the same root
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: combine-clauses
created_at: 2024-04-23T20:37:52Z
updated_at: 2024-04-26T22:07:43Z
url: https://github.com/astral-sh/uv/pull/3225
synced_at: 2026-01-12T16:05:30Z
```

# Combine dependency clauses with the same root

---

_@ibraheemdev_

## Summary

Simplifies dependency errors of the form `you require package-a and you require package-b` to `you require package-a and package-b`. Resolves https://github.com/astral-sh/uv/issues/1009.

---

_@ibraheemdev reviewed on 2024-04-23 20:39_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_install_scenarios.rs`:369 on 2024-04-23 20:39_

This one I'm a little unsure about, but I think it's still clearer.

---

_Comment by @ibraheemdev on 2024-04-23 20:45_

@zanieb you mentioned that we might want to combine clauses within PubGrub itself. Is there a chance for false reporting with a transformation like this during formatting or does modifying PubGrub just allow for more types of simplification?

---

_Review requested from @zanieb by @zanieb on 2024-04-23 22:15_

---

_@zanieb reviewed on 2024-04-23 22:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:369 on 2024-04-23 22:37_

Hm interesting thanks for flagging. It does seem like an improvement, I think.

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:327 on 2024-04-23 22:42_

I think we should able to do this for packages other than the root as well.

---

_@zanieb reviewed on 2024-04-23 22:42_

---

_Comment by @zanieb on 2024-04-23 22:52_

Are you referring to https://github.com/astral-sh/uv/pull/1673#issuecomment-1951578438 or something else?

This looks like a really reasonable step. In contrast to #1673, this implementation seems quite safe. I don't think this could false-report. 

I believe the alternative to handling this in the formatter is to derive a new tree with some collapsed nodes. We could look into implementing this in the default PubGrub formatter as well to improve the error messages it displays. I'm not sure we could collapse the nodes in PubGrub's resolver — that's more like https://github.com/astral-sh/uv/issues/1901 which requires https://github.com/pubgrub-rs/pubgrub/pull/208.

We may also be able to use this pattern this for other incompatibility types e.g. instead of "because foo is unavailable and bar is unavailable" we could say "because foo and bar are unavailable".

---

_@zanieb reviewed on 2024-04-23 22:53_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:327 on 2024-04-23 22:53_

I'm not sure if we have scenarios covering other cases though?

---

_Comment by @zanieb on 2024-04-23 22:54_

Nice work :)

---

_@ibraheemdev reviewed on 2024-04-24 14:14_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_install_scenarios.rs`:369 on 2024-04-24 14:14_

Just noticed the extra whitespace.

---

_@ibraheemdev reviewed on 2024-04-24 14:16_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_install_scenarios.rs`:369 on 2024-04-24 14:16_

I wonder if `you require package-a but also package-b` would be better in general.

---

_@zanieb reviewed on 2024-04-24 14:19_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:369 on 2024-04-24 14:19_

I'm not sure that generalizes well for the "proof" that's being stated.

---

_@ibraheemdev reviewed on 2024-04-24 14:55_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/pubgrub/report.rs`:327 on 2024-04-24 14:55_

Created a scenario for that here: https://github.com/astral-sh/packse/pull/179.

---

_@konstin approved on 2024-04-24 15:19_

---

_Review requested from @zanieb by @ibraheemdev on 2024-04-24 16:09_

---

_@zanieb approved on 2024-04-24 16:29_

Sweet! Might be worth opening some issues for those follow-ups I mentioned so we don't forget about them.

---

_Merged by @ibraheemdev on 2024-04-24 16:34_

---

_Closed by @ibraheemdev on 2024-04-24 16:34_

---

_Comment by @Eh2406 on 2024-04-26 22:07_

Thank you so much for the progress here. Even this targeted fix is probably worth up streaming in pubgrub at some point. 

---
