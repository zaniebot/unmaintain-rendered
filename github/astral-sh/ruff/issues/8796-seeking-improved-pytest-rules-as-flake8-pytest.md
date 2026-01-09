---
number: 8796
title: "Seeking improved pytest rules, as `flake8-pytest-style` doesn't align with best practices"
type: issue
state: open
author: RonnyPfannschmidt
labels:
  - rule
assignees: []
created_at: 2023-11-20T21:00:09Z
updated_at: 2025-03-24T01:29:42Z
url: https://github.com/astral-sh/ruff/issues/8796
synced_at: 2026-01-07T13:12:15-06:00
---

# Seeking improved pytest rules, as `flake8-pytest-style` doesn't align with best practices

---

_Issue opened by @RonnyPfannschmidt on 2023-11-20 21:00_

hi everyone,

before i evaluated ruff i wasn't even aware of flake8-pytest-styles,
as one of the pytest maintainers i'm particularly annoyed as the rules don't reflect documented and communicated best practices and the upstream doesn't seem likely to change those details 


* `PT001` should be reversed (drop brackets)
* `PT004` is simply incorrect - public no return value fixtures should have public names for both usage in the usefixtures maker and as dependencies in other fixtures
* `PT005` is equally incorrect - internal fixtures return values even if underscores are used - sabotage of that is not ok
* `PT006` undoes the intentionally established pattern of being able to comma separate multiple names in a single string
* `PT009` sabotages intentional usage of unittest (which is sometimes necessary)
* `PT021` sabotages well established patterns for complex fixtures that setup in phases where on needs to ensure teardown but ought not to split them into multiple smaller fixtures
* `PT023` should drop parens on marks by default
* `PT024/25` should warn on any mark on a fixture as no marks are supported on fixtures
* `PT027`also sabotages legitimate usage of unittest patterns

---

_Comment by @ThiefMaster on 2023-11-20 21:32_

As a user who just started adding ruff to his (somewhat big) codebase I also added config to change the weird defaults WRT parentheses because the defaults were quite weird!

Just some thoughts on your points:

