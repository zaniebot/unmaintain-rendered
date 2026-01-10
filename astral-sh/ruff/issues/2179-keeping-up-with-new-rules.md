---
number: 2179
title: Keeping up with new rules
type: issue
state: closed
author: rassie
labels: []
assignees: []
created_at: 2023-01-25T22:44:45Z
updated_at: 2025-08-24T01:35:36Z
url: https://github.com/astral-sh/ruff/issues/2179
synced_at: 2026-01-10T01:22:40Z
---

# Keeping up with new rules

---

_Issue opened by @rassie on 2023-01-25 22:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

With the velocity of ruff's development and the influx of new rules, it's getting harder to keep up. Are there any plans to help with rules discoverability? If not, I have two suggestions in my mind:

1. Implement "recommended" ruleset (similar to what ESLint is doing). Would probably require generic mix-and-match ruleset functionality across rule collections.
2. Implement a CLI command for listing all implicitely disabled rules (or collections, if all rules in a collection are disabled). Or something like that, not sure whether this would work properly as described. For example, I'd hope to be able to run this command and get the new TYP rules, since I haven't explicitely enabled or disabled them, but not see B rules, since I've disabled them all.

Sorry if I missed an existing issue about this, hope you like the suggestion.

---

_Comment by @charliermarsh on 2023-01-25 22:53_

(Would love more input on this -- any reader, feel free to chime in. (The second thing we could probably do.))

---

_Comment by @Flowake on 2023-01-25 23:17_

It's something that I have also been dealing with at work:

1. After a few weeks I notice that I am 10 releases behind the latest one
2. I update ruff and look at the README to find all the new rules categories to add them.
3. I run ruff on the codebase, look at the new failures and decide which rules to keep (and fix) and which to disable.
4. I'll only upgrade again a few weeks later when there are enough new rules to justify going doing this process again.

