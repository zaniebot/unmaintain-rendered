```yaml
number: 1673
title: combine dependency clauses with same root
type: pull_request
state: closed
author: dwreeves
labels:
  - error messages
assignees: []
base: main
head: 1009-combine-dependency-clauses-with-same-root
created_at: 2024-02-19T01:23:31Z
updated_at: 2024-02-19T02:24:34Z
url: https://github.com/astral-sh/uv/pull/1673
synced_at: 2026-01-12T16:04:41Z
```

# combine dependency clauses with same root

---

_@dwreeves_

Resolves #1009

## Summary

Combines clauses with the same root.

- **Before:** And because you require albatross and you require bluebird
- **After:** And because you require both albatross and bluebird
- **Difference:** And because you require **both** albatross and ~you require~ bluebird

I'm still a Rust beginner, please let me know if this is not an appropriate implementation of this requirement and I'll try to adjust it.

## Test Plan

`cargo nextest run`


---

_Assigned to @zanieb by @zanieb on 2024-02-19 02:04_

---

_Comment by @zanieb on 2024-02-19 02:15_

Hi! Thanks for taking this on — it's actually a pretty big task.

In this pull request, it looks like you're taking the string representations and combining them if they both start with the "you require" root. I haven't looked into solving this issue yet, but my intuition is that we'll want to take a more robust approach. For example, we may want to "walk" the derivation tree and transform it into a new tree with combined clauses (you can see this in concept in action in PubGrub's [`collapse_no_versions`](https://github.com/pubgrub-rs/pubgrub/blob/717289be5722dd5caaa0d1f4ed13047d11a7f7fd/src/report.rs#L75-L101)). If we do this _before_ formatting, we can check if clauses are the same kind of [incompatibility](https://github.com/pubgrub-rs/pubgrub/blob/717289be5722dd5caaa0d1f4ed13047d11a7f7fd/src/report.rs#L38-L47) which would let us collapse more things in a type-safe manner rather than relying on string comparisons. This might require significant changes to the design of the report formatter or our own representation of the derivation tree. Does that make sense? It's kind of open-ended still and there's going to be a significant amount of design involved in solving this issue. I was imagining tackling this in the future as part of a larger effort to make the error message reporting system more powerful.

Note: I've linked to the upstream PubGrub repository above but our [fork](https://github.com/zanieb/pubgrub) has diverged a bit and is the relevant repository if you need to make PubGrub changes.

---

_Label `error messages` added by @zanieb on 2024-02-19 02:15_

---

_Comment by @dwreeves on 2024-02-19 02:20_

That makes sense, and yes the string modification did not feel very robust. I did check to see if there was a relevant abstraction in the implementation and did not see one and did see that PubGrub was not being actively maintained; however I did not realize you were maintaining a fork. I will look into it from that perspective. Thanks! I will close this issue.

---

_Closed by @dwreeves on 2024-02-19 02:20_

---

_Comment by @zanieb on 2024-02-19 02:21_

PubGrub is being maintained it's just on a [dev](https://github.com/pubgrub-rs/pubgrub/tree/dev) branch.

Happy to chat more in the issue about possible designs!

---

_Comment by @dwreeves on 2024-02-19 02:24_

Ah! 💡

@zanieb Thanks for being so helpful, it's really appreciated. I need to look more into PubGrub first but I think the idea of enuming issue types and combining that way makes perfect sense.

---
