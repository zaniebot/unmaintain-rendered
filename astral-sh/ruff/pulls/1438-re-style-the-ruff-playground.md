```yaml
number: 1438
title: Re-style the Ruff playground
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/playground
created_at: 2022-12-29T15:12:55Z
updated_at: 2022-12-29T16:47:28Z
url: https://github.com/astral-sh/ruff/pull/1438
synced_at: 2026-01-12T05:36:31Z
```

# Re-style the Ruff playground

---

_Pull request opened by @charliermarsh on 2022-12-29 15:12_

_No description provided._

---

_Comment by @squiddy on 2022-12-29 15:21_

Looks great! :tada: 

Have you considered adding the JSON schema to the settings editor? Example: https://microsoft.github.io/monaco-editor/playground.html#extending-language-services-configure-json-defaults

---

_Comment by @charliermarsh on 2022-12-29 15:26_

@squiddy - Hah! I actually just did that before seeing your comment!

---

_Comment by @charliermarsh on 2022-12-29 15:27_

You'll notice I chose to just use a JSON editor for settings. I think it's a little simpler, lets us leverage the schema, and avoids having to build a lot of custom form UI.

---

_Comment by @charliermarsh on 2022-12-29 15:27_

I should probably change the default settings to come from Wasm. Then we wouldn't need the TS codegen step, I think...

---

_Comment by @squiddy on 2022-12-29 15:44_

> You'll notice I chose to just use a JSON editor for settings. I think it's a little simpler, lets us leverage the schema, and avoids having to build a lot of custom form UI.

Yeah, much more sensible for the target audience as well.

As for the default options coming from wasm, I think getting rid of that separate TS file is also one step towards (easier) version selection. One less file next to the wam to worry about.

---

_Comment by @charliermarsh on 2022-12-29 16:47_

Yeah. The last thing for version selection would be the JSON schema. It probably doesn’t make sense to include that in the WASM, but we could just upload a versioned copy on every release.

---

_Comment by @charliermarsh on 2022-12-29 16:47_

I have a few more TODOs to improve the playground, but they’re purely additive.

---

_Merged by @charliermarsh on 2022-12-29 16:47_

---

_Closed by @charliermarsh on 2022-12-29 16:47_

---

_Branch deleted on 2022-12-29 16:47_

---
