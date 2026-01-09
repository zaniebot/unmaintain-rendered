---
number: 16767
title: blind-except (BLE001) is not active at all
type: issue
state: closed
author: dolfandringa
labels:
  - documentation
  - question
assignees: []
created_at: 2025-03-15T18:50:49Z
updated_at: 2025-03-15T19:26:26Z
url: https://github.com/astral-sh/ruff/issues/16767
synced_at: 2026-01-07T13:12:16-06:00
---

# blind-except (BLE001) is not active at all

---

_Issue opened by @dolfandringa on 2025-03-15 18:50_

### Summary

According to [the docs, blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/) says that `except Exception` is bad, which I agree with. But it looks like ruff by default doesn't actually check for blind-except at all. Is this intentional? If there are errors that ruff does not check for by default, it would make sense to me to document with those errors, that they are off by default.

This test.py file does not get flagged by ruff as being invalid.
```
a = 1
b = 0
try:
    print(a / b)
except Exception as e:
    print(e)

try:
    print(a / b)
except Exception:
    print("error")

try:
    print(a / b)
except BaseException:
    print("error2")
```

```
❯ ruff --version
ruff 0.11.0
❯ ruff check test.py
All checks passed!
```

While if I turn it into this:

```
a = 1
b = 0
try:
    print(a / b)
except Exception as e:
    print(e)

try:
    print(a / b)
except Exception:
    print("error")

try:
    print(a / b)
except:
    pass
```

it does get flagged as a bare except:
```
❯ ruff check test.py
test.py:15:1: E722 Do not use bare `except`
   |
13 | try:
14 |     print(a / b)
15 | except:
   | ^^^^^^ E722
16 |     pass
   |

Found 1 error.
```

And running ruff with selecting BLE001 explicitely also flags it:
```
❯ ruff check test.py --select BLE001
test.py:5:8: BLE001 Do not catch blind exception: `Exception`
  |
3 | try:
4 |     print(a / b)
5 | except Exception as e:
  |        ^^^^^^^^^ BLE001
6 |     print(e)
  |

test.py:10:8: BLE001 Do not catch blind exception: `BaseException`
   |
 8 | try:
 9 |     print(a / b)
10 | except BaseException:
   |        ^^^^^^^^^^^^^ BLE001
11 |     print("error")
   |

Found 2 errors.
```

### Version

0.3.7 and 0.11.0

---

_Renamed from "blind-except (BLE001) false negative with `except Exception`" to "blind-except (BLE001) is not active at all" by @dolfandringa on 2025-03-15 19:02_

---

_Comment by @dolfandringa on 2025-03-15 19:15_

Ok, this is as intended.  I am just surprised this is not on by default. Also,  i think it makes sense to document what rules are on by default with the rules. Now it's really tedious to search through the default config and then the list of rules what is and isn't on by default. But that's a different issue.

---

_Closed by @dolfandringa on 2025-03-15 19:15_

---

_Comment by @ntBre on 2025-03-15 19:25_

Only a pretty small set of rules is enabled by default. See the [rule documentation](https://docs.astral.sh/ruff/rules/#rules):

> By default, Ruff enables Flake8's F rules, along with a subset of the E rules, omitting any stylistic rules that overlap with the use of a formatter, like ruff format or [Black](https://github.com/psf/black).

You can also run `ruff check --show-settings` and look for the `linter.rules_enabled` section. That shows all of the enabled rules with their names and codes.

I think some people also like to use the special `ALL` rule code to select all rules and then [`ignore`](https://docs.astral.sh/ruff/settings/#lint_ignore) the ones they don't want. If you want to enable a large number of rules, that could be easier than selecting them all.

Maybe it would be nice if we added an icon for default rules in the main rules page, though.

---

_Label `documentation` added by @ntBre on 2025-03-15 19:26_

---

_Label `question` added by @ntBre on 2025-03-15 19:26_

---