- `PT006`: Maybe [parametrize-names-type](https://docs.astral.sh/ruff/settings/#flake8-pytest-style-parametrize-names-type) (and possibly the other settings related to parametrize as well?) should allow a list of valid types (e.g. `['csv', 'tuple']` if you don't want lists), and when applying a fix pick the first one in the list (to avoid an extra setting to set the preferred type)?
- `PT009`: I guess this is a matter of old existing vs new/modern codebase? On the latter I absolutely want this enabled because that way someone not familiar with the nice pytest features will immediately get a linter warning if they try to use unittest-style assertions. Personally I'd simply disable it in codebases that make use of it, but I think "supporting legacy stuff" should not be a default... I'm curious, when is using the legacy assertions actually necessary? (My first thought there was "imagine if this was simply autofixable")
- `PT021`: Would it make sense if this rule only triggered when there's just a single `request.addfinalizer()` call in a fixture? I guess for most applications it's not common to have truly complex fixtures, so it nicely covers the common cases of someone simply not being aware that they can use `yield` in their fixtures.

---

_Comment by @ofek on 2023-11-20 21:43_

Agreed, except:

- `PT006` - please keep this as-is, I think it is perfect and wherever I have worked people got confused about the types and for multiple prefer the tuple just until now there was no way to enforce it
- `PT009`/`PT021`/`PT027` - please keep these as-is, there should be a desire to encourage the "best way" to do things as that is the entire purpose of tools like this

---

_Comment by @RonnyPfannschmidt on 2023-11-20 22:34_

in that case there should also be a suggestion to drop `TestCase` base class and xunit setup methods
imho `PT009/PT021/PT027`should group under a unittest2pytest style thing - and perhaps take some more code off https://github.com/pytest-dev/unittest2pytest

---

_Label `rule` added by @charliermarsh on 2023-11-21 01:35_

---

_Comment by @RonnyPfannschmidt on 2023-11-21 08:54_

perhaps it makes sense to trigger the pytest migration rules based on whether a pytest import is in a test file,

a very neat but expensive way would be to have "migraton" rules/fixes only apply when the line is currently being changed anyway

---

_Comment by @ThiefMaster on 2023-11-21 09:20_

> a very neat but expensive way would be to have "migraton" rules/fixes only apply when the line is currently being changed anyway

I would not like that in my own project; It leaves an inconsistent mess and makes PRs harder to review because now they suddenly contain no longer just the main change they are about but also 'infrastructure changes'. I prefer a separate PR for those (which can be more easily skimmed over during review)

> perhaps it makes sense to trigger the pytest migration rules based on whether a pytest import is in a test file 

but many test files don't contain any pytest imports if they don't use parametrization...

---

_Comment by @eli-schwartz on 2023-11-22 23:22_

> before i evaluated ruff i wasn't even aware of flake8-pytest-styles,
> as one of the pytest maintainers i'm particularly annoyed as the rules don't reflect documented and communicated best practices and the upstream doesn't seem likely to change those details

I do not use flake8-pytest-style anyway, but. I did notice that it is recommended at https://docs.pytest.org/en/stable/explanation/goodpractices.html#checking-with-flake8-pytest-style

Should it be removed?

---

_Comment by @RonnyPfannschmidt on 2023-11-23 05:13_

Good point

It should 

---

_Comment by @charliermarsh on 2023-11-24 15:35_

FWIW, I have no problem with shipping better pytest rules (though I haven't used pytest extensively so don't feel qualified to have good opinions on the rules themselves). In practice, it may be easiest to ship them under an entirely new pytest prefix / rule set, and mark the `flake8-pytest-style` rules as deprecated, but we can make that decision later.

---

_Comment by @RonnyPfannschmidt on 2023-11-24 16:33_

I'll coordinate with pytest-core to get together a official set 

---

_Comment by @ofek on 2023-11-24 16:43_

FYI until such time that there is an official set, for Hatch's new `fmt` command I'm going to by default ignore rules `PT001`, `PT004`, `PT005`, and `PT023`, while enabling the rest.

---

_Comment by @Mogost on 2023-11-28 10:18_

PT001, PT023 styles are configurable by https://docs.astral.sh/ruff/settings/#flake8-pytest-style-fixture-parentheses and https://docs.astral.sh/ruff/settings/#flake8-pytest-style-mark-parentheses

---

_Referenced in [ipython/traitlets#893](../../ipython/traitlets/pulls/893.md) on 2023-11-30 14:07_

---

_Renamed from "requesting alternative rules for pytest as the flake8-pytest-style are neiither coordinated with pytest not considered best practice" to "Seeking improved pytest rules, as `flake8-pytest-style` doesn't align with best practices" by @dhruvmanila on 2023-11-30 22:26_

---

_Referenced in [pypa/hatch#1084](../../pypa/hatch/pulls/1084.md) on 2023-12-05 17:33_

---

_Referenced in [bakdata/kpops#391](../../bakdata/kpops/pulls/391.md) on 2023-12-08 10:08_

---

_Comment by @karolinepauls on 2024-01-09 08:38_

Would it be possible to disable the most offending rules by default for the time being?

---

_Comment by @ofek on 2024-01-09 15:37_

If you would prefer to have a large default selection of "good" rules, you can use Hatch https://hatch.pypa.io/latest/config/static-analysis/

The undesirable ones that you speak of here are excluded by default.

---

_Comment by @jhbuhrman on 2024-04-15 15:31_

> as one of the pytest maintainers i'm particularly annoyed as the rules don't reflect documented and communicated best practices and the upstream doesn't seem likely to change those details

I understand to a large extent what you mean. But the way I see it is that there is a difference between the _possibilities_ of a programming language, a (testing) framework, or a library (API), and the _agreements_ a team wants to make about a uniform usage of those assets.

As an example, I can understand that a team agrees to _always_ use a decorator _with_ a set of parentheses, even when there are no additional parameters to provide in a particular case, or that a team _always_ uses the list (or tuple) form in specifying the parameter names in `@pytest.mark.parametrize` instead of specifying them separated by commas in a single string. So I don't see it as a [Bad Thing](http://www.catb.org/jargon/html/B/Bad-Thing.html) that these rules exist, ready to be used by any team that wants to do so.

OTOH, I didn't dive in all the particular rules, but advocating `PT004` / `PT005` as a by `pytest` advocated practice is incorrect IMHO.

BTW, I think a know where these two rules come from: my line of reasoning is as follows:
* if you need to use a fixture having a side effect but without a useful return value, the fixture will probably return `None`.
* The developer needs the side effect, so specifies the fixture name as a parameter in their test function, or in another fixture, so a `usefixture` decorator is not possible.
* Other linters will start complaining that the parameter value is not used in the function.
* There is a "convention" that parameters starting with an underscore are a hint to some linters that it is OK that the value is not being used.

I assume that this might be the line of reasoning behind `PT004` / `PT005`.

Unfortunately the rule is implemented in a wrong way. Instead of checking the _name of the decorated function_, the rule should check the _name of the fixture_. This _could_ be the name of the function, but can also be different if the `fixture` decorator has a `name=` parameter, or when `pytest_bdd.given` for example has a `target_fixture=`  parameter.

---

_Comment by @bthorsted on 2024-05-28 09:34_

@charliermarsh FYI the latest version of flake8-pytest-style (v2.0.0), which was shipped on april 1st, inverted the defaults for PT001 and PT023 so that they now conform with pytest recommendations. Maybe ruff should push out a similar change soon?

---

_Comment by @PhorstenkampFuzzy on 2024-06-28 12:45_

If you do establish a new set of rules around pytest. A rule that warns users on the use of the `@pytest.mark.usefixtur` and demands the `@pytest.mark.usefixturs`. I know of a few instances where debugging this caused A LOT of work.

---

_Referenced in [astral-sh/ruff#12106](../../astral-sh/ruff/pulls/12106.md) on 2024-06-29 20:40_

---

_Referenced in [HypothesisWorks/hypothesis#4050](../../HypothesisWorks/hypothesis/pulls/4050.md) on 2024-07-17 20:12_

---

_Comment by @avilaton on 2024-08-10 01:26_

If you are using pants (pantsbuild.org) then rule PT004 and PT005 will really hurt. We had a session scoped fixture which for changed from `django_db_modify_db_settings` into `_django_db_modify_db_settings` which then caused it to be ignored and was VERY HARD to figure out later. Rule PT004 is a menace IMO.

---

_Comment by @RonnyPfannschmidt on 2024-08-10 08:36_

@charliermarsh based on this detail i think a release that declares them unsafe or removes them ougth to be expedited

---

_Comment by @charliermarsh on 2024-08-10 12:06_

@RonnyPfannschmidt -- Neither of these rules have a fix though -- those changes must've been applied manually by the author.

---

_Comment by @charliermarsh on 2024-08-10 12:12_

I'd be fine to deprecate PT004 and PT005 but it will have to wait until the next minor release (we don't deprecate rules in patch releases IIRC).

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-10 13:21_

---

_Comment by @PhorstenkampFuzzy on 2024-08-11 20:06_

Could also a rule be added. I have sometimes seen a problem with `pytest.mark.usefixutres` vs. `pytest.mark.usefixture`.
Could a roule be added that forbides the later?


---

_Comment by @charliermarsh on 2024-08-11 20:30_

Current plan is to deprecate PT004 and PT005 in the upcoming v0.6.

---

_Referenced in [astral-sh/ruff#12837](../../astral-sh/ruff/pulls/12837.md) on 2024-08-12 09:49_

---

_Referenced in [astral-sh/ruff#12838](../../astral-sh/ruff/pulls/12838.md) on 2024-08-12 10:08_

---

_Comment by @MichaReiser on 2024-08-13 15:56_

The upcoming 0.6 release deprecates PT004 and PT005 and changes the defaults for PT001 and PT023.

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-13 15:56_

---

_Referenced in [OSGeo/grass#4520](../../OSGeo/grass/pulls/4520.md) on 2024-10-14 17:38_

---

_Referenced in [python-poetry/poetry#9748](../../python-poetry/poetry/pulls/9748.md) on 2024-11-01 10:25_

---

_Comment by @bje- on 2025-03-24 01:15_

I'm not a big fan of PT009. I want to use `unittest` as it's in the Python standard library and doesn't require any additional packages. I'd rather write my tests using `unittest` and then be free to choose a suitable driver. It seems unsatisfactory to just switch everything to `pytest`. Happy to receive comments on this approach!

---

_Comment by @eli-schwartz on 2025-03-24 01:29_

I have a similar outlook but also I don't enable `PT*`

---

_Referenced in [opensafely-core/opencodelists#2502](../../opensafely-core/opencodelists/pulls/2502.md) on 2025-05-07 17:15_

---
