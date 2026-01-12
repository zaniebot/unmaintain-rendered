```yaml
number: 12693
title: "[Feature Request] User-Defined Deprecations?"
type: issue
state: closed
author: alanhdu
labels: []
assignees: []
created_at: 2024-08-05T16:57:44Z
updated_at: 2024-11-10T09:30:37Z
url: https://github.com/astral-sh/ruff/issues/12693
synced_at: 2026-01-12T15:54:52Z
```

# [Feature Request] User-Defined Deprecations?

---

_@alanhdu_

This is probably a big feature request (and apologies if it has already been asked about), but I am wondering whether there are plans for ruff to allow *user*-defined / config-defied lint rules?

In particular, I have some internal codebase functions that are deprecated and would like people to move off of. I can certainly try to write a custom CI rule for this, but it'd be super nice if I could somehow configure `ruff` to detect this (since it's already integrated into our CI).

I admit I haven't fully thought through what this would look like in practice. I'd love a world where I can specify in my toml file something like:

```
[ruff.deprecated]
rule1 = { function="my.code.deprecated_function", message="This function is deprecated", replacement="some.other.function", safe=False}
```

Is something like this feasible?

(There's a more general question about a plugin interface -- our codebase is big enough that having custom lint rules is useful, but it's not OSS so upstreaming changes to ruff probably isn't worthwhile and forking ruff to add our own rules seems undesirable).

---

_Comment by @AlexWaygood on 2024-08-05 17:37_

Hi, thanks for the feature request! My instinct is that we should probably fold this into the broader discussion in https://github.com/astral-sh/ruff/issues/283, which discusses design principles for plugins/user-specific lints.

---

_Comment by @MichaReiser on 2024-08-06 06:04_

I do like the idea and I see generic rules as a first step towards a customizable Ruff that doesn't require plugin support.

This request is similar to https://github.com/astral-sh/ruff/issues/10131. Would Ruff's [banned-api](https://docs.astral.sh/ruff/rules/banned-api/) work for your use case?

---

_Closed by @MichaReiser on 2024-11-10 09:30_

---
