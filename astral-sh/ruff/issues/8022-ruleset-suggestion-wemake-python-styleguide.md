```yaml
number: 8022
title: "Ruleset suggestion: wemake-python-styleguide WPS2xx (complexity)"
type: issue
state: open
author: strmwalker
labels:
  - plugin
assignees: []
created_at: 2023-10-17T18:49:37Z
updated_at: 2024-11-29T18:03:19Z
url: https://github.com/astral-sh/ruff/issues/8022
synced_at: 2026-01-12T15:54:47Z
```

# Ruleset suggestion: wemake-python-styleguide WPS2xx (complexity)

---

_@strmwalker_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
`wemake-python-styleguide` ([link](https://wemake-python-styleguide.readthedocs.io/en/latest/index.html)) is a collection of plugins as well as a number of custom rules (WPSxxx). Implementing all rules outlined in #3845 seems to be an impossible task, so I suggest to tackle this step by step. The most useful section of WPS is a WPS2xx rules, focused on reducing overall unit complexity. 
I implemented WPS201, WPS202, WPS203, and plan to submit a PR in nearest time. I would appreciate help on other rules. Here is a list of WPS2xx rules:

Unlike mccabe (C90), WPS also relies on other metrics, like Jones complexity and cognitive complexity (as proposed by G. Ann Campbell). Some violations are covered by Pylint (PLR).

- [ ] WPS200 — Forbid modules with complex lines. (median [Jones Complexity](https://github.com/Miserlou/JonesComplexity) of module > 12)
- [x] WPS201 — Forbid modules with too many (12+) imports.
- [x] WPS202 — Forbid too many (7+) classes and functions in a single module.
- [x] WPS203 — Forbid modules with too many (50+) imported names.
- [ ] WPS204 — Forbid overused expressions in a module, function or method. (expression referenced over 4 times in function / over 7 times in module)
- [ ] WPS210 — Forbid too many (5+) local variables in the unit of code.
- [x] WPS211 — ~~Forbid too many arguments for a function or method.~~ covered by PLR0913
- [x] WPS212 — ~~Forbid placing too many return statements in a function.~~ covered by PLR0911
- [x] WPS213 — ~~Forbid putting too many expressions in a single function.~~ covered by PLR0915
- [ ] WPS214 — Forbid too many methods (7) in a single class.
- [ ] WPS215 — Restrict the maximum number of base classes (3).
- [ ] WPS216 — Restrict the maximum number of decorators (5).
- [ ] WPS217 — Forbid placing too many (5+) await expressions in a function.
- [ ] WPS218 — Forbid placing too many (5+) assert statements into a function.
- [ ] WPS219 — Forbid consecutive expressions with too deep (4+) access level. (e.g. self.attr.inner.wrapper.method.call() — 5 levels deep)
- [x] [WPS220] — Forbid nesting blocks too deep.
    kinda covered by C901?
- [ ] WPS221 — Forbid complex (Jones Complexity > 14) lines.
- [ ] WPS222 — Forbid conditions with too many (4+) logical operators.
- [ ] WPS223 — Forbid too many (3+) elif branches.
- [ ] WPS224 — Forbid too many for statements within a comprehension.
- [ ] WPS225 — Forbid too many (3+) except cases in a single try clause.
- [ ] WPS226 — Forbid the overuse (3+ usages) of string literals.
- [ ] WPS227 — Forbid returning or yielding tuples that are too long.
- [ ] WPS228 — Forbid compare expressions that are too long (4+ items).
- [ ] WPS229 — Forbid try blocks with bodies that are too long (over 1 line).
- [ ] WPS230 — Forbid instances with too many (6+) public attributes.
- [ ] WPS231 — Forbid functions with too much (12+) cognitive complexity.
- [ ] WPS232 — Forbid modules with average cognitive complexity that is too high (8+).
- [ ] WPS233 — Forbid call chains that are too long (3+).
- [ ] WPS234 — Forbid overly complex annotations (3+ levels) see also: https://github.com/best-doctor/flake8-annotations-complexity
- [ ] WPS235 — Forbid from ... import ... with too many (8+) imported names.
- [ ] WPS236 — Forbid using too many (4+) variables to unpack a tuple.
- [ ] WPS237 — Forbids f-strings that are too complex.
    A complex format string is defined as use of any formatted value that is not:
   * the value of a variable
   * the value of a collection through lookup with a variable, number, or string as the key
   * the return value of a procedure call without arguments
- [ ]  WPS238 — Forbids too many (3+) raise statements in a function.


---

_Label `plugin` added by @charliermarsh on 2024-01-05 04:49_

---

_Comment by @apatrushev on 2024-03-19 23:10_

WIP: [repo/branch](https://github.com/strmwalker/ruff/tree/preview/wps), [issues](https://github.com/strmwalker/ruff/issues)

---

_Comment by @sobolevn on 2024-03-27 09:57_

@charliermarsh What's the best way to approach this? We now have a working group of people who are interested in implementing this.

The most important question: do you even plan to include this?
Because WPS is massive. The amount of code to maintain is significant. 
Plus, I don't plan to stop supporting the upstream project, so it is very possible that people will continue to send new PRs and fixes even after the initial version.
(It would be so nice to have this as a plugin 🙏)

The amount of code, docs, and config is very significant maintaince burden.

One more question about the workflow (if you plan to support this):
1. Should we create multiple PRs with each rule?
2. Should we create one big PR with everything at once?

I think that `1.` should be much better, because this way you and the team won't be overwhelmed by the amount of code to review.


---

_Comment by @MichaReiser on 2024-03-27 10:22_

> @charliermarsh What's the best way to approach this? We now have a working group of people who are interested in implementing this.

That's great news! 

We're reworking our rule categorize (see #1774) and the rule acceptance guidelines. That's why I think it's important that someone on the team first goes through the rules and identifies the rules that we consider not a good fit for Ruff or where there's overlap with other rules that need figuring out to avoid unnecessary work. 

From looking at the list (without reading the rules in detail):

* Many of the rules are very opinionated. We want to hold off on adding such rules until #1774 is done because they would be enabled by default for everyone using `--select ALL` (which many believe is Ruff's recommended rule set). We hope to roll out a better default set as part of #1774 that allows us to add opinionated rules (in this case the rules are more about intentionally limiting what you're allowed to use in Python).
* Some of the rules above are centered around complexity. For example, WPS200. It's not a direct overlap, but both measure local complexity and can, e.g., conflict with a rule restricting complexity by the maximum number of lines in a module (not supported by Ruff). We need to figure out how we deal with this overlap. Should complexity be a single, configurable rule where you specify the desired "metric," or is it okay to have multiple rules (if so, how do we avoid fixing cycles)


> One more question about the workflow (if you plan to support this):

I strongly prefer 1. because it makes it easier to triage, review, and allows us to merge rules faster

---

_Comment by @sobolevn on 2024-03-27 10:34_

> Many of the rules are very opinionated.

That's the whole point :)

I think that adding individual rules does not make much sense, because they work as a complex.
https://sobolevn.me/2019/10/complexity-waterfall

This is why I am asking about a plugin or a wrapper :)

---

_Comment by @ashrub-holvi on 2024-11-29 18:03_

To be honest I don't see any issues with disabling not needed rules, it's easy to do (`ruff check --statistics` can be transformed to a piece of config), but now I can't use those rules (as I understand there is some draft) because they are blocked by categorisation and not sure there is any clarity when it will be implemented.

---
