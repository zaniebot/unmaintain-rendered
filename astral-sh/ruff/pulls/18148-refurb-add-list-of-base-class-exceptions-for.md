```yaml
number: 18148
title: "[`refurb`] add list of base class exceptions for `FURB180`"
type: pull_request
state: open
author: robsdedude
labels:
  - rule
  - configuration
  - preview
assignees: []
base: main
head: feat/13307-furb180-configure-exceptions
created_at: 2025-05-17T11:06:12Z
updated_at: 2025-06-13T21:23:55Z
url: https://github.com/astral-sh/ruff/pull/18148
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] add list of base class exceptions for `FURB180`

---

_Pull request opened by @robsdedude on 2025-05-17 11:06_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add a config option `tool.ruff.lint.refurb.allow-abc-meta-bases` (`["typing.Protocol", "typing_extensions.Protocol"]` by default) that allows to specify a list such that if  class has at least one base-class and all base-classes are in this list it is exempt from `FURB180`. This is useful for classes that validate their base-classes at runtime such as the two that are the default value.

## Test Plan

Besides the adjusted test `ruff_linter::rules::refurb::tests::rules`, I created a minimal Python project to test the configuration option.

## References

Fixes https://github.com/astral-sh/ruff/issues/13307 (together with https://github.com/astral-sh/ruff/pull/18149)


---

_Comment by @MichaReiser on 2025-05-25 11:00_

@ntBre could you take a look at this PR when you're back?

---

_Label `rule` added by @ntBre on 2025-05-29 19:37_

---

_Label `configuration` added by @ntBre on 2025-05-29 19:37_

---

_Label `preview` added by @ntBre on 2025-05-29 19:44_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:3425 on 2025-05-29 19:52_

One thing to note here, which might be fine, is that this example will remove `typing.Protocol` and `typing_extensions.Protocol` from the list, so users will have to include those manually when modifying this. We often include an `extend_*` variant of the settings to get around this, but maybe that's overkill in this situation.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-05-29 19:59_

I think I'd probably slightly prefer something like this to avoid having to allocate a `HashSet`:

```suggestion
    let bases = class_def.bases();
    let all_bases_allowed = !bases.is_empty()
        && bases.iter().all(|base| {
            checker
                .semantic()
                .resolve_qualified_name(base)
                .map(|qualified_name| qualified_name.to_string())
                .is_some_and(|name| checker.settings.refurb.allow_abc_meta_bases.contains(&name))
        });

    if all_bases_allowed {
        return;
    }
```

---

_@ntBre approved on 2025-05-29 20:00_

Thanks, this all makes sense to me! I just had a couple of minor suggestions.

---

_Comment by @github-actions[bot] on 2025-05-29 20:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@robsdedude reviewed on 2025-05-30 19:01_

---

_Review comment by @robsdedude on `crates/ruff_workspace/src/options.rs`:3425 on 2025-05-30 19:01_

I'm happy to implement the extend approach, if you prefer to have it. I don't expect it to take very long to do. 

 1. Do you want me to?
 2. If yes, should the extend config co-exist with the full list config that I've added? If yes to this as well, when both configs are given, I assume they should be merged?

---

_@robsdedude reviewed on 2025-05-30 19:04_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-05-30 19:04_

Smart :cookie: 

---

_@robsdedude reviewed on 2025-05-30 21:47_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-05-30 21:47_

Actually, thinking about it more, shouldn't it be that no lint is emitted if any of the base classes is in the whitelist,  instead of all as currently implemented? I'm thinking that if even one of the bases is known to be one that does run-time checks on other base classes (or otherwise change it's behavior in an unwanted way when fixing this lint), the lint should probably be silenced. What do you think @ntBre?

---

_Converted to draft by @robsdedude on 2025-06-03 05:44_

---

_@robsdedude reviewed on 2025-06-03 06:38_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-06-03 06:38_

I just went with `any` for now. The commit can be reverted easily, I figured.

---

_@robsdedude reviewed on 2025-06-03 06:39_

---

_Review comment by @robsdedude on `crates/ruff_workspace/src/options.rs`:3425 on 2025-06-03 06:39_

I added an extension option.

---

_Marked ready for review by @robsdedude on 2025-06-03 08:54_

---

_Review requested from @ntBre by @robsdedude on 2025-06-03 08:54_

---

_@ntBre reviewed on 2025-06-03 14:27_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:3425 on 2025-06-03 14:27_

Sorry my suggestion was a bit vague, I just didn't have strong feelings either way. I think it does make sense to have the extend version for convenience!

---

_@ntBre reviewed on 2025-06-03 14:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-06-03 14:29_

Ah `any` does make sense to me. I thought `all` was preserving the behavior of `is_subset`, but I might have gotten that wrong.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/settings.rs`:20 on 2025-06-03 14:32_

