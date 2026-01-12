```yaml
number: 11232
title: Conflicting groups should handle conflicting inclusions automatically
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-02-05T04:18:57Z
updated_at: 2025-03-08T18:21:26Z
url: https://github.com/astral-sh/uv/issues/11232
synced_at: 2026-01-12T16:00:31Z
```

# Conflicting groups should handle conflicting inclusions automatically

---

_@zanieb_

### Summary

When using the `include-group` directive in dependency groups, uv does not propagate dependency group conflicts automatically. Instead, the conflicts must be manually enumerated. uv should be able to do this? Either by retaining the provenance of the included dependencies, or by just filling in more conflicts for the user before forwarding to the resolver.

### Example

The following is required

```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[dependency-groups]
dev = [
  { include-group = "test" },
]
test = ["tox>4"]
magic = ["tox<4"]

[tool.uv]
conflicts = [
  [
    { group = "test" },
    { group = "magic" },
  ],
  [
    { group = "dev" },
    { group = "magic" },
  ], 
]
```

But I would like to just do

```toml
[tool.uv]
conflicts = [
  [
    { group = "test" },
    { group = "magic" },
  ],
]
```

which fails with

```
❯ uv lock
  × No solution found when resolving dependencies:
  ╰─▶ Because example:magic depends on tox<4 and example:dev depends on tox>4, we can conclude that example:dev and
      example:magic are incompatible.
      And because your project requires example:dev and example:magic, we can conclude that your project's requirements
      are unsatisfiable.
```

While this example is contrived, you can imagine more complicated examples with multiple nested group inclusions and conflicts.

---

_Label `enhancement` added by @zanieb on 2025-02-05 04:18_

---

_Comment by @zanieb on 2025-02-05 04:19_

cc @BurntSushi just curious for your take on feasibility here.

Does something similar apply to extras since those can reference other extras too? I'd need to check.

---

_Comment by @gaborbernat on 2025-02-05 04:20_

Here's the real world example where this came up https://github.com/gaborbernat/datamodel-code-generator/commit/b654b43e7f5d84d78788b60b4976ce57ba9f50e1, `dev` needs to be included otherwise things fail, even though only `pkg-meta` conflicts....

---

_Comment by @BurntSushi on 2025-02-05 12:18_

> or by just filling in more conflicts for the user before forwarding to the resolver.

I think this implementation path would be pretty feasible. I think this is ultimately what you would have to do anyway. As in, I don't see how the resolver could do anything better per se given knowledge that one group references another.

As for extras, I'm actually not sure if you can do this? Is there syntax for referring to a sibling extra in the same package?

Overall I think the main concern I'd have here is that this could make it very easy for the number of conflicts to grow large. And that you could have a number of conflicts that isn't proportional to the actual written down declared conflicts. And this matters because increasing the number of conflicts means increasing the number of resolver forks, which can slow down resolution. But this is a very vague hand-wavy concern, and I don't think it's enough of a concern to out-weigh the benefits here.

---

_Comment by @zanieb on 2025-02-05 15:10_

> As for extras, I'm actually not sure if you can do this? Is there syntax for referring to a sibling extra in the same package?

Of course! People frequently use this to make an `all` group.

```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
dev = ["example[test]"]
test = ["tox>4"]
magic = ["tox<4"]

[tool.uv]
conflicts = [
  [
    { extra = "test" },
    { extra = "magic" },
  ],
]
```

which fails with

```
❯ uv lock
  × No solution found when resolving dependencies:
  ╰─▶ Because example[magic] depends on tox<4 and example[dev] depends on tox>4, we can conclude that example[dev] and
      example[magic] are incompatible.
      And because your project requires example[dev] and example[magic], we can conclude that your project's requirements
      are unsatisfiable.
```

> And that you could have a number of conflicts that isn't proportional to the actual written down declared conflicts. And this matters because increasing the number of conflicts means increasing the number of resolver fork

If the declared conflict is correct, then the transitive inferred conflict is strictly necessary for them to perform a successful resolution at all.

---

_Comment by @BurntSushi on 2025-02-05 15:18_

Derp, I tried that, but I had a typo.

So yeah, it probably makes sense to do this for extras as well. I think it only requires a shallow traversal of optional dependencies here? As in, we don't need the full dependency tree to setup the conflicts. I think that would be harder.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-03-07 09:36_

---

_Closed by @jtfmumm on 2025-03-08 18:21_

---

_Closed by @jtfmumm on 2025-03-08 18:21_

---
