```yaml
number: 1884
title: "Subheadings not rendered in `invalid-method-override` documentation"
type: issue
state: open
author: AlexWaygood
labels:
  - documentation
assignees: []
created_at: 2025-12-14T16:07:20Z
updated_at: 2025-12-15T05:23:28Z
url: https://github.com/astral-sh/ty/issues/1884
synced_at: 2026-01-12T15:54:26Z
```

# Subheadings not rendered in `invalid-method-override` documentation

---

_@AlexWaygood_

Our [docs for `invalid-method-override`](https://docs.astral.sh/ty/reference/rules/#invalid-method-override) are longer than the docs for most of our rules. I wanted to make them high-quality, because I expect that many users won't have heard of the Liskov Substitution Principle, and it's useful to have a good explanation of why it's important for us to link to.

As part of the documentation, I added a ["Common issues" section](https://github.com/astral-sh/ruff/blob/8bc753b842967b53dc2698fdff8e1a410ffafa46/crates/ty_python_semantic/src/types/diagnostic.rs#L2170-L2188), with the idea that I'd be able to link directly to it or subsections in it if users brought up a question that we'd already written up an answer to. Currently, however, the `## Common issues` heading is the same font size as the `### Why does ty complain about my `__eq__` method?` heading immediately below it. I also can't link directly to any subsections in the docs for this rule currently.

We should either find a way to fix this, or I should rewrite the docs for this rule to not use subheadings 🙃

---

_Label `documentation` added by @AlexWaygood on 2025-12-14 16:07_

---

_Comment by @dhruvmanila on 2025-12-15 05:23_

I think it might be useful to have separate pages for each rule (similar to Ruff) which would allow us to provide links to individual sections in a rule documentation. We'd need to have a design on how to present a list of rules similar to https://rust-lang.github.io/rust-clippy/master/index.html and add some filters / sorting ability based on attributes like severity, version added, etc.

---
