```yaml
number: 9354
title: "[Question] A status page for all implemented plugins and rules?"
type: issue
state: open
author: mikaelarguedas
labels: []
assignees: []
created_at: 2024-01-02T10:56:16Z
updated_at: 2024-01-05T09:34:06Z
url: https://github.com/astral-sh/ruff/issues/9354
synced_at: 2026-01-10T11:09:51Z
```

# [Question] A status page for all implemented plugins and rules?

---

_Issue opened by @mikaelarguedas on 2024-01-02 10:56_

Hey there :wave:

Thanks for this great project!

I'm exploring the idea of migrating some projects from flake8 to ruff.
I'm currently having a hard time seeing exactly all the flake8 rules I rely on v.s. the ones implemented in ruff.

Apologies if this is the wrong venue for this or duplicating lots of existing discussions on that topic... happy to be redirected to the right place


My current approach:
- for each flake8 plugin I use, find the relevant section in the [ruff rules](https://docs.astral.sh/ruff/rules)
  - side-by-side compare the rules provided by the latest version of the coresponding flake8 plugin
  - for the missing ones flag them to check later in ruff issues or other existing ruff rules if they are: already covered by other ruff rules, obsolete and wont be implemented, to be implemented
 
The process seems tedious, manual and error prone.
Is there a central place where one can find:
- a side by side comparison of flake8-plugin behavior (at a given version) and ruff rules
  - for the ones not implemented: a status or rationale

This would help a lot figuring out how ruff would behave compared to a given flake8 set of plugins / versions for a given project and also update the list of rules to be implemented when new versions of plugins come out.

Thanks!

---

_Comment by @akx on 2024-01-05 09:28_

I've been thinking of implementing something like this to (try to) cover all of the reimplemented rules, but by golly do I have too many side projects...

For context (and prior art), 

* my [pylint-to-ruff](https://github.com/akx/pylint-to-ruff) project currently parses the list from the issue #970 and hopes for the best
* I think [flake8-to-ruff](https://pypi.org/project/flake8-to-ruff/) <del>just relies on its own tribal knowledge (since it's built from this repo afaiu), etc. (It might be a good idea to port it to Python instead?)</del> was retired in #9329, so it might be worth to reimplement it in Python as a separate project that'd use this same knowledge.

It would certainly be useful if all of this was in one machine-readable place 😄 


---
