---
number: 14523
title: RUF035 (unsafe-markup-use) can be noisy and would benefit from a whitelist
type: issue
state: closed
author: ThiefMaster
labels:
  - rule
  - preview
assignees: []
created_at: 2024-11-22T10:32:38Z
updated_at: 2024-12-20T09:36:14Z
url: https://github.com/astral-sh/ruff/issues/14523
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF035 (unsafe-markup-use) can be noisy and would benefit from a whitelist

---

_Issue opened by @ThiefMaster on 2024-11-22 10:32_

There are a few cases where it does not really make sense to get the warning, since it's pretty clearly intentional to avoid escaping, but adding noqa comments in every place where it's used would be very noisy/verbose:

- `Markup(render_template(...))`
- `Markup(_('Have a look at <strong>this</strong> translated string!'))`
- `msg = _('Have a look at <strong>this</strong>!'); Markup(msg)`

So ideally I'd like to have a setting where I can add function names (ideally as import strings, but just names would also be OK most of the time) to be considered safe to have their return value passed to `Markdown` - either directly or via a variable assignment.

In the above case, I'd expect all 3 warnings to disappear by whitelisting `render_template` and `_`.

---

_Comment by @MichaReiser on 2024-11-22 10:47_

@Daverball what are your thoughts on this. I remember that you intentionally decided not to exclude gettext. 


---

_Label `rule` added by @MichaReiser on 2024-11-22 10:47_

---

_Label `preview` added by @MichaReiser on 2024-11-22 10:47_

---

_Comment by @Daverball on 2024-11-22 10:58_

Generally my recommendation in these cases it to encapsulate the behavior that looks unsafe into a new function and use that instead of the original one, that way you only have to ignore errors in those few helper functions.

i.e. define a `render_template_markup` and `translated_markup` (you can configure i18n tooling to pick up additional function names for picking up i18n strings automatically) for those two cases and use those where appropriate. I think that's also good documentation.

Allowing `gettext` and other translation functions to pass through unchecked still has some risk, since the translations themselves could be compromised and that's a lot harder to spot than a compromised string literal. So I would try to avoid including markup in translation strings. That being said, I realize it can be inconvenient to split translations into chunks just for the sake of safety, so I have included markup in translations myself in some occasions.

In either case we could perhaps add an additional setting to define a number of functions that are considered safe to be wrapped in `Markup`, things like sanitization functions and template rendering functions come to mind. But I still think it's generally better to define wrapper functions, since then you don't always have to manually wrap the result. It's also less likely that you will forget to wrap the result this way.

---

_Comment by @Daverball on 2024-11-22 11:03_

Another thing I have done in the past is define a function like this:

```python
@overload
def my_gettext(text: str) -> str: ...

@overload
def my_gettext(text:str, *, markup: Literal[True]) -> Markup: ...

def my_gettext(text: str, *, markup: bool = False) -> str:
    return Markup(gettext(text)) if markup else gettext(text)
```

And then import that as `_` instead of `gettext`.

(Of course you would then also want to add that function to the list of functions that should be treated like `Markup` for RUF035)

---

_Comment by @Daverball on 2024-11-22 15:09_

All in all I'm +0 on adding a whitelist. It can genuinely make sense for some cases and it puts the power to make the decision where to draw the line for safety into the user's hands (you may e.g. decide that you take proper care to secure your translation data, so it's fine to include markup in translations).

But it also comes with the risk of making it too easy to accidentally ignore a large set of patterns that still have some unsafe corner cases and this is a security rule, so a higher level of churn is to be expected. Prompting people to restructure their code a bit, to make it less likely to introduce safety bugs seems like a net-positive to me, but that could also be resolved by recommending against using that whitelist as anything other than a temporary stop-gap measure for functions other than sanitization functions (like `bleach.clean`).

---

_Comment by @ThiefMaster on 2024-11-22 15:10_

I think w/o the whitelist the most likely "solution" people come up with is just ignoring the whole rule... which is not great :)

---

_Comment by @Daverball on 2024-11-22 15:17_

You're not wrong, but giving people a false sense of security isn't great either. A lot of bandit rules are even noisier than this one and don't even try to exclude some easily detectable false positives, so by that measure this rule is actually quite forgiving.

I'm not sure what the correct trade-off is going to be. But I'm certainly not strongly opposed to adding a whitelist, just cautioning that it may weaken the rule's effect.

---

_Comment by @MichaReiser on 2024-11-22 16:40_

I like your suggestion of a wrapper function for new code. But I can see how existing code can't easily be rewritten to use such a wrapper function, which is why an allow-list probably makes sense.

---

_Referenced in [astral-sh/ruff#15076](../../astral-sh/ruff/pulls/15076.md) on 2024-12-20 08:25_

---

_Closed by @dhruvmanila on 2024-12-20 09:36_

---

_Closed by @dhruvmanila on 2024-12-20 09:36_

---

_Referenced in [indico/indico-plugins-cern#192](../../indico/indico-plugins-cern/pulls/192.md) on 2025-01-27 20:38_

---
