```yaml
number: 12011
title: "Add doc on 'uv init --build-backend` default and caveat (#11502)"
type: pull_request
state: closed
author: dd-ssc
labels: []
assignees: []
base: main
head: main
created_at: 2025-03-06T17:58:31Z
updated_at: 2025-03-06T19:10:39Z
url: https://github.com/astral-sh/uv/pull/12011
synced_at: 2026-01-12T16:10:06Z
```

# Add doc on 'uv init --build-backend` default and caveat (#11502)

---

_@dd-ssc_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Amend user documentation with details on `uv init` build backend defaults.
Doc caveat: Omitting `--build-backend` and passing `--build-backend hatch` do not result in identical projects.

## Test Plan

Following the instructions in [CONTRIBUTING.md#documentation](https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#documentation):
+ run `cargo dev generate-all`
+ run the doc dev server
+ add my changes
+ inspect the result in a browser: Looks ok ✅.
+ run `npx prettier --prose-wrap always --write "**/*.md"`

## Updated Test Plan

It seems the instructions don't mention the CLI reference section getting generated from Rust code: I modified the `.md` file directly and that caused some PR checks to fail 🙈. New plan:
+ add the new documentation to `crates/uv-cli/src/lib.rs`
+ run `cargo dev generate-cli-reference`
+ run the doc dev server
+ inspect the result in a browser: Looks ok ✅.
+ run `npx prettier --prose-wrap always --write "**/*.md"`


---

_Comment by @zanieb on 2025-03-06 18:17_

Where do we suggest that `--build-backend hatch` should be the same as the default?

---

_Comment by @dd-ssc on 2025-03-06 18:37_

> Where do we suggest that `--build-backend hatch` should be the same as the default?

Nowhere, AFAIK - but IMHO, if `hatch` is used as the default, then it seems logical to assume that omitting `--build-backend` and passing `--build-backend hatch` have the same effect, i.e. result in identical projects. Might be just me 🤷🏻‍♂️

---

_Comment by @zanieb on 2025-03-06 18:44_

`hatch` isn't used as the default though, it's only used with `--package` or `--lib` which I think call out this difference from `--app`?

---

_Comment by @dd-ssc on 2025-03-06 19:00_

Possibly - I've just taken that info from @konstin at https://github.com/astral-sh/uv/issues/11502#issuecomment-2695259348  🤷🏻‍♂️ 

As mentioned in that issue - to me (and to possibly many other users), the info about uv's build backend default is absolutely crucial - and in fact removes a hard block in our projects, because now I know where to look for info on how to achieve my original goal: The [hatchling docs](https://hatch.pypa.io/latest) ✅. Hence this PR.

I might make it a habit in the future though to pass `--build-backend hatch` explicitly to `uv init`, just so that the build backend is explicitly written into `pyproject.toml`.

---

_Closed by @dd-ssc on 2025-03-06 19:02_

---

_Comment by @zanieb on 2025-03-06 19:08_

I see, yeah sorry about the confusion. We're moving forward with our own build backend ASAP so you don't need to do that. `uv init --package` should be equivalent for you,.

---

_Comment by @dd-ssc on 2025-03-06 19:10_

Sorry, I messed up the PR 🙈
Seems best to submit a new one: https://github.com/astral-sh/uv/pull/12017

---