This is kind of minor, but I think we usually go ahead and combine the settings at this point. For example:

https://github.com/astral-sh/ruff/blob/87a15f7e4cf328e6485cee98960d19f51a955ef4/crates/ruff_workspace/src/options.rs#L1457-L1468

That should simplify the check a bit in the rule itself.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:42 on 2025-06-03 14:45_

It would be a bit more verbose, but I think something like `allowed-abc-meta-bases` and `extend-allowed-abc-meta-bases` would be more similar to our other settings like this. It would be even longer still, but I might prefer `base-classes` instead of just `bases` too.

cc @MichaReiser, do you have any suggestions for the names?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB180_FURB180.py.snap`:3 on 2025-06-03 14:49_

Is it possible that you ran quite an old version of cargo-insta? I don't remember seeing this before, and it seems like it was removed in 2022 (https://github.com/mitsuhiko/insta/pull/218).

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/mod.rs`:129 on 2025-06-03 14:57_

Could this use the `test_case` macro? Maybe something like this:

```suggestion
    #[test_case(&["ast.Name", "FURB180_exceptions.A0"]; allow_abc_meta_bases)]
    fn test_furb180_exemption(settings: refurb::settings::Settings) -> Result<()> {
```

and then change the type of the argument to `&[&str]` or something. That would only work if the `Settings` fields were combined as in my other comment. You could also pass `Settings` directly via `test_case` but that might be a bit unwieldy.

If `extend` is handled before the `Settings` conversion, there might be less reason for two tests here anyway.

---

_@ntBre reviewed on 2025-06-03 15:17_

Thanks, this is looking good. Just a couple more suggestions, and one question about naming

---

_@robsdedude reviewed on 2025-06-03 15:50_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:88 on 2025-06-03 15:50_

I think `all` does indeed preserve the functionality of `is_subset`. Your suggested re-write just made it more obvious what it does and that made me think whether I really want what I wrote :upside_down_face: 

---

_@robsdedude reviewed on 2025-06-03 15:56_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:42 on 2025-06-03 15:56_

@ntBre I based the design (the naming as well as not merging the options) on the existing `allowed_markup_calls` and `extend_markup_names` config options pair... Which I just realized isn't even a pair. Reading do be hard 🤦

---

_Renamed from "FURB180: add list of base class exceptions" to "[`refurb`] add list of base class exceptions for `FURB180`" by @robsdedude on 2025-06-03 16:01_

---

