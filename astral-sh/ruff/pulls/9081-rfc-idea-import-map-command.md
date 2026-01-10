```yaml
number: 9081
title: "RFC/idea: import map command"
type: pull_request
state: closed
author: akx
labels: []
assignees: []
base: main
head: import-map
created_at: 2023-12-10T22:17:30Z
updated_at: 2024-03-12T18:48:34Z
url: https://github.com/astral-sh/ruff/pull/9081
synced_at: 2026-01-10T22:47:01Z
```

# RFC/idea: import map command

---

_Pull request opened by @akx on 2023-12-10 22:17_

## Summary

This PR experiments with a new top-level command `ruff import-map` (nomenclature subject to change and discussion).

Since Ruff is pretty fast at parsing Python and it already needs to figure out import maps internally (the type being aptly named `ImportMap`), I think it would be useful to be able to export those in a machine-readable format too, for instance to act as a backend for something like [`pydeps`](https://github.com/thebjorn/pydeps) – or in my case, I'd like to find names in a codebase that aren't being imported by anything else. (I tried `pydeps` and ran out of patience.)

As you can see, 
* there's a whole bunch of TODOs
* documentation is lacking
* the output stage just YOLO dumps serde_json
* the code is copy-paste-cannibalized from `format.rs` and `checkers/imports.rs`
* I'm honestly not sure I'm using the correct abstraction level of APIs for reading the files (is there a "just give me an AST" API?)
* probably other things

but I figured I'd preferably get some code out there rather than just discussion 😁 

WDYT, should I continue with this, would it be accepted in some form?

## Test Plan

No good test plan yet other than trying out e.g.

```
cargo run --bin=ruff -- import-map ../home-assistant-core
```
to see that it doesn't explode. (It doesn't, and the current non-cached, non-parallel implementation takes about 8 seconds to suss out and JSON dump exports from about 11,000 Python files there.)

---

_Comment by @MichaReiser on 2023-12-10 23:35_

Thanks for this PR. 

For me, the main question is whether such a command fits into the goals of Ruff, and we probably need to spend some time internally figuring this out. What's Ruff's ultimate goal? Is it to improve developer productivity? Is supporting a vast set of lint rules from the ecosystem achieving this goal? Is exporting a `import-map` command achieving this goal or is it distracting (making the tool harder to understand because the tool is more complicated?). 

Alternatives are: Programmatic API only, standalone CLI, a subcommand to collect various project information, LSP (or any other daemon process) that can be queried, but not over the CLI, ... 

We haven't talked about this as a team, but to me, an import-map-specific command seems too specific for a tool with Ruff's focus. But I see value in giving users a way to use ruff (or a similar tool) to quickly extract information about their project and build some custom scripts. 

---

_Comment by @akx on 2023-12-11 06:17_

@MichaReiser Thanks for chiming in! I was honestly thinking the same thing, that this doesn't necessarily feel like a `ruff` (the command) thing, but it was the easiest way to get something running for now :)

If overcomplicating Ruff the Command is an issue (and I can see how it can be), one option might be a separate `ruff-plumb` type CLI (and package), that, like [Git's plumbing (c.f. porcelain)](https://stackoverflow.com/questions/6976473/what-does-the-term-porcelain-mean-in-git) would contain commands that regular users might not need that often or at all, but would benefit from Ruff's infrastructure (such as this PR does). (Alternatively, like Git does, plumbing could be invoked via `ruff` itself, but as sub-subcommands, and would be thus hidden from the top-level `help`: `ruff plumb import-map`?) Of course maintaining plumbing stuff in-tree in this repo will have their own cognitive and maintainability costs...



---

_Comment by @zanieb on 2024-03-12 18:48_

While this is pretty cool, I think our lack of response here is an indication that we're too small of a team to expand the scope of Ruff further at this time. I worry kinds of plumbing interfaces end up being a lot of maintenance since they are closer to our internal abstractions.

---

_Closed by @zanieb on 2024-03-12 18:48_

---
