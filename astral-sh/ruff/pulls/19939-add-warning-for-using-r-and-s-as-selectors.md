```yaml
number: 19939
title: Add warning for using R and S as selectors
type: pull_request
state: open
author: GDYendell
labels:
  - bug
  - configuration
assignees: []
base: main
head: deprecate-invalid-selectors
created_at: 2025-08-16T15:40:20Z
updated_at: 2025-09-16T07:57:51Z
url: https://github.com/astral-sh/ruff/pull/19939
synced_at: 2026-01-12T15:56:50Z
```

# Add warning for using R and S as selectors

---

_@GDYendell_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Following on from discussion in https://github.com/astral-sh/ruff/pull/17896, this change adds a user deprecation warning for selectors that currently work, but are not included in the schema. I believe R and S are the only ones.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I have tested this locally with 

```
ruff check --select R
```

However it does not display the warning. If I change to `println!` it does. Is there something I am not understanding about the `warn_user*` macros?

<!-- How was it tested? -->


---

_@ntBre reviewed on 2025-08-20 19:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-20 19:04_

Do you think it would make more sense to put this const and the warning in `rule_redirects.rs` next to the `REDIRECTS` hash map? Then the warning could be in `get_redirect` and possibly also `get_redirect_target`, which are where the redirects are looked up. That seems like a better fit to me than in the `FromStr` implementation.

I also mentioned the IC -> ICN redirect in my [comment](https://github.com/astral-sh/ruff/pull/17896/files#r2078434064) on the other PR. Should we add that here too? And there are several other `REDIRECTS` with TODO comments. I think it makes sense to try to cover them all in one PR. I think we can emit a warning for any redirect not present in the schema, as I also mention in the comment, based on the initial issue.

---

_Label `configuration` added by @ntBre on 2025-08-20 19:04_

---

_Label `bug` added by @ntBre on 2025-08-20 19:05_

---

_@GDYendell reviewed on 2025-08-21 00:04_

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 00:04_

> Do you think it would make more sense to put this const and the warning in rule_redirects.rs next to the REDIRECTS hash map? Then the warning could be in get_redirect and possibly also get_redirect_target, which are where the redirects are looked up. That seems like a better fit to me than in the FromStr implementation.

Yep this makes much more sense!

> I also mentioned the IC -> ICN redirect in my [comment](https://github.com/astral-sh/ruff/pull/17896/files#r2078434064) on the other PR. Should we add that here too? And there are several other REDIRECTS with TODO comments. I think it makes sense to try to cover them all in one PR. I think we can emit a warning for any redirect not present in the schema, as I also mention in the comment, based on the initial issue.

Ah yes sorry, I was only checking the single letter redirects initially. So, any redirect that is not in the schema should be deprecated? That is quite a few of them!

---

_@MichaReiser reviewed on 2025-08-21 12:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 12:37_

We might want to move this into `LintConfiguration`. It seems to be where we have all other deprecation warnings:

https://github.com/astral-sh/ruff/blob/e917d309f1d37887a9f5ea11698389caa6c76b11/crates/ruff_workspace/src/configuration.rs#L1044-L1052

---

_@ntBre reviewed on 2025-08-21 12:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 12:55_

> So, any redirect that is not in the schema should be deprecated? That is quite a few of them!

I think this is true, that's how I was checking before, but maybe Micha can confirm.

---

_@MichaReiser reviewed on 2025-08-21 13:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 13:02_

Do any of them appear in the schema? 

I'd focus on rules that are prefixes of other groups, because that can be confusing (e.g. `R` is a prefix of both `RET` and `RUF` but it only enables `RET`). I don't see much harm in supporting redirects that don't overlap with any other rule code and deprecating them does cause some churn for our users that I'd avoid unless we have good reason for it

---

_@ntBre reviewed on 2025-08-21 13:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 13:16_

I just spot-checked a few more, and I don't think any of them appear in the schema, so it's fine with me to focus on the confusing ones. The original issue (https://github.com/astral-sh/ruff/issues/17890) mentioned that the mismatch with the schema was causing errors in IDEs (in the pyproject.toml?), which I think is where I got the schema idea.

---

_@MichaReiser reviewed on 2025-08-21 13:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-21 13:20_

> mentioned that the mismatch with the schema was causing errors in IDEs (in the pyproject.toml?)

That's something we can solve separately by including those names in the schema (but mark them as deprecated)

---

_@GDYendell reviewed on 2025-08-30 17:38_

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-08-30 17:38_

> We might want to move this into LintConfiguration. It seems to be where we have all other deprecation warnings:

I think this would be too late. It is currently checking just before the redirect is applied during clap parsing
```
ruff_linter::rule_redirects::get_redirect (/home/gdy/development/ruff/crates/ruff_linter/src/rule_redirects.rs:26)
<ruff_linter::rule_selector::RuleSelector as core::str::traits::FromStr>::from_str (/home/gdy/development/ruff/crates/ruff_linter/src/rule_selector.rs:68)
...
clap_builder::derive::Parser::parse_from (/home/gdy/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.43/src/derive.rs:56)
ruff::main (/home/gdy/development/ruff/crates/ruff/src/main.rs:44)
```

after which "R" is converted to "RET". Unless it is possible to get the original command line arguments somehow? The map of redirects is empty here if I run with `check --select R`.

---

_@MichaReiser reviewed on 2025-09-16 07:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_selector.rs`:15 on 2025-09-16 07:57_

Hmm I see. It would require validating the clap arguments (and probably options too?) to when we resolve the settings but that's obviously a much larger change. 

The main downside I see of the current solution is that it isn't obvious to users where they used the deprecated code, making it difficult for them to act on the error. 

Could we either:

* Add an argument to `get_redirect` which tells where the redirect is used (e.g. CLI, Config(Path)) so that we can display the source as part of the error message
* Change the return type to `Redirect::To(String)`/`Redirect::Deprecated(String)` depending on whether the redirect is deprecated and use custom errors at each call site

---
