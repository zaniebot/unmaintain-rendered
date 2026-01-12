```yaml
number: 1612
title: "Design: Rule settings"
type: issue
state: open
author: MichaReiser
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2025-11-23T14:21:43Z
updated_at: 2025-12-31T15:35:52Z
url: https://github.com/astral-sh/ty/issues/1612
synced_at: 2026-01-12T15:54:25Z
```

# Design: Rule settings

---

_@MichaReiser_

https://github.com/astral-sh/ty/issues/1354, https://github.com/astral-sh/ty/issues/1422, and https://github.com/astral-sh/ty/issues/278 are in themselves trivial, but they're all blocked on a decision on where to put rule-related settings/type checker settings other than environment.

Here a few options

## Top-level

Put the settings directly under `ty`. 

```toml
[tool.ty]
allowed-unresolved-modules = [...]
```

The main downside of this, and the reason why all existing settings are grouped, is that top-level settings make it more difficult to explain which settings are allowed within `.overrides`, in user-level configurations, or when we add hierarchical configurations. 

No grouping can also make it more difficult to discover options, asssuming we can come up with meaningful section names. 

## Rule-level

```toml
[tool.ty.rules]
unresolved-import = { level = "warning", allow=[...] }
```

I like that the settings are very local to the rule. However, many settings are specific to many rules (e.g. whether ty should respect `type: ignore` comments) and I worry that the configuration schema becomes very complicated and I don't know how well editors support tagged-unions... 

## Sub-section

This is still my favorite option, except that I'm struggling to come up with a good section name. The most obvious choice is `check`

```toml
[tool.ty.check]
allowed-unresolved-modules = [...]
```

Long-term, what if ty also supports formatting and the options only apply to typing? Is `check` still a good name? Could we come up with something more meaningful? `tool.ty.typing`? 

The other downside is that a separte section is somewhat verbose when using overrides

```toml
[[tool.ty.overrides]]
include = [...]

[[tool.ty.overrides.check]]
allowed-unresolved-modules = []
```

I think the following should also work, but only for as long as the configuration fits on a single line

```toml
[[tool.ty.overrides]]
include = [...]
check.allowed-unresolved-modules = []
```




---

_Label `configuration` added by @MichaReiser on 2025-11-23 14:21_

---

_Label `needs-design` added by @MichaReiser on 2025-11-23 14:21_

---

_Comment by @MichaReiser on 2025-12-09 12:47_

Related to rule settings, there's also a discussion of where to put a potential `strict-typing` setting, which controls how ty handles `type-ignore` comments, and a setting to allow unresolved modules (which is the case I used above). What's special about `strict-typing` is that this is another setting that probably can't be overridden in an initial version; instead, it applies to the entire project. 

Under the current design, the idea was to use different sections for project-level settings (e.g., `environment`) and settings that can be overridden at a per-file level. The idea behind this guiding principle was that having settings grouped this way makes it relatively easy to explain and verify which settings can be used within `overrides`, a user-level configuration, or, in the future, a workspace member configuration. 

However, I do think there are two main challenges with it and I'm starting to conclude that we should give up on this principle:

* It's difficult to come up with both semantically meaningful section names while also separating project-level settings from file-level settings. E.g., one proposal is to introduce an `analyze` section for settings that change how the analyzer works (this section might differ from rule-specific options). If there were an `analyze` section, as a user, I'd expect both `allowed-unresolved-modules` and `strict-typing` to be there rather than, e.g., finding `strict-typing` in `environment` just because it's a project-level setting.
* It's impossible to predict the future, and what used to be a project-level setting inevitably becomes a file-level (or member-level) setting. For example, we currently don't support checking multiple projects in a single ty invocation. However, this might change depending on how we design our mono-repository support. It would be unfortunate if we had to move settings around just because their scope has changed. 

Given that project-level settings should be rare, I think it should be sufficient if their scope is documented (and warning about invalid usages). 

Drawing this conclusion, I think we should put `allowed-unresolved-modules` into a new `analyze` section. I consider `allowed-unresolved-modules` an analyzer and not a rule-level setting because ignoring imports changes what types we infer for those modules. Meaning, the setting doesn't just change a single diagnostic; it has a more widespread impact on how your project is checked.

---

_Comment by @carljm on 2025-12-09 18:07_

I'm happy with your conclusions in the previous comment.

> a potential `strict-typing` setting, which controls how ty handles `type-ignore` comments

It's not clear to me why an option for how we handle `type: ignore` comments would use the name `strict-typing`? That seems like a very general name with many possible interpretations; if we have it at all, I think it probably needs to be a meta-option that turns on a bunch of other different options. (But I think we should try to avoid having it, and just let the individual options stand on their own.)

Another "strict typing" option that I think we will need is an option to not union `Unknown` into un-annotated attribute types. (Or alternatively this could be named something like a `gradual-typing` option.)


---

_Comment by @MichaReiser on 2025-12-09 18:51_

> It's not clear to me why an option for how we handle type: ignore comments would use the name strict-typing? That seems like a very general name with many possible interpretations; if we have it at all, I think it probably needs to be a meta-option that turns on a bunch of other different options. (But I think we should try to avoid having it, and just let the individual options stand on their own.)

Sorry, that must have been my auto correct tool making my English better but mixing up the semantics of what I tried to say 😆. These should be two settings: one for `strict-typing` and one for ignore comments.

---

_Comment by @zanieb on 2025-12-09 19:07_

Is the final concept then something like this?

```toml
[tool.ty.rules]
unresolved-import = { level = "warning" }

[tool.ty.analyze]
gradual-typing-strict-name = false
type-ignores = "foobar"
```

---

_Comment by @zanieb on 2025-12-09 19:07_

(I think `analysis` makes more sense than `analyze` if you're using `rules`?)

---

_Comment by @MichaReiser on 2025-12-09 19:19_

> Is the final concept then something like this?

Yes (minus exact setting names). I don't know yet whether we should put lint rule settings under `lint`, inline them into `rules`, put them in `rules.settings`, or put them in `analyze`, but I'm leaning towards punting on that design question for now because we don't need a decision today.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:35_

---
