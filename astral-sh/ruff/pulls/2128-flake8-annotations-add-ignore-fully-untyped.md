```yaml
number: 2128
title: "flake8-annotations: add ignore-fully-untyped"
type: pull_request
state: merged
author: akx
labels:
  - configuration
  - typing
assignees: []
merged: true
base: main
head: ann-allow-untyped-return-if-untyped
created_at: 2023-01-24T13:59:57Z
updated_at: 2023-02-07T16:36:04Z
url: https://github.com/astral-sh/ruff/pull/2128
synced_at: 2026-01-12T15:55:07Z
```

# flake8-annotations: add ignore-fully-untyped

---

_@akx_

This PR adds a configuration option to inhibit ANN* violations for functions that have no other annotations either, for easier gradual typing of a large codebase (looking at you, Home Assistant 😁).

---

_@charliermarsh reviewed on 2023-01-24 14:23_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/converter.rs`:189 on 2023-01-24 14:23_

Generally, we only need to add these if the source plugin defines them. But I don't _think_ this option exists in `flake8-annotations`, does it?

---

_Comment by @charliermarsh on 2023-01-24 14:25_

Would functions to which this applies not trigger ANN100 et al for each untyped argument? What's the configuration that would make this useful? Suppressing ANN100 plus enabling this to allow fully untyped functions + require return types for any partially typed functions?

---

_Comment by @akx on 2023-01-24 14:45_

Replying to both comments here 😄 

> Generally, we only need to add these if the source plugin defines them. But I don't _think_ this option exists in `flake8-annotations`, does it?

I don't think it does, that's right, but I feel this is an useful check to make ANN2* less blanket-y. See below...

> What's the configuration that would make this useful?

I should have probably mentioned this in the PR proper, but this is meant as a replacement for [this pylint plugin in Home Assistant](https://github.com/home-assistant/core/blob/0530f61373395db83b016173399b06eb8337a39a/pylint/plugins/hass_constructor.py): ('`__init__` should have explicit return type "None"' – Used when `__init__` has all arguments typed but doesn't have return type declared).

In particular, enabling ANN204 on Home Assistant's repo right now yields oodles (1108 in particular) of these warnings for files that haven't been fully strictly typed yet; this option makes that number, well, 1. 😂 

(The other (bad, cumbersome) option I see would be to allow setting `per-file-select` or similar, and configure it to align with https://github.com/home-assistant/core/blob/dev/.strict-typing.)

---

_@charliermarsh reviewed on 2023-01-24 14:47_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/converter.rs`:189 on 2023-01-24 14:47_

Oh sorry, what I meant was: we only need to add this logic to flake8-to-ruff, if it exists in the original plugin. Since, otherwise, we'll never run into them in `.flake8` files!

---

_Review comment by @akx on `src/flake8_to_ruff/converter.rs`:189 on 2023-01-24 14:49_

Ah, right, yeah 😄 I based this on `mypy_init_return` and looked for all the places that worked with it. I'll remove this!

---

_@akx reviewed on 2023-01-24 14:49_

---

_Comment by @charliermarsh on 2023-01-24 14:58_

Ah, okay, yeah, I see. That makes sense.

(In practice, this would be coupled with disabling the ANN00-level rules, right? Again, just confirming my understanding.)

---

_Comment by @akx on 2023-01-24 15:01_

> (In practice, this would be coupled with disabling the ANN00-level rules, right? Again, just confirming my understanding.)

Yep, my idea is to enable just ANN204 with this configuration for now to emulate what the pylint plugin does. 

---

_Comment by @charliermarsh on 2023-01-26 23:46_

I wonder if this should be made into a more general setting: if a definition doesn't have _any_ types, suppress all `ANN` rules. That way, you wouldn't need to turn off the `ANN00*` rules. But they would get flagged if you had a return type, but not argument types.

---

_Comment by @akx on 2023-01-27 15:53_

@charliermarsh Yeah, that sounds good too. `tool.ruff.flake8-annotations.all-or-nothing = true`? 😁 

---

_Comment by @charliermarsh on 2023-01-27 16:16_

`ignore-fully-untyped`?

---

_@charliermarsh requested changes on 2023-01-29 01:22_

(Just requesting changes to help me manage my queue, no rush.)

---

_Renamed from "flake8-annotations: add allow_untyped_return_if_untyped" to "flake8-annotations: add allow-untyped-return-if-untyped & ignore-fully-untyped" by @akx on 2023-02-02 12:06_

---

_Review requested from @charliermarsh by @akx on 2023-02-02 12:06_

---

_@charliermarsh reviewed on 2023-02-02 12:59_

---

_Review comment by @charliermarsh on `src/rules/flake8_annotations/rules.rs`:516 on 2023-02-02 12:59_

I was hoping we could just have this setting, and not `allow_untyped_return_if_untyped` :joy: What's an example of where `allow_untyped_return_if_untyped` would be useful but you'd want `ignore_fully_untyped` to be kept false?

---

_Review comment by @akx on `src/rules/flake8_annotations/rules.rs`:516 on 2023-02-02 20:17_

Ah yeah, you're right, `ignore_fully_untyped` alone should be enough.

---

_@akx reviewed on 2023-02-02 20:17_

---

_Renamed from "flake8-annotations: add allow-untyped-return-if-untyped & ignore-fully-untyped" to "flake8-annotations: add ignore-fully-untyped" by @akx on 2023-02-02 20:21_

---

_Converted to draft by @akx on 2023-02-02 20:25_

---

_Comment by @charliermarsh on 2023-02-04 21:27_

Is this ready for review?

---

_Marked ready for review by @akx on 2023-02-07 15:35_

---

_Comment by @akx on 2023-02-07 15:36_

@charliermarsh Yeah, sorry, it's ready for review now! 😄 I'm not sure why the mypy_init_return snapshot changed though...

---

_@charliermarsh approved on 2023-02-07 16:35_

---

_Merged by @charliermarsh on 2023-02-07 16:35_

---

_Closed by @charliermarsh on 2023-02-07 16:35_

---

_Label `configuration` added by @charliermarsh on 2023-02-07 16:36_

---

_Label `typing` added by @charliermarsh on 2023-02-07 16:36_

---
