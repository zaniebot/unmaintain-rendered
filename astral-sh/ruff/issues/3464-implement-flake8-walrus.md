```yaml
number: 3464
title: Implement flake8-walrus
type: issue
state: open
author: Cielquan
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-12T17:21:38Z
updated_at: 2025-06-03T17:35:35Z
url: https://github.com/astral-sh/ruff/issues/3464
synced_at: 2026-01-10T11:09:46Z
```

# Implement flake8-walrus

---

_Issue opened by @Cielquan on 2023-03-12 17:21_

# [flake8-walrus](https://pypi.org/project/flake8-walrus/)

Forbids usage of the `:=` walrus operator.

### Error Codes
* [ ] `ASN001` do not use assignment expressions

---

_Label `plugin` added by @charliermarsh on 2023-03-12 17:47_

---

_Comment by @kyoto7250 on 2023-04-06 15:41_

 Personally, I would like to work on implementing this plugin, but is this an appropriate rule to incorporate into `ruff`?

I know there were objections to the introduction of this operator, but I feel this rule is a matter of user preference.

---

_Comment by @MichaReiser on 2023-04-06 16:10_

What's the rational of forbidding the operator (other than `lol` :D). Is it useful to a broad audience of the Python community?

---

_Comment by @Cielquan on 2023-04-06 18:26_

The objection is readability. It may not be the only one, but it is mine.
I totally see that it is somewhat of a nice QoL feature, but I and others too do not want to exchange readability for 1 line of code saved.

And I do not want others to be able to use it in code I maintain, so I forbid its usage via the flake8 plugin by running flake8 in CI.

It is totally subjective preference but so is any style rule somehow, is it not?
But in comparison to PEP8 it is rather niche I guess. So I can see why one would not want to implement it. But nontheless I and probably others too would like to have it being implemented. 😉

---

_Comment by @charliermarsh on 2023-04-06 21:24_

Yeah, I think this is a hard one for us to merge until we have a clearer policy on opinionated rules / plugins (similar to #3463).

---

_Comment by @MichaReiser on 2023-04-11 07:11_

> It is totally subjective preference but so is any style rule somehow, is it not?
But in comparison to PEP8 it is rather niche I guess. So I can see why one would not want to implement it. But nontheless I and probably others too would like to have it being implemented.

Thanks for explaining your motivation. 

We're considering reorganizing the ruff rules and one category that we intend to introduce is `restriction`. Rules in the `restriction` category are opinionated, not recommended for most users, and restrict the allowed language feature. I could see this rule fit into that category if it satisfies the criteria that it is useful to some community users (we don't want to have "I don't like" rules, where *I* is a single person). 

---

_Comment by @Cielquan on 2023-04-11 08:58_

The `restriction` category sounds very nice. I think this will be a very good way to separate those special rules.

Sure, when e.g. I am the only one using it, I would have to wait for plugin support and write my own plugin for this. This makes sense to me.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @MichaReiser on 2024-10-14 12:34_

https://github.com/astral-sh/ruff/issues/13741 is related where the author asked for restricting most, but not all walrus operators.

---

_Comment by @alexprengere on 2025-05-30 17:42_

I have been using [auto-walrus](https://github.com/MarcoGorelli/auto-walrus/) to automatically add walrus operators to a private codebase (lots of XML parsing with lxml), and I found the results really good.

A typical change by that tool looks like this:

```python
# Before
version = get_version(xml)
if version is not None:
    m.version = version

# After
if (version := get_version(xml)) is not None:
    m.version = version
```

Auto-walrus does not "catch" everything, for example this one was manually added by me, not detected by the tool as-is:

```python
# Before
if xml.find("TotalPrice/Total") is not None:
    total = xml.find("TotalPrice/Total")
    flight.price = float(total.text)

# After
if (total := xml.find("TotalPrice/Total")) is not None:
    flight.price = float(total.text)
```

I was about to suggest new rewriting rules to ruff based on auto-walrus.

---

_Comment by @Cielquan on 2025-06-01 11:51_

Well the walrus operator is very dividing. There are people who like it and people who don't. It's a matter of taste after all I think.

I see value in have an optional restriction rule to forbid it's usage, but I also see value in an optional rule like auto-walrus.

I personally used it once in a list comprehension where it made the code simpler in my opinion. But in a general use like in your example I personally don't like it, because I think the code without is easier to read. But that is just my taste and I respect your taste if you like to use it in your code base.

---

_Comment by @Avasam on 2025-06-03 17:35_

> https://github.com/astral-sh/ruff/issues/13741 is related where the author asked for restricting most, but not all walrus operators.

I do feel like that was a different ask. Removing redundant walrus leads to the same come but with 1 less character and autofixable.

Not the same as forbidding/forcing walrus usage where the code would need to be changed by adding or removing a separate assignement.

---
