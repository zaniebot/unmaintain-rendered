```yaml
number: 289
title: "Alternative or remove `lint` prefix in diagnostics"
type: issue
state: closed
author: MichaReiser
labels:
  - diagnostics
assignees: []
created_at: 2025-05-09T11:06:31Z
updated_at: 2025-05-10T13:03:10Z
url: https://github.com/astral-sh/ty/issues/289
synced_at: 2026-01-10T02:34:09Z
```

# Alternative or remove `lint` prefix in diagnostics

---

_Issue opened by @MichaReiser on 2025-05-09 11:06_

I find the `lint::` prefix in front of every rule diagnostic very noisy. It's also somewhat confusing that it is `lint` but we use `rules` in the configuration.

```
warning: lint:undefined-reveal: `reveal_type` used without importing it
  --> /Users/micha/astral/test/notebook.ipynb:13:1
   |
11 | if "🇬🇧"[::-1] and b == True: ...
12 |
13 | reveal_type("🇬🇧"[::-1] == "🇧🇬")
   | ^^^^^^^^^^^
14 | static_assert("🇺🇦"[::-1] == "🇦🇺")
15 | static_assert("🇸🇪"[::-1] == "🇪🇸")
   |
info: This is allowed for debugging convenience but will fail at runtime
info: `lint:undefined-reveal` is enabled by default
```

The motivation for the `lint` prefix is that we want every `LintId` to be globally unique. This is important when it comes to suppression comments (although we already allow users to omit the prefix there). 
The challenge with lint rules is that it is an open "namespace". 

Possible options:

* Rename `lint:` to `rule:` 
* Make it a postfix, e.g. `undefined-reveal (rule)` where we render the `(rule)` is rendered *dimmed* 
* Omit it entirely
* ... other ideas?

CC: @BurntSushi 

---

_Label `diagnostics` added by @MichaReiser on 2025-05-09 11:06_

---

_Comment by @AlexWaygood on 2025-05-09 11:11_

> Omit it entirely

I would just do this for now, personally. I don't think the reason for the prefix being there is at all obvious to users, and I also find it really noisy in the diagnostic rendering (especially when using `--output-format=concise`)

---

_Comment by @BurntSushi on 2025-05-09 11:13_

I would also omit it. And I'd clean up the formatting a little bit too, e.g.,

```
warning[undefined-reveal]: `reveal_type` used without importing it
...
```

---

_Comment by @MichaReiser on 2025-05-09 11:15_

I'm fine with that. We'll see if users get confused once ty supports multiple tools but we could e.g. use notes if necessary

---

_Comment by @AlexWaygood on 2025-05-09 11:17_

> And I'd clean up the formatting a little bit too, e.g.,
> 
> ```
> warning[undefined-reveal]: `reveal_type` used without importing it
> ```

Hmm, not sure -- I think the spaces we have currently make it more readable?

```
warning: undefined-reveal: `reveal_type` used without importing it
```

Or maybe we could use round brackets -- it feels closer to English prose?

```
warning(undefined-reveal): `reveal_type` used without importing it
```

---

_Comment by @BurntSushi on 2025-05-09 11:19_

The `severity[id]` format is what rustc uses so I guess I'm used to it. I don't like the spaces personally. It looks oddly unbalanced to me. Parentheses seem okay to me. Or at least, not really much difference between them and square brackets to my eyes.

---

_Comment by @MichaReiser on 2025-05-09 11:24_

I like the `warning[name]`, it removes some unnecessary space and the colons. I slightly prefer it over `warning(name)`. Not sure why

---

_Comment by @AlexWaygood on 2025-05-09 11:26_

> it removes some unnecessary space

They're all exactly the same number of characters 😆

```
warning: undefined-reveal: `reveal_type` used without importing it
warning[undefined-reveal]: `reveal_type` used without importing it
warning(undefined-reveal): `reveal_type` used without importing it
```

---

_Comment by @MichaReiser on 2025-05-09 11:28_

What I mean by space is whitespace. I also really dislike the double colon. 

---

_Comment by @zanieb on 2025-05-09 13:48_

I have felt strongly for a while now that `lint` is not an appropriate name for the type check rules / violations :)

Why is it being used for the concept when the user-facing config is "rule"?

---

_Comment by @MichaReiser on 2025-05-09 14:05_

> Why is it being used for the concept when the user-facing config is "rule"?

Just a relict from when it was called `lint` in the configuration too.

We intentionally use the term `lint` internally because we may want to have some lints that aren't configurable but suppressable and those wouldn't be rules.

---

_Comment by @zanieb on 2025-05-09 14:13_

I'm not sure I understand how `lint` creates that distinction, can you say more?

---

_Comment by @MichaReiser on 2025-05-09 14:14_

lint is the generic concept for any suppressable violation. A lint rule is a specific form of lint that can be enabled or disabled. We don't have any lint today that isn't a lint rule.

This isn't a user-visible concept today 

---

_Comment by @zanieb on 2025-05-09 14:17_

I see, thanks! I think I stand by "lint" being a misleading term for this and would advocate for renaming it (which may be a separate question from removing the prefix). It sounds like it can't be a "rule" because that implies configurability, which seems fine. What's the concept for a non-suppressible violation?

---

_Comment by @MichaReiser on 2025-05-09 14:21_

> I see, thanks! I think I stand by "lint" being a misleading term for this and would advocate for renaming it (which may be a separate question from removing the prefix). It sounds like it can't be a "rule" because that implies configurability, which seems fine. What's the concept for a non-suppressible violation?

These are all just code internals. There's no single concept for other error codes. They're just [`DiagnosticId`s](https://github.com/astral-sh/ruff/blob/981bd70d393fe29324c07c3ffce5ecddd40e6bd6/crates/ruff_db/src/diagnostic/mod.rs#L575-L594). Examples are IO errors, syntax errors, revealed types, configuration errors (unknown rule), panics... Just anything that isn't a rule

---

_Comment by @zanieb on 2025-05-09 14:29_

> Just anything that isn't a rule

Anything that isn't a lint ;)

Would it be reasonable to call the top-level concept a "rule" even if it's not configurable? Why does it need a different name if there's configuration exposed? (just trying to understand the existing model, thanks for your patience!)

---

_Comment by @MichaReiser on 2025-05-09 15:00_

> Would it be reasonable to call the top-level concept a "rule" even if it's not configurable? Why does it need a different name if there's configuration exposed? (just trying to understand the existing model, thanks for your patience!)

Let's discuss this somewhere else and after the release. 

---

_Comment by @AlexWaygood on 2025-05-10 13:03_

Fixed in https://github.com/astral-sh/ruff/pull/17987

---

_Closed by @AlexWaygood on 2025-05-10 13:03_

---
