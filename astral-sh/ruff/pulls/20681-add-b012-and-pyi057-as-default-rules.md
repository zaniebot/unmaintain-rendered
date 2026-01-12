```yaml
number: 20681
title: "Add `B012` and `PYI057` as default rules"
type: pull_request
state: closed
author: ntBre
labels:
  - breaking
  - rule-selection
assignees: []
draft: true
base: main
head: brent/new-default-rules
created_at: 2025-10-02T14:58:00Z
updated_at: 2025-10-07T16:23:39Z
url: https://github.com/astral-sh/ruff/pull/20681
synced_at: 2026-01-12T15:57:07Z
```

# Add `B012` and `PYI057` as default rules

---

_@ntBre_

## Summary

@AlexWaygood and I discussed this a bit on [Discord](https://discord.com/channels/1039017663004942429/1343691651243315312/1422892554868756552), and it seemed relevant for the Python 3.14 release next week, so I figured I'd put up a PR.

This adds [jump-statement-in-finally (B012)](https://docs.astral.sh/ruff/rules/jump-statement-in-finally/#jump-statement-in-finally-b012) and [byte-string-usage (PYI057)](https://docs.astral.sh/ruff/rules/byte-string-usage/#byte-string-usage-pyi057) to the default rule set. `B012` is especially relevant for Python 3.14 because it will become a `SyntaxWarning` and may be upgraded to a `SyntaxError` in the future. `PYI057`, on the other hand, covers APIs that now won't be removed until 3.17, but Ruff could help to make this more visible to avoid it being pushed back again.

I'm realizing now that I didn't attempt to gate the new defaults based on the target Python version as Alex suggested, but I can look into that if we want. It doesn't immediately seem straightforward to do, based on our current usage of `DEFAULT_SELECTORS`, though. `B012` seems serious enough to me always to be enabled, but the nice stdlib replacement for `PYI057` was only added in Python 3.12.

## Test Plan

Existing tests, plus a new CLI test with the new rules


---

_Added to milestone `v0.14` by @ntBre on 2025-10-02 14:58_

---

_Label `breaking` added by @ntBre on 2025-10-02 14:58_

---

_Label `rule-selection` added by @ntBre on 2025-10-02 14:58_

---

_Comment by @github-actions[bot] on 2025-10-02 15:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/prefecthq/prefect/blob/54d62e660d90f556e7b0f204f15b3b61c8f8cd21/src/integrations/prefect-snowflake/prefect_snowflake/database.py#L799'>src/integrations/prefect-snowflake/prefect_snowflake/database.py:799:17:</a> B012 `return` inside `finally` blocks cause exceptions to be silenced
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B012 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/prefecthq/prefect/blob/54d62e660d90f556e7b0f204f15b3b61c8f8cd21/src/integrations/prefect-snowflake/prefect_snowflake/database.py#L799'>src/integrations/prefect-snowflake/prefect_snowflake/database.py:799:17:</a> B012 `return` inside `finally` blocks cause exceptions to be silenced
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B012 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@AlexWaygood reviewed on 2025-10-02 15:29_

---

_Review comment by @AlexWaygood on `docs/configuration.md`:56 on 2025-10-02 15:29_

(Use spaces rather than tabs ;)

```suggestion
    # Enable Pyflakes (`F`), a subset of the pycodestyle (`E`) codes,
    # and a couple of other codes corresponding to CPython deprecations
    # and syntax warnings by default.
```

---

_@ntBre reviewed on 2025-10-02 15:33_

---

_Review comment by @ntBre on `docs/configuration.md`:56 on 2025-10-02 15:33_

Wow how did you see the tabs instead of spaces?? I thought for sure GitHub's suggestion rendering was failing me until I checked locally. Thanks!

---

_@AlexWaygood reviewed on 2025-10-02 15:36_

---

_Review comment by @AlexWaygood on `docs/configuration.md`:56 on 2025-10-02 15:36_

> Wow how did you see the tabs instead of spaces??

I'll never reveal my secrets ;)

---

_Comment by @AlexWaygood on 2025-10-02 15:42_

> `B012` seems serious enough to me always to be enabled

agreed; I think this is almost always a mistake, whatever Python version you're using

> but the nice stdlib replacement for `PYI057` was only added in Python 3.12.

Hmm, yeah. _Ideally_ I think this would only be enabled by default if your code targets Python 3.12+. But most users can probably use a `bytes | bytearray` union on lower Python versions instead of an ABC. And if they have typing_extensions as a dependency, they can use the backport of `collections.abc.Buffer` in typing_extensions too. So I think it _should_ be okay if it's made the default regardless of target Python version.

It might be worth adding some more examples to https://docs.astral.sh/ruff/rules/byte-string-usage/#example for what you can do if you support Python <3.12? (It also seems like there's a typo in https://docs.astral.sh/ruff/rules/byte-string-usage/#why-is-this-bad -- it says `Python \<3.12` rather than `Python <3.12`.)

---

_Comment by @MichaReiser on 2025-10-02 15:47_

I'm a bit worried about making our default rules more complicated, especially if the selection becomes dynamic

> I'm realizing now that I didn't attempt to gate the new defaults based on the target Python version as Alex suggested, but I can look into that if we want.

Could `B012` become a syntax rule? I guess the main reason why it shouldn't is because there are reasons why someone would want to disable it (on a per-file or per-line basis).



---

_Comment by @ntBre on 2025-10-02 16:01_

I can definitely expand the examples, I think I already caught the typo but I'll double check that too.

> Could B012 become a syntax rule? I guess the main reason why it shouldn't is because there are reasons why someone would want to disable it (on a per-file or per-line basis).

It's tempting to make it a semantic syntax error, but it would be a bit more aggressive than CPython itself. The [PEP](https://peps.python.org/pep-0765/#emit-syntaxerror-in-cpython) explicitly rejected or at least deferred plans to upgrade the warning to an error.

I know what you mean about expanding the defaults. I'm also happy to split the two rules into separate PRs and/or defer both until a later release after more discussion. I just wanted to have it ready in case we wanted to include these in the 3.14 release next week.

---

_Comment by @AlexWaygood on 2025-10-03 09:30_

> I'm a bit worried about making our default rules more complicated, especially if the selection becomes dynamic

Yeah, I can see that making the default selection dynamic might be pretty confusing to users. As I said above, I think it's probably okay if PYI057 is enabled by default regardless of Python version. Of the two, I also think there's an even stronger case for enabling B012 by default, as well, so considering the two rules separately might indeed make sense here.

> Could `B012` become a syntax rule? I guess the main reason why it shouldn't is because there are reasons why someone would want to disable it (on a per-file or per-line basis).

Yeah, I think it's pretty important that B012 should be suppressible:
- CPython will still parse and execute the code at runtime (albeit with a warning)
- The vast majority of the time, code that uses this pattern isn't working as intended, and there's always a way to rewrite the code in a clearer, more explicit way. But not _every_ instance of this pattern is _necessarily_ a bug; sometimes it might actually be working in the desired way! Even in these instances, I think the lint is beneficial, since you could rewrite the code in a clearer way, but that doesn't mean that it should be unsuppressible.
- What if your code has lots of instances of this pattern? I think it would be reasonable to add `noqa` comments to all occurences (using `--add-noqa`) as part of the initial PR upgrading your Ruff version, and then slowly remove the `noqa` comments in followup PRs.
- There's no particular reason to make B012 non-suppressible while other rules that cover `SyntaxWarning`s at runtime (such as [F632](https://docs.astral.sh/ruff/rules/is-literal/)) are still suppressible.

---

_Comment by @MichaReiser on 2025-10-03 10:23_

It makes me wonder if we should have a single rule that flags any syntax warnings in your code (maybe dependent on python version). 

But that is a separate discussion 

---

_Comment by @AlexWaygood on 2025-10-03 10:31_

yeah, I do agree that we should try to ensure that we catch all pattenrs that CPython flags at compile-time with `SyntaxWarning`s, that rules which flag syntax warnings such as `B012` and `F632` should probably all be enabled by default, and that these rules should probably all be in a common category. It's definitely a similar problem to the syntax-errors project, and I've chatted with @ntBre about it before. But, I also agree that this is a separate, broader discussion.

---

_Comment by @dylwil3 on 2025-10-03 12:48_

> It makes me wonder if we should have a single rule that flags any syntax warnings in your code (maybe dependent on python version).
> 

see https://github.com/astral-sh/ruff/issues/20196

---

_Comment by @ntBre on 2025-10-03 18:19_

It sounds like we may want to hold off on this for now and take a more general approach to syntax warnings, which sounds good to me. I think we can still call out these rules in the release post without enabling them by default and hopefully still help with the deprecation.

---

_Closed by @ntBre on 2025-10-07 16:23_

---
