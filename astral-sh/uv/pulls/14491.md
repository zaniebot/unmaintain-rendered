```yaml
number: 14491
title: Add detail to settings/environments doc
type: pull_request
state: closed
author: lewis-wf
labels: []
assignees: []
base: main
head: main
created_at: 2025-07-07T19:26:33Z
updated_at: 2025-07-07T23:16:58Z
url: https://github.com/astral-sh/uv/pull/14491
synced_at: 2026-01-10T06:53:02Z
```

# Add detail to settings/environments doc

---

_Pull request opened by @lewis-wf on 2025-07-07 19:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I got a bit stuck today trying to specify an environment for resolution - to resolve a PyPy/CPython conflict, as suggested by a hint in the CLI - but wasn't sure on the grammar.

I am sure I missed a doc somewhere, but I was surprised details on the grammar weren't at the obvious place. 

So, I thought to add it quickly!

## Test Plan

I run the npx prettier command.


---

_@zanieb reviewed on 2025-07-07 20:21_

---

_Review comment by @zanieb on `docs/reference/settings.md`:188 on 2025-07-07 20:21_

This file is generated from the code. We should make any edits there then use `cargo dev generate-all` to update the reference page.

---

_@zanieb reviewed on 2025-07-07 20:21_

---

_Review comment by @zanieb on `docs/reference/settings.md`:203 on 2025-07-07 20:21_

I'm hesitant to complicate this example. Maybe we should put this in the "Resolver" concept documentation instead?

---

_@zanieb reviewed on 2025-07-07 20:22_

---

_Review comment by @zanieb on `docs/reference/settings.md`:188 on 2025-07-07 20:22_

Separately, this seems confusing as we're not using the dependency specifier grammar, but the markers? https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers

---

_@lewis-wf reviewed on 2025-07-07 20:40_

---

_Review comment by @lewis-wf on `docs/reference/settings.md`:188 on 2025-07-07 20:40_

Ah I didn't realise! I'll take a look at this tomorrow :) 

---

_@lewis-wf reviewed on 2025-07-07 20:42_

---

_Review comment by @lewis-wf on `docs/reference/settings.md`:188 on 2025-07-07 20:42_

You're right, the link I've put in is insufficiently specific. I'll update along with moving this to the relevant bit of code as per your other comment 

---

_@zanieb reviewed on 2025-07-07 20:43_

---

_Review comment by @zanieb on `docs/reference/settings.md`:203 on 2025-07-07 20:43_

We actually have this example already https://docs.astral.sh/uv/concepts/resolution/#limited-resolution-environments

---

_@zanieb reviewed on 2025-07-07 20:43_

---

_Review comment by @zanieb on `docs/reference/settings.md`:203 on 2025-07-07 20:43_

Unfortunately we can't / don't cross-link from the reference -> the concepts yet

---

_@lewis-wf reviewed on 2025-07-07 20:44_

---

_Review comment by @lewis-wf on `docs/reference/settings.md`:203 on 2025-07-07 20:44_

Ah this is good to know about! But weirdly in all my searching for some reason this didn't come up...

---

_@lewis-wf reviewed on 2025-07-07 20:47_

---

_Review comment by @lewis-wf on `docs/reference/settings.md`:203 on 2025-07-07 20:47_

Is the issue that linking isn't permitted in the references or that we can't do relative linking within the references?

---

_Comment by @lewis-wf on 2025-07-07 20:53_

Going to close this and give it another go tomorrow now I have the pointers to the correct places in the docs 

---

_Closed by @lewis-wf on 2025-07-07 20:53_

---

_@zanieb reviewed on 2025-07-07 23:16_

---

_Review comment by @zanieb on `docs/reference/settings.md`:203 on 2025-07-07 23:16_

We probably could do it, but they'll be broken links in the Rust doc so we'd need to somehow repair them during generation? We just haven't invested in the infrastructure. 

---
