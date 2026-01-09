---
number: 5019
title: Provide additional configuration for long-line exemptions (f.ex. via one or many regular expressions)
type: issue
state: open
author: exhuma
labels:
  - configuration
assignees: []
created_at: 2023-06-12T07:38:37Z
updated_at: 2023-06-12T15:22:14Z
url: https://github.com/astral-sh/ruff/issues/5019
synced_at: 2026-01-07T13:12:15-06:00
---

# Provide additional configuration for long-line exemptions (f.ex. via one or many regular expressions)

---

_Issue opened by @exhuma on 2023-06-12 07:38_

Some of our docstrings contain raw HTTP examples:

```
GET /looooooooooooooooooooooooooooooooong/uuuuuuuuuuuuuuuuuuuuurl?arg=foooooooooooo HTTP/1.1
Host: example.com
```

`pylint` provides this via `--ignore-long-lines` (https://pylint.readthedocs.io/en/stable/user_guide/configuration/all-options.html#ignore-long-lines): 

<blockquote>
---ignore-long-lines

Regexp for a line that is allowed to be longer than the limit.

Default: ^\s*(# )?<?https?://\S+>?$
</blockquote>

This is already addressed to a degree in #3051 for URLs. Unfortunately this does not cover other valid cases. Those cases are often very domain-specific so it would be nice to have that as a configuration option.

Another example that we run into in our team is docstring examples including SNMP OID paths. They can be very long as well and do not match a URL regex. An example:

```
1.3.6.1.4.1.6527.3.1.2.16.27.1.1.4.14.79.76.79.51.77.98.95.97.114.98.105.116.101.114
```

Having a list of regexes would cover many of such cases. I would suggest having a *list* of regexes instead of a single value. This makes the configuration more readable if you have several patterns.


---

_Label `configuration` added by @charliermarsh on 2023-06-12 15:22_

---
