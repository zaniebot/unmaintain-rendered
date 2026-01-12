```yaml
number: 3294
title: rule for detecting format error in gettext translations
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-03-01T19:25:47Z
updated_at: 2023-03-27T18:03:41Z
url: https://github.com/astral-sh/ruff/issues/3294
synced_at: 2026-01-12T15:54:43Z
```

# rule for detecting format error in gettext translations

---

_@spaceone_

```python
import gettext
_ = gettext.NullTranslations().gettext # ugettext, ngettext
var = 'baz'
# incorrect:
_('foo %s bar' % var)
_('foo {} bar'.format(var))
_(f'foo {var} bar')
# correct:
_('foo %s bar') % var
_('foo {} bar').format(var)
```

those 3 cases should be detected in a new rule:
→ calls to `_` (the conventional name of gettext) should only be done with a contant string as argument.

---

_Comment by @charliermarsh on 2023-03-01 21:53_

Is the error that you're _supposed_ to leave the parameters unfilled?

---

_Label `question` added by @charliermarsh on 2023-03-01 23:07_

---

_Comment by @charliermarsh on 2023-03-01 23:08_

(Seems like a reasonable rule, I'm just not familiar with the API.)

---

_Comment by @Jackenmen on 2023-03-02 08:32_

The error is that you're supposed to use constant strings, not dynamic ones as that passed string is used as a key during the lookup in message catalog.

I think that ideally this should support not just the most basic gettext call accepting a single argument but also calls to gettext, ngettext, and so on:
https://docs.python.org/3/library/gettext.html

---

_Comment by @spaceone on 2023-03-02 08:42_

thanks for the feedback, I clarified the initial description.

---

_Comment by @charliermarsh on 2023-03-02 15:26_

Thank you!

---

_Label `question` removed by @charliermarsh on 2023-03-02 15:42_

---

_Label `rule` added by @charliermarsh on 2023-03-02 15:42_

---

_Closed by @charliermarsh on 2023-03-27 18:03_

---
