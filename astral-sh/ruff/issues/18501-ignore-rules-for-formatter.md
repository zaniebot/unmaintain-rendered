```yaml
number: 18501
title: Ignore rules for formatter
type: issue
state: closed
author: dornech
labels:
  - configuration
  - formatter
  - needs-info
assignees: []
created_at: 2025-06-06T15:54:44Z
updated_at: 2025-06-11T07:17:01Z
url: https://github.com/astral-sh/ruff/issues/18501
synced_at: 2026-01-12T15:54:56Z
```

# Ignore rules for formatter

---

_@dornech_

### Summary

Following the discussion https://github.com/astral-sh/ruff/discussions/18486 here I would like to suggest to have a formatter config setting allowing to ignore formatting rules. 

As mentioned in the linked discussion already I am sometimes not too happy with the PEP 8 rules for empty lines because I find the rules too strict to organize code. Especially the pcodestyle rule for empty lines and here E303 are sometimes not really helpful to override them with this # fmt on/off stuff.

I wonder if somebody else has same feeling :-)

---

_Label `configuration` added by @ntBre on 2025-06-06 16:25_

---

_Label `suppression` added by @ntBre on 2025-06-06 16:25_

---

_Label `formatter` added by @ntBre on 2025-06-06 16:25_

---

_Comment by @MichaReiser on 2025-06-06 16:39_

Hi @dornech 

Thanks for opening this issue. Unfortunately, it's a bit too broad, making it hard to understand what kind of option you're looking for. 

Can you be more specific: What option do you want, how would it change formatting?

---

_Label `needs-info` added by @MichaReiser on 2025-06-06 16:39_

---

_Label `suppression` removed by @MichaReiser on 2025-06-06 16:40_

---

_Comment by @dornech on 2025-06-06 17:52_

Sorry for not being too specific. In general I would appreciate a formatter option similar to the linter option to IGNORE specific rules. I think that might make sense for a limited number of rules.

As such currently I feel limited by the pcodestyle rule E303. Other way round: I would like to have sometimes three empty lines to structure code better where other options like splitting in separate modules is not preferable. Additional dummy comment lines do not make it better either.

---

_Comment by @MichaReiser on 2025-06-07 13:07_

> Sorry for not being too specific. In general I would appreciate a formatter option similar to the linter option to IGNORE specific rules.

As mentioned in the discussion. This isn't something we plan on doing. We're open to discuss specific formatting options. 

> As such currently I feel limited by the pcodestyle rule E303. Other way round: I would like to have sometimes three empty lines to structure code better where other options like splitting in separate modules is not preferable. Additional dummy comment lines do not make it better either.

I think it's fair to discuss whether the formatter should be less strict about empty line enforcement. For example, that it doesn't enforce 2 empty lines between functions or classes. This way, you could use one empty line for functions that belong "together" and 2 empty lines to separate the functions that you would otherwise put into a separate file.

I don't think allowing 3 empty lines has clear benefits. It only results in less consistent code.

---

_Closed by @MichaReiser on 2025-06-11 07:17_

---
