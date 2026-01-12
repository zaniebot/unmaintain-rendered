```yaml
number: 7525
title: "Formatter: Add --skip-string-normalization option (parity with Black)?"
type: issue
state: closed
author: mikeckennedy
labels:
  - cli
  - formatter
  - wish
assignees: []
created_at: 2023-09-19T17:28:50Z
updated_at: 2025-10-23T13:23:39Z
url: https://github.com/astral-sh/ruff/issues/7525
synced_at: 2026-01-12T15:54:47Z
```

# Formatter: Add --skip-string-normalization option (parity with Black)?

---

_@mikeckennedy_

Hey folks. I see that Black added --skip-string-normalization ( https://github.com/psf/black/issues/118#issuecomment-393298377 ) but `ruff format --skip-string-normalization` leads to `error: unexpected argument '--skip-string-normalization' found`. Any chance of adding this feature to ruff format?

Alternatively, preferably, have ruff potentially prefer `''` rather than `""` for strings as an option. That is, still normalize strings, but to `''` if specified. 

I saw two other issues include this text, but neither seemed to be related to this actual functionality. Sorry if it's already addressed.

---

_Comment by @charliermarsh on 2023-09-19 17:31_

Hey Michael! We haven't implemented it yet but we're considering it -- there's some discussion here (https://github.com/astral-sh/ruff/discussions/7305). The formatter _does_ support parameterizing on preferred quote style (single vs. double) -- it's not exposed on the CLI or configuration yet (just internally), but will be soon. So the main thing we're trying to figure out is whether the ability to select a preferred quote style obviates the need for `--skip-string-normalization`, or whether there are still important use-cases where `--skip-string-normalization` is required. Any thoughts? What do you tend to use it for?

---

_Label `formatter` added by @charliermarsh on 2023-09-19 17:31_

---

_Comment by @mikeckennedy on 2023-09-19 17:39_

Thanks for the quick thoughts @charliermarsh! On the Black conversation/issue there are lots of times that people mentioned `'string'` has less visual noise than `"string"` and I totally agree and prefer `''` for that reason. 

Another, albeit less common one, that applies to me is RSI issues. I've long had to deal with RSI issues so I'm very careful about how and how much I type (not a huge deal these days but do have to be careful).

When typing `"k"`, you press 5 keys and use two hands. When typing `'k'` you press three and only have to use one hand. Minor but when typing code all day, it adds up.

Those are my two reasons.

If you gave us a way to prefer ' over " for normalization, I would not use `--skip-string-normalization` personally. I do want them normalized but want less visual noise and fewer incentives to type more (striving for parity visually).

---

_Comment by @charliermarsh on 2023-09-19 17:45_

Thanks, that's super helpful. We _are_ planning to expose an option to prefer `'` over `"` (it's already implemented internally, but not yet exposed -- coming in one of the next few releases). We'll probably start there and then see if folks still have a need for `--skip-string-normalization`.

---

_Comment by @mikeckennedy on 2023-09-19 17:48_

Awesome, I'll keep my eye out for it. :)

---

_Comment by @T-256 on 2023-09-19 21:14_

@charliermarsh Should optional features different between Ruff and Black documented? What Ruff has, Black has not, and vice versa.

---

_Comment by @coady on 2023-09-19 22:52_

I would still be interested in the neutral `--skip-string-normalization`. Before black, there was a common (but seemingly forgotten) convention to use `'` for short identifiers and `"` for text. Which was also convenient for text with apostrophes.

---

_Comment by @MichaReiser on 2023-09-20 06:15_

> I would still be interested in the neutral `--skip-string-normalization`. Before black, there was a common (but seemingly forgotten) convention to use `'` for short identifiers and `"` for text. 

Can you give an example for a short identifier where you prefer using `'`? 

> Which was also convenient for text with apostrophes.

Ruff doesn't change the quote if it would lead to more escapes. Ruff preserves the double quotes for `"Ruff doesn't change the quotes of this string"` because rewriting it to use single quotes would require escaping `'` which reduces readability.

---

_Comment by @coady on 2023-09-21 01:32_

> Can you give an example for a short identifier where you prefer using `'`?
```python
if 'id' not in data:
    warnings.warn("Data is missing id field.")
```

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:40_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-25 09:45_

---

_Comment by @charliermarsh on 2023-09-25 14:10_

@mikeckennedy - Just trying to gather data: do you use single quotes for docstrings and multi-line strings? E.g.:

```python
def function(x):
    '''Some docstring.'''
```

Or only for "inline" strings, like `function('argument')`?


---

_Comment by @153957 on 2023-09-25 14:21_

Personally I use triple double quotes for docstrings, and single quotes (including triple single quotes) for most other strings except if the string contains single quotes (and no double quotes) then I use double quotes.

But I might be fine with a preference for single quotes everywhere (so also for docstrings).

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 16:25_

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 16:26_

---

_Label `wish` added by @MichaReiser on 2023-09-27 16:26_

---

_Comment by @charliermarsh on 2023-09-27 16:26_

Our current plan is _not_ to implement this setting as it exists in Black, and first see how far folks can get with our quote-style configuration (see: https://github.com/astral-sh/ruff/issues/7615). If there's still strong demand, we'll of course revisit this flag (and so we'll leave this issue open).

Another potential outcome here is that we add something like `quote-style = "preserve"`, to avoid changing quotes on strings but still reformat unnecessary escape sequences and similar.


---

_Comment by @mikeckennedy on 2023-09-28 16:56_

@charliermarsh Exciting to hear, thanks. For me, I think it's always `"` for multi-line strings for me. I recall some sort of recommendation (at least via PyCharm but where did they get it?) for `"` to be used for doc strings (and kinda all the `""" this style """`). But I don't have strong feelings here.

---

_Comment by @kbd on 2023-10-19 13:28_

+1 for `quote-style = "preserve"`. A habit I picked up a long time ago is to use single quotes for identifiers, like dictionary keys, and use double quotes for strings.

---

_Comment by @MartinCura on 2023-10-25 20:01_

Chiming in as i test the new formatter (awesome work you're doing, btw!):

I'm testing `ruff fmt` in an existing codebase that has `skip-string-normalization = true` and the result is over 150 files changed just because of normalizing strings 😅 

If you ask _me_, i prefer to just normalize strings in all my projects and move on... but on the other hand we have cases like this project i'm working on with a team that doesn't want to bother with this. I _am_ a fan of opinionated tools so i'm not really asking that you implement this, *but* i can see why it's useful for migrating between tools (`black` -> `ruff fmt`) as painlessly as possible. my 2 ¢!

---

_Comment by @charliermarsh on 2023-10-25 20:04_

Thanks @MartinCura -- totally understand, this kind of feedback is really valuable for us as we try to make a decision here :)

---

_Comment by @AndydeCleyre on 2023-10-26 04:52_

I usually use `"` for strings I expect to be printed or seen during the program's use, and `'` for all internal strings, making exceptions to avoid escaping. This will keep some of my projects using black for now.

For triple quoted docstrings, whatever makes the docstring linter happy.

For other triple quoted docstrings, I prefer single quotes because it's less noisy and more obvious what I'm looking at, but it's not important to me.

---

EDIT: Oh yeah, and I use `"` for f-strings.

---

_Comment by @MichaReiser on 2023-10-26 04:58_

One consideration with supporting the option is (we could look at black) what the expected behavior is when using the preview formatting that automatically joins and splits strings to make them fit. We then face the situation that we need to join multiple strings, where each string might use different quotes. In which case at least some sort of normalization becomes necessary.

---

_Comment by @AndydeCleyre on 2023-10-26 05:19_

FWIW those long strings usually fall under the user-facing-English-content category for me, which I associate with double quotes.

---

_Label `cli` added by @MichaReiser on 2023-10-26 23:54_

---

_Comment by @MartinCura on 2023-10-27 15:19_

> but on the other hand we have cases like this project i'm working on with a team that doesn't want to bother with this

small unnecessary update: normalized strings in my repo when no one was looking 👀 😅 

---

_Comment by @mmerickel on 2023-10-27 23:02_

As someone converting some codebases to ruff, I ran into this immediately as we are using skip-string-normalization with black. I personally do not see the need to normalize what quote-style is used by folks and would like the option for a `quote-style = "preserve"` to keep us operating as we were with black. Thanks for this amazing tool regardless!

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-30 00:47_

---

_Comment by @pmhahn on 2023-11-06 09:05_

I inherited some 20y old code, where `"` and `'` are both used. Sadly I'm not the only code owner so I cannot enforce one style. Therefore a `quote-style = "preserve"` would help until the powers above me withdraw their blockage to just move to just one consistent style :disappointed: 

---

_Comment by @chockseswaramurthy on 2023-11-17 14:56_

Adding comment for more data points :) . We also ran into this issue when trying to replace Black with ruff formatter in our 10yr old big’ish Django code base with `skip-string-normalization=true` configured currently. We use both single and double quotes and have preferred single quotes historically but also allow double quotes for some things. Internal consensus is leaning towards preserving existing quote styles to co-exist. From reading above, I think `quote-style = "preserve"` should help us adopt ruff formatter with minimal moving changes. 

---

_Comment by @sciyoshi on 2023-11-22 20:36_

Re-reading the comments in this thread, it doesn't seem like there's been a final decision on what the best approach is. I've opened a PR which adds "preserve" as a quote-style option, which is what we would want in order to adopt ruff format for a codebase which uses `skip-string-normalization`. We don't particularly care about having consistent quotes across the codebase which is why that option is useful for us. Also, maybe "ignore" would be more descriptive than "preserve"?

---

_Comment by @charliermarsh on 2023-11-24 15:28_

FWIW I support adding a `"preserve"` option. @MichaReiser is back next week so will wait to get his reaction before moving forward though.

---

_Comment by @MichaReiser on 2023-11-29 03:53_

Could people who want the `--skip-string-normalization` feature please take a look at https://github.com/astral-sh/ruff/pull/8822 to verify if it satisfies their needs and, if not, leave a comment. Thanks

---

_Closed by @MichaReiser on 2023-12-07 23:59_

---

_Comment by @mmerickel on 2024-01-03 18:30_

I ran this against our large codebase at $work and the two differences from black that I see are:

- This always converts docstrings to use double quotes. Either `"` or `"""` based on what version of single quotes was used before.
- This seems to prefer to change `'''` or `r'''` to double quotes, at least in the examples I saw. Even when not in a docstring.

This isn't what I want (would prefer for it to just not touch the string quoting ever), but not the end of the world. Hopefully the observations are helpful since the feature is still in preview.

---

_Comment by @charliermarsh on 2024-01-03 18:34_

@mmerickel - Thank you, it's super helpful to get feedback like this.

---

_Comment by @stephenfin on 2025-07-02 12:49_

Another difference between ruff and black, to add to @mmerickel's comments, is that `u` prefixes are stripped with ruff. This doesn't generally matter since the `u` prefix is a no-op in Python 3, but it does matter if you're in the unfortunate position of having to maintaining code that still has Python 2.7 support. It's not worth fixing IMO and I'm just noting it here in case anyone else stumbles upon this.

---

_Comment by @spaceone on 2025-10-23 13:23_

FYI: `--skip-string-normalization` can be achieved with:

`ruff format --config 'format.quote-style = "preserve"' .`

---