For the first point , I think when categorization (#1774) is implemented, it would make sense to create a "recommended" ruleset gathering consensual categories (like "correctness", "style" , "perf" ...).

The second point would definitely make the update process easier and faster.

---

_Comment by @charliermarsh on 2023-01-25 23:18_

The other thing that would help here, possibly, or at least make it feel less overwhelming, is slowing down to a weekly release cadence.

---

_Comment by @Flowake on 2023-01-25 23:56_

> The other thing that would help here, possibly, or at least make it feel less overwhelming, is slowing down to a weekly release cadence.

That would also help regarding #2136 !

---

_Comment by @charliermarsh on 2023-01-25 23:57_

Yeah. It would also mean I could keep a proper changelog, rather than auto-generating from PRs. (Right now, it'd take too much time to maintain a changelog with the current release cadence.)

---

_Comment by @yakMM on 2023-01-27 14:27_

> Implement a CLI command for listing all implicitely disabled rules (or collections, if all rules in a collection are disabled).

I like this idea, it can be useful outside of upgrading as well.

---

_Comment by @Crocmagnon on 2023-01-29 12:38_

We could imagine a strict mode, where all rules have to be explicitly selected or ignored.

---

_Comment by @hofrob on 2023-02-28 11:16_

Commands that show rules (disabled, enabled, all) would be great!

# My 2 cents regarding naming and flags

> Implement a CLI command for listing all implicitely disabled rules (or collections, if all rules in a collection are disabled).

Personally I feel the `rule` sub-command could be extended to list all available and disabled rules. This would fit great into my worflow :wink:. The `linter` sub-command already lists all linters, but it would be nice to have a similar functionality to list all disabled linters.

Regarding naming: 

* `linter` returns a list of linters
* `config` returns a list of configs
* `rule` explains a rule: maybe this should be changed to also return a list of rules by default? 

Then adding flags (like `--disabled/-d`) to those 3 sub-commands could return disabled/enabled linters, configs, rules. 

# FYI: my workflow 

To have an overview over what linters/rules I want to enable and which one's I explicitly disabled (because of the extremely high development speed :grinning: :+1:), my workflow is the following:

* enable all linters: a test checks if one is missing using the command `ruff linter`, so I never miss newly introduced linters
* if we want to disable a rule: put it into the `ignore` section
* if a rule list command is implemented it will continue like this:
  * run ruff with a disabled rule and see if it succeeds: if it does, enable it (with exceptions)
  * additionally I could check if I can disable a pylint rule in my pylint config because ruff already covers it

This way I will always be alerted about new linters and I have to explicitly disable them. Preferably of course I only disable rules, not a whole linter.

---

_Comment by @Avasam on 2023-03-07 02:24_

The way I tend to work in those cases ( I do the same with ESLint and its many plugins). Is that I use presets that enable *everything*. Then disable what I don't like or doesn't fit my/our coding standards, with comments explaining why. Save that in a preset. Disable extra-per-project rules if needed in per-project configurations that extend my base preset. For projects that need stability, I simply pin the linting/formatting dependencies.

So far with Ruff this means:
- `select = ["ALL"]`
- disable about a dozen `flake` things. Idc if I run extra checks given how blazing fast Ruff is.
- share the preset in common project until a better way to share configs exists (like nitpick, eslint, Angular CLI or NX/NRWL does, see: https://github.com/charliermarsh/ruff/issues/809)
- Once in a while something suddenly starts failing in a test project, or I bump linting deps in a stable project, and if there's any new rule that mattered for us, that's when I'd learn about it, because I have to fix it (or turn as a warning with a FIXME comment https://github.com/charliermarsh/ruff/issues/1256 ).

---

_Comment by @steve-mavens on 2023-03-26 17:40_

Supposing that @rassie 's suggestion of a rules-listing command was implemented as `ruff list --not-selected`, perhaps other filters could be applied to the list. For example, `ruff list --not-selected --passing` to mean rules I have implicitly not selected, but which I can safely add. `ruff list --failing` is like `ruff check select=ALL` except that it doesn't tell me where any of the violations occur, just gives me a list of what rules I'm up against :-)

Further supposing that each rule were somehow tagged with the first ruff version it appears in, `ruff list --since 0.0.254` is basically just the "Rules" section of the changelog, so not a valuable new feature. But `ruff list --not-selected --since 0.0.254` is everything new since 0.0.254 **that I'm not getting**. I'd run that when I upgrade ruff off 0.0.254 to the latest.

`ruff list --since 0.0.254 --passing` is new stuff I can add without thinking about it. `ruff list --since 0.0.254 --failing` is all new rules that disagree with my previous life choices. It's better than running `ruff check select=ALL`, because I don't have to think again about rules from pre-0.0.254.

This would also enable a config setting that separates the workflow for getting the latest ruff code from the workflow for getting the latest ruff rules (that match my wildcard selections). For example last week my project was on 0.0.254. This week I upgraded to 0.0.259, because I wanted https://github.com/charliermarsh/ruff/pull/3583 (colours in my Windows terminal)

IIRC this didn't cause my project to fail linting, because there were no new failing rules in the categories I have selected. Indeed, many categories are already stable and will only rarely add new rules. But supposing I want to select "PL", and I haven't previously used PyLint on that code base, then updating ruff is frequently going to find new violations, some of which will be opinionated rules asking for non-trivial code refactor.

Aside from the current rate of change this is no more of an issue for ruff than for (say) mypy. But I shouldn't have to refactor my code, or ideally even need to ignore a bunch of new rules, just to fix terminal colours. Linters often do it, which is why linters often get pinned to exact versions and not updated with everything else: because it amounts to conflating bugfixes with breaking changes.

This may be a niche use case, of course, since quite reasonably you can say, "don't select "PL" yet if you're not already using PyLint: wait until PL is more complete and in the mean time list all the PL rules you want individually".

Still, if it is useful to select unstable categories but get stable behaviour, without your ruff config being massive, then it could look like any of the following:

`rules-version = "0.0.254"` means "wildcards only select rules that existed when I last reviewed which I want to ignore". 

`ignore = [..., "since:0.0.254"]` means the same thing (it looks ambiguous, though, since it apparently might mean "actually ignore everything since 0.0.254, even if I selected it with a non-wildcard").

`select = ["PL:0.0.254"]` means "select all PyLint rules up to the point where I last reviewed which I want to ignore"

`ignore = ["PL:since:0.0.254"]` means the same thing.

All of this assumes that ruff has a nice linear versioning. If in future ruff supported parallel major versions, and backports rules from ruff 2.* to the still-in-maintenance ruff 1.*, then the question of what version a rule first appeared in becomes a bit more complicated than just naming a single version. Still, within the context of each release branch it might be a single version.


---

_Comment by @steve-mavens on 2023-03-26 18:14_

"`ruff list --failing` is like `ruff check select=ALL`" - OK, now I've found `ruff check --statistics`.

---

_Comment by @charliermarsh on 2023-05-10 21:06_

(I'm going to close just in the interest of trying to keep the Issues tab actionable. We could always continue this as a Discussion if folks have other ideas or suggestions.)

---

_Closed by @charliermarsh on 2023-05-10 21:06_

---

_Referenced in [Qiskit/qiskit#10116](../../Qiskit/qiskit/pulls/10116.md) on 2023-05-30 16:28_

---

_Comment by @tylerlaprade on 2023-07-22 23:31_

I would like a way to be notified when new _categories_ of rules are added. Since `ALL` had too many unrelated errors, I've been opting in to categories instead of opting out, and opting out of specific rules within those categories. Now that I've done a full pass of all the categories, I can be confident that I enabled all the ones that seemed relevant to my project, but I don't have a good way to know when a new category needs to be vetted by me.

---

_Comment by @earonesty on 2023-10-25 18:38_

i want a rule for unbounded queues.  it kills me every damn time.

---
