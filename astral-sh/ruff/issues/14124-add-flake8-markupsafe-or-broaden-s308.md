```yaml
number: 14124
title: Add flake8-markupsafe or broaden S308
type: issue
state: closed
author: Daverball
labels:
  - rule
assignees: []
created_at: 2024-11-06T08:10:11Z
updated_at: 2024-11-11T18:30:04Z
url: https://github.com/astral-sh/ruff/issues/14124
synced_at: 2026-01-12T15:54:53Z
```

# Add flake8-markupsafe or broaden S308

---

_@Daverball_

Improper use of `markupsafe.Markup` is a common source of XSS vulnerabilities and I've seen many people, including myself, make mistakes here regardless of how experienced they are, in part due to f-string's convenience and sometimes due to being API-compatible with regular `str`, so it's easy to write `Markup(''.format(unsafe))` instead of `Markup('').format(safe)`, this particular error can also be quite tricky to spot depending on the formatting.

There is an existing [flake8 plugin](https://github.com/vmagamedov/flake8-markupsafe), however it was never published on pypi and it makes a questionable exception for i18n.

Providing support for additional classes that behave like `Markup` on the other hand, like the `webhelpers` `literal` (which is a subclass of `Markup`) with a user-configurable list of fully qualified names seems sensible, since there's also `flask.Markup` which is an alias for `markupsafe.Markup`.

There's an existing issue to either add a rule or extend the existing `B308` rule in bandit, although it never went anywhere. So it could make sense to extend `S308` in ruff instead, or even just add a new `RUF` rule, rather than use up the `MS` prefix for a single rule.

In any case, the implementation should be quite simple and I'd be happy to provide one if you're open to adding a rule for it or augment the existing bandit rule.

---

_Label `rule` added by @MichaReiser on 2024-11-06 12:17_

---

_Comment by @sbrugman on 2024-11-07 12:07_

Thanks for the clear writeup!

One aspect to take into consideration is that mapping this implementation to `flake8-markupsafe` then that's a nice way of providing attribution to the original author (which I believe is crucial in open-source software).

I could not find a single open-source repository using this plugin, so we can expect no or very few users of the original `MS001` code. 

@Daverball there is an effort going on to recategorise the rules, and this rule would be a valuable addition to the `security` rules (#1774). I'd encourage you to go ahead with the implementation!

Note that [mako](https://github.com/astral-sh/ruff/issues/3653) and jinja files are currently not supported.



---

_Closed by @MichaReiser on 2024-11-11 18:30_

---
