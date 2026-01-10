```yaml
number: 2205
title: Improve the developer experience of working on ty
type: issue
state: open
author: AlexWaygood
labels:
  - testing
  - internal
assignees: []
created_at: 2025-12-24T13:46:27Z
updated_at: 2025-12-24T21:06:56Z
url: https://github.com/astral-sh/ty/issues/2205
synced_at: 2026-01-10T01:56:41Z
```

# Improve the developer experience of working on ty

---

_Issue opened by @AlexWaygood on 2025-12-24 13:46_

We discussed in a recent ty planning meeting that it can sometimes feel a bit painful to work on ty right now as a developer. Things that we discussed which might help include:
- Improving the typing-conformance workflow. All lines in the typing-conformance test suite where a type checker _should_ emit an error have an `# E` comment next to them; a type checker is not expected to emit an error on any other lines. It should therefore not be too hard to improve this workflow so that it automatically tells you whether a diagnostic being added or going away is a good or bad change.
- Make the ecosystem-analyzer and/or mypy_primer workflow more "positive". If you've worked for several days on a PR, it can be a bit dispiriting to then be greeted with an overwhelming list of new diagnostics being emitted on the ecosystem that you have to dig through and analyze. It would be nice to also have more positive feedback, such as: "You reduced the percentage of `@Todo` or `Unknown` types in the ecosystem by 37%, well done!".
- Make the ecosystem reports easier to analyze:
  - Could we run other type checkers on this code as well, and also have an indication in the report about whether our diagnostics are in line with those of other type checkers? It's often useful to know that other type checkers also emit an error on a certain line of code.
  - Fix https://github.com/astral-sh/ty/issues/1670

We didn't discuss it explicitly in the meeting, but I'd also add to this list:
- Reduce the number of generated files that it's necessary to check in when making a PR. We're adding/removing/renaming/redocumenting diagnostics at a very quick velocity right now. Having to run `cargo dev generate-all` increases the amount of time it takes to prepare a PR (I have to compile a whole different crate locally before submitting it), and it's hard to remember to do it, so you often end up filing the PR, then only remembering to run `cargo dev generate-all` when CI starts to fail. Generating the `rules.md` docs and the `ty.schema.json` file feel like they could be done as part of the release workflow on this repository, rather than having to be checked in as part of PRs to the Ruff repository? Alternatively, we could look at whether these files could be generated automatically behind the scenes when running `mdtest.py` or `cargo test -p ty_python_semantic`; then you wouldn't have to remember to execute a separate command before filing the PR.

---

_Label `testing` added by @AlexWaygood on 2025-12-24 13:46_

---

_Label `internal` added by @AlexWaygood on 2025-12-24 13:46_

---

_Comment by @MichaReiser on 2025-12-24 13:49_

> Alternatively, we could look at whether these files could be generated automatically behind the scenes when running mdtest.py or cargo test -p ty_python_semantic; then you wouldn't have to remember to execute a separate command before filing the PR.

We could consider an ENV variable for people who prefer this YOLO mode (I don't)

---

_Comment by @AlexWaygood on 2025-12-24 14:01_

Another advantage of only generating these files as part of the release workflow (rather than having to do it as part of PRs to the Ruff repo at all) is that it would make it much easier for people to submit drive-by docs contributions. In my experience, drive-by docs contributors often tend to edit the source directly from the GitHub UI; they might not even have Ruff cloned locally, let alone have Rust installed. I think most contributors don't expect that they'll need to run a regeneration script when editing the docs. Removing the need to run the regeneration script and commit the changes for every docs PR makes our repo much more friendly to that category of external contributors, I think.

---

_Comment by @MeGaGiGaGon on 2025-12-24 21:06_

That would be good, the only reason I was able to contribute as much as I did to the Ruff docs is because making those drive-by PRs was very easy. I haven't looked at contributing to the ty docs yet, but if making even simple rule description changes requires having the project compiled I would not be able to contribute because of how long it takes for Ruff to compile.

---