_@robsdedude reviewed on 2025-06-03 16:08_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB180_FURB180.py.snap`:3 on 2025-06-03 16:08_

Oh wow :see_no_evil: That really confused me for a hot minute. My insta was up-to-date, I'm just a silly. I manually renamed the new snapshot files instead of using `cargo insta review`. I didn't anticipate it to do anything other that that, and doing it manually allowed me to use diff tooling that better suits me. Welp, that won't happen again :sweat_smile: thanks.

---

_@robsdedude reviewed on 2025-06-03 16:16_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:42 on 2025-06-03 16:16_

However, `Flake8GetTextOptions` has `function_names` and `extend_function_names` which works as you suggested. Similarly with `Flake8ImportConventionsOptions`'s `aliases` and `extend_aliases` and `Flake8SelfOptions`'s `ignore_names` and `extend_ignore_names`. There are some more option pairs that work like this.

And then, there's `Flake8PytestStyleOptions`, which has `raises_require_match_for` and `raises_extend_require_match_for` as well as `warns_require_match_for` and `warns_extend_require_match_for`. So this plugin follows its own naming scheme and is not merged either.

So it seems there is prior art for pretty much any approach :upside_down_face: However, the one you suggested seems to be the most common. So I'll get the PR aligned with this :)

---

_@ntBre reviewed on 2025-06-03 19:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:42 on 2025-06-03 19:07_

Yeah I did a double take on the markup rule too :smile: I'm sure we're not totally consistent, so this is more of a preference than a hard requirement. Micha might have some insight here too.

I was having trouble highlighting this subtle difference in the code formatting, but I still think allow**ed** might be better than "allow" alone. When I was searching through the settings earlier, it looked like `allow-` is usually for boolean settings like `allow-multiline`, while `allowed-` is more often used for lists (although there are exceptions there too!)

I think that's my last nit/bike-shedding suggestion on the name, though. For more context, it's a bit of a pain to deprecate/rename config options later, which is the only reason I'm so worried about nailing the names.

---

_@ntBre reviewed on 2025-06-03 19:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB180_FURB180.py.snap`:3 on 2025-06-03 19:08_

Ahh, gotcha. I wouldn't have expected that either!

---

_@ntBre approved on 2025-06-03 19:10_

Awesome, thanks again! This is good to go from my perspective, modulo any further bike-shedding on the name.

---

_@robsdedude reviewed on 2025-06-04 07:19_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:42 on 2025-06-04 07:19_

> I was having trouble highlighting this subtle difference in the code formatting

Oh :innocent: I missed that detail indeed. This seems like the appropriate kind of bike-shedding to me :D

---

_Review requested from @ntBre by @robsdedude on 2025-06-04 07:53_

---

_Comment by @robsdedude on 2025-06-11 16:25_

 @ntBre Just want to make sure this is still on your radar :eyes: 

---

_Comment by @ntBre on 2025-06-11 16:42_

Oh yes, thank you! Let me actually cc someone who might have an opinion on the name (@MichaReiser any thoughts on the names and maybe the two separate options?)

I believe the current state is that we added two new config options:
- `lint.refurb.allowed_abc_meta_bases`
- `lint.refurb.extend_allowed_abc_meta_bases`



---

_@MichaReiser reviewed on 2025-06-12 06:24_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:3423 on 2025-06-12 06:24_

Do we need to gate this behind preview, given that we now emit fewer lints (which can lead to unused noqa comments) or do we consider this a bug fix?

From our versioning policy

> A new configuration option is added in a backwards compatible way (no formatting changes or new lint errors)

So I think we need to gate this behind preview

---

_Comment by @MichaReiser on 2025-06-12 06:31_

> Oh yes, thank you! Let me actually cc someone who might have an opinion on the name (@MichaReiser any thoughts on the names and maybe the two separate options?)

I'm not entirely sure I understnad what you want feedback on. I see that this PR introduces two new options. Are you asking for my opinion on the option names? Is there mroe feedback that you're looking for?

Looking at the issue https://github.com/astral-sh/ruff/issues/13307#issuecomment-2340770515 it's unclear if we want these options. More specifically, options have a non-zero cost because users need to understand them. This PR also changes the default for everyone using the rule, which may not what they want.



---

_Comment by @robsdedude on 2025-06-13 21:23_

> This PR also changes the default for everyone using the rule, which may not what they want.

That's a fair point. I figured if users are in the situation where this becomes relevant, they have code along the lines of

```python
class Foo(Protocol, metaclass=ABCMeta): ...
```
I imagine if a user wants this to be linted, the lint shouldn't be "use ABC as a base instead of ABCMeta", but  rather "don't bother specifying ABC/ABCMeta if the base(s) already includes ABC in the class hierarchy".

---
