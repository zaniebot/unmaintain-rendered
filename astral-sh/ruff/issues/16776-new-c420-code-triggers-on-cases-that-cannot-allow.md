---
number: 16776
title: "New C420 code triggers on cases that cannot allow for `fromkeys`"
type: issue
state: closed
author: facelessuser
labels: []
assignees: []
created_at: 2025-03-16T14:41:26Z
updated_at: 2025-03-16T14:58:53Z
url: https://github.com/astral-sh/ruff/issues/16776
synced_at: 2026-01-10T01:22:58Z
---

# New C420 code triggers on cases that cannot allow for `fromkeys`

---

_Issue opened by @facelessuser on 2025-03-16 14:41_

### Summary

The new code can be helpful, but can also burn people, especially less experienced coders who aren't paying close attention. The new check doesn't take into consideration the context of the default value.

For instance, consider the example below. We have a dict comprehension which ruff will suggest that `fromkeys` should be used.

```py
>>> d = {'a': 0, 'b': 0, 'c': 0}
>>> d2 = {k: [0] for k in d}
>>> d2['a'][0] = 1
>>> d2
{'a': [1], 'b': [0], 'c': [0]}
```

But look what happens if we take its advise:

```py
>>> d = {'a': 0, 'b': 0, 'c': 0}
>>> d2 = dict.fromkeys(d, [0])
>>> d2['a'][0] = 1
>>> d2
{'a': [1], 'b': [1], 'c': [1]}
```

This suggestion should not be made in this case.


### Version

ruff 0.11.0

---

_Comment by @facelessuser on 2025-03-16 14:45_

Additionally, if I try to ignore it, I get: `RUF100 [*] Unused `noqa` directive (unused: `C420`)`.

I'm not sure why.

---

_Comment by @InSyncWithFoo on 2025-03-16 14:55_

I can't reproduce the problem:

```shell
$ ruff check --isolated --select C420 -
d = {}
d2 = {k: [0] for k in d}
All checks passed!
```

The rule [does have a check](https://github.com/astral-sh/ruff/blob/2de8455e43efc55b2ed302c0bdf4e59744338504/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs#L84-L87) for this case and it has been there ever since the rule was added in #9613.

---

_Comment by @facelessuser on 2025-03-16 14:58_

I had two cases that looked similar, and I looked at the wrong one 🤦🏻. This looks to be my mistake. Thanks for verifying.

---

_Closed by @facelessuser on 2025-03-16 14:58_

---
