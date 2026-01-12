```yaml
number: 10565
title: "High-level project overview and contributing guide for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
assignees: []
merged: true
base: main
head: jane/server/contributing
created_at: 2024-03-25T10:23:13Z
updated_at: 2024-03-26T06:08:38Z
url: https://github.com/astral-sh/ruff/pull/10565
synced_at: 2026-01-12T15:55:32Z
```

# High-level project overview and contributing guide for `ruff server`

---

_@snowsignal_

## Summary

This PR adds an overview and roadmap to the `README.md` for the `ruff_server` crate along with a rudimentary `CONTRIBUTING.md` that explains some of the technical decisions behind the project and basic information about local testing. 

---

_Label `documentation` added by @snowsignal on 2024-03-25 10:23_

---

_Review comment by @VascoSch92 on `crates/ruff_server/CONTRIBUTING.md`:8 on 2024-03-25 10:36_

```suggestion
over `stdin` and dispatches 'tasks' based on the type of message. A 'task' can either be 'local' or 'background' - the former kind has
```

---

_@VascoSch92 reviewed on 2024-03-25 10:39_

---

_Review comment by @MichaReiser on `crates/ruff_server/CONTRIBUTING.md`:28 on 2024-03-25 14:04_

The sentence here is unclear to where I need to provide the path to the locally-built binary. I think it's worth clarifying that this applies to the VS code extension.

You don't have to restart your editor for testing. Reload Window (CTRL+P, Reload) should be sufficient.

---

_Review comment by @MichaReiser on `crates/ruff_server/CONTRIBUTING.md`:32 on 2024-03-25 14:05_

It might be worth linking to the pre-release branch, considering that this is for contributors

---

_Review comment by @MichaReiser on `crates/ruff_server/CONTRIBUTING.md`:7 on 2024-03-25 14:06_

Is there a file we could link to from `data model`. Like is there a datastructure that stores the entire editor state? 

I think it would also be helpful to add other links. E.g link `event loop`  to the event loop in the implementation. Link `local` or `background` to the corresponding definitions. Another useful link could be around *Snapshots of the server*. 

---

_Review comment by @MichaReiser on `crates/ruff_server/README.md`:34 on 2024-03-25 14:08_

I recommend moving this to an issue and linking to it instead, to avoid having to update the information as part of a commit

---

_@MichaReiser approved on 2024-03-25 14:09_

Nice write up. It gives a good overview of the core concepts and the design decisions behind it. Thanks for taking the time for writing this up. I've a few smaller comments on how I think the documentation could be improved.

---

_@snowsignal reviewed on 2024-03-25 18:07_

---

_Review comment by @snowsignal on `crates/ruff_server/README.md`:34 on 2024-03-25 18:07_

I've moved this to a discussion: https://github.com/astral-sh/ruff/discussions/10581

---

_@snowsignal reviewed on 2024-03-25 18:10_

---

_Review comment by @snowsignal on `crates/ruff_server/CONTRIBUTING.md`:28 on 2024-03-25 18:10_

This was actually intended to be a guide for testing with editors that were _not_ VS Code, but that probably isn't very clear. I've tried to clarify what I mean here (this is supposed to be for the 'server command' in your editor's configuration)

---

_@dhruvmanila reviewed on 2024-03-25 18:37_

---

_Review comment by @dhruvmanila on `crates/ruff_server/README.md`:1 on 2024-03-25 18:37_

nit: I have a slight preference for the former "Ruff Language Server" :)

---

_@dhruvmanila approved on 2024-03-25 18:42_

Thank you!

---

_@snowsignal reviewed on 2024-03-26 01:09_

---

_Review comment by @snowsignal on `crates/ruff_server/README.md`:1 on 2024-03-26 01:09_

You know what? Same. Let's change it back 😄 

---

_Merged by @snowsignal on 2024-03-26 06:08_

---

_Closed by @snowsignal on 2024-03-26 06:08_

---

_Branch deleted on 2024-03-26 06:08_

---
