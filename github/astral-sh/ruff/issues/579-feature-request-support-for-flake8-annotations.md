---
number: 579
title: "Feature request: support for flake8-annotations"
type: issue
state: closed
author: tekumara
labels:
  - rule
assignees: []
created_at: 2022-11-04T09:14:59Z
updated_at: 2023-01-26T04:49:25Z
url: https://github.com/astral-sh/ruff/issues/579
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature request: support for flake8-annotations

---

_Issue opened by @tekumara on 2022-11-04 09:14_

Support for flake8-annotations would be awesome! 🙏 

---

_Label `rule` added by @charliermarsh on 2022-11-04 13:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-06 04:40_

---

_Comment by @charliermarsh on 2022-11-06 04:40_

Working on this now as it's relatively straightforward.

---

_Referenced in [astral-sh/ruff#625](../../astral-sh/ruff/pulls/625.md) on 2022-11-06 22:03_

---

_Closed by @charliermarsh on 2022-11-06 22:25_

---

_Comment by @charliermarsh on 2022-11-07 02:23_

This is going out as part of `0.0.105` (building now). Please give it a try and let me know if you see any inconsistencies!

I ended up using two error codes for public vs. private functions (instead of three codes for public vs. protected vs. secret), and the only settings I implemented were `suppress-dummy-args`, `suppress-none-returning`, and `mypy-init-return`.

Note that the settings should be provided in `pyproject.toml` like:

```toml
[tool.ruff.flake8-annotations]
suppress-dummy-args = true
```


---

_Comment by @not-my-profile on 2023-01-24 15:00_

I am curious what the use case for all of these missing-* rules from `flake8-annotations` is. These checks seem very much redundant with the checks of static type checkers, which are much more thorough and if you have type annotations, you'll want to use a static type checker like mypy or pyright alongside ruff anyway ... so I don't really see the point. Pyright in particular also supports LSP and works great alongside ruff (and it's also very efficient).

\cc @akx since you just now opened #2128 to make these checks more configurable.

---

_Comment by @akx on 2023-01-24 15:04_

@not-my-profile :+1: As discussed in #2128, the UC for that additional configuration is for easier increasingly strict typing of a large repo, where `mypy.ini` is being used to control which modules are being strictly type-checked and which aren't.

---

_Comment by @not-my-profile on 2023-01-24 15:06_

Right ... I wasn't asking about the use case for your config setting but the use case for these ruff checks in general.

I don't think it makes sense for a project to use ruff to check for missing types at all when the project is already using a static type checker like mypy or pyright (and I also don't see any reason for a project to not be using a static type checker).

---

_Comment by @charliermarsh on 2023-01-24 15:30_

> I don't think it makes sense for a project to use ruff to check for missing types at all when the project is already using a static type checker like mypy or pyright (and I also don't see any reason for a project to not be using a static type checker).

This strikes me as a reasonable stance in theory but not one I agree with in practice.

I haven't been spent time benchmarking Mypy or Pyright, in part because comparing them to Ruff generally doesn't make sense given how different they are, but I'd guess invoking Ruff is... 100x faster? More? I don't know. Even at 10x I would feel the same way. If Ruff is orders of magnitude faster and can help you spot issues reliably (like missing annotations, which Mypy will also enforce), it's extremely useful to be able to enforce those rules with Ruff too. Unlike Mypy, Ruff can also automatically add type annotations for you, which is, again, extremely useful!


---

_Comment by @akx on 2023-01-24 18:34_

Yep, I agree with @charliermarsh here! From a DX standpoint it's really useful and nice to have a fast tool immediately point out that I should annotate this thing, or better yet, automagically fill them in, even if enforcement is done separately by another (slower) tool.

---

_Comment by @tekumara on 2023-01-24 22:50_

We use a type checker (pyright) but need this linter to enforce that type annotations are used in function declarations.

Without type annotations, pyright will infer `Unknown` for function args and return types. By default, it will just ignore unknown types and do nothing unless strict mode is enabled. But we don't want strict mode because it will also error on third party libraries that don't have type stubs. So we use non-strict mode with this linter to catch untyped functions just in our code base.

---

_Comment by @not-my-profile on 2023-01-26 04:14_

> If Ruff is orders of magnitude faster and can help you spot issues reliably [..], it's extremely useful to be able to enforce those rules with Ruff too.

Yes ruff is much faster when run from scratch against a large codebase. Pyright does however support fast incremental updates, which UX-wise bring it very much up to speed with ruff. I use both the ruff and pyright via LSP at the same time and both report mistakes I make instantaneously.

> Ruff can also automatically add type annotations for you, which is, again, extremely useful

Pyright has very sophisticated type inference and does not require type annotations for functions that are so simple that ruff can infer the return type.

> But we don't want strict mode because it will also error on third party libraries that don't have type stubs.

You can just use `typeCheckingMode = "strict"` with `reportMissingTypeStubs = "warning"` ... pyright [can be extensively configured](https://github.com/microsoft/pyright/blob/main/docs/configuration.md).

---

My concern is that by implementing some lints that obviously fall within the area of operation of a type checker, users less familiar with Python tooling might get the wrong impression that ruff does it all ... (linting and type checking) ... which very much is not the case.

I think at the very least we should add a disclaimer to the ruff documentation that while ruff can report some simple type errors, it is not a type checker, so it is very much recommended to use it alongside an actual type checker, such as mypy or pyright ... but I don't feel this adequately addresses my concern (since many users won't read the documentation).

I think the best solution would be to implement a ruff-specific rule that checks if some kind of type checker has been configured in `pyproject.toml` ... I'll see to this, when I get around to #1472.

---

_Comment by @tekumara on 2023-01-26 04:29_

> You can just use typeCheckingMode = "strict" with reportMissingTypeStubs = "warning" ... pyright [can be extensively configured](https://github.com/microsoft/pyright/blob/main/docs/configuration.md).

`reportMissingTypeStubs = "warning"` doesn't help me though... I will still get `reportUnknown*` errors from third-party code without stubs, or with only partial type annotations.

---

_Comment by @charliermarsh on 2023-01-26 04:45_

I totally understand your concerns, but I still want to support these features. I think they're useful!

Pyright is great, but it's orders of magnitude slower than Ruff, even when run in incremental mode. (This is not a criticism of Pyright. It's a very different tool.) And Mypy, which is slower, is more popular, despite being more limited in its inference. I can provide data here, but I'm trying to avoid getting bogged down in the details since I don't think they're super important.

I want Ruff to be useful to and useable by everyone, but it's primarily targeted at large projects. I think it's very unlikely that large projects will misunderstand the relationship between Ruff and a type checker. So in that light, I'm fine with doing some basic enforcement around annotations. Of course, I'm happy to make the docs clearer regarding how Ruff fits into the stack.

(I'd be more hesitant if we were trying to provide some kind of type-inference-based features without implementing a full-blown type checker, but that's not what's being debated here :))


---
