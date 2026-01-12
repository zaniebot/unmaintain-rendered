```yaml
number: 12017
title: "Add a link to the Concepts > Build systems page to 'uv init --build-backend` documentation (#11502)"
type: pull_request
state: closed
author: dd-ssc
labels: []
assignees: []
base: main
head: main
created_at: 2025-03-06T19:09:40Z
updated_at: 2025-03-10T05:52:45Z
url: https://github.com/astral-sh/uv/pull/12017
synced_at: 2026-01-12T16:10:06Z
```

# Add a link to the Concepts > Build systems page to 'uv init --build-backend` documentation (#11502)

---

_@dd-ssc_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

~~Amend user documentation with details on `uv init` build backend defaults.
Doc caveat: Omitting `--build-backend` and passing `--build-backend hatch` do not result in identical projects.~~

Amending pull request with better approach:
Add a link to the Concepts > Build systems page to 'uv init --build-backend` documentation.
This works, but I'm not sure how happy developers are about markdown links in the code 🤷🏻‍♂️.

Second attempt at submitting a PR; see first attempt https://github.com/astral-sh/uv/pull/12011 for some comments.

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

_@konstin reviewed on 2025-03-07 14:17_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2613 on 2025-03-07 14:17_

This is a consequence of "Implicitly sets `--package`.", as only packaged projects have a build backend, as of now. (This may change with the uv build backend becoming the default). Maybe we need to phrase it the other way round: If not set explicitly and the project is non-packaged, the default is no build-system entry.

---

_Renamed from "Add doc on 'uv init --build-backend` default and caveat (#11502)" to "Add a link to the Concepts > Build systems page to 'uv init --build-backend` documentation (#11502)" by @dd-ssc on 2025-03-09 18:48_

---

_@dd-ssc reviewed on 2025-03-09 18:56_

---

_Review comment by @dd-ssc on `crates/uv-cli/src/lib.rs`:2613 on 2025-03-09 18:56_

I have learned in the mean time that _If not passed, uv will use `hatch`_ is indeed not correct; the rest might be true, but is missing the point; things are a little more involved than I initially understood.

A possibly better way forward: Replace my earlier changes by a link to the appropriate section in the _Concepts_ chapter so people new to the project know where to look.

<details>
  <summary>Details</summary>

(For some reason, my comments tend to get far longer than intended 🥴 --> adding a TL;DR above and hiding this in details block to keep things concise.)

> This is a consequence of "Implicitly sets `--package`.", as only packaged projects have a build backend, as of now. (This may change with the uv build backend becoming the default). Maybe we need to phrase it the other way round: If not set explicitly and the project is non-packaged, the default is no build-system entry.

I'm afraid you've somewhat lost me there 🥴. Please remember: I'm a not a uv developer, but a newbie uv user.

To give you an idea of the extent of my cluelessness: I have never heard of most of the build backends listed by `uv init --help`; in the last 20+ years of commercial python development, I never had to. Also, this is not something I am concerned with when starting out with uv.

Maybe it helps to look at the user experience of the first 10 minutes of getting started with uv as it was for me - and as it possibly is for many new users:

1. I take a quick look at [Installing uv](https://docs.astral.sh/uv/getting-started/installation) and run

```shell
$ brew install uv
   ...
```

2. I check [Creating a new project](https://docs.astral.sh/uv/guides/projects#creating-a-new-project) and run

```shell
$ uv init <name of my project>
   ...
```

3. I add configuration to the generated `pyproject.toml` as needed for my project, possibly copying / adapting things from an existing project. Most of the standard stuff is somewhat straight forward (`authors`, `dependencies`, `description`, etc.); I build the project to verify things work as expected:

```shell
$ uv build
   ...
```

4. I want to configure "non-standard" things; adding non-source files to the package in my case. At this stage I figure the required setting probably depends on the build backend. However, there is no information on which build backend is actually used. Also, I realize there isn't even a `[build-system]` in the generated `pyproject.toml` at all 🤨.

5. I post an [issue, asking for help](https://github.com/astral-sh/uv/issues/11502).

6. Someone has to spend time to [write a response](https://github.com/astral-sh/uv/issues/11502#issuecomment-2695259348) to help me out - thanks again, @konstin 🙏🏻.

   It is only now that I have all the info I need to continue: I head over to the [hatch docs](https://hatch.pypa.io/latest/config/build), read up and can get on with my work 😌. Of course, the additional info provided by @konstin also helped a great deal 👍.

7. I create two quick test projects using `uv init` and `uv init --build-backend hatch` and am surprised they don't produce the same results: Apparently, I misinterpreted _hatchling, our default with `uv init`_ as _if no `--build-backend` argument is passed, then `hatch` is used as default argument_.

8. In order to prevent that anyone in the future needs to overcome the same obstacles while getting started with uv, I amend the documentation with what I learned and observed and submit this PR.

---

It is only now as I write this comment that I find the time to dig a little deeper and look into e.g. [Build systems](https://docs.astral.sh/uv/concepts/projects/config/#build-systems) - after all, who reads a section titled _Concepts_ when they're only starting out with a new tool ?

My key learning from this: `uv init` alone creates a `pyproject.toml` that makes `uv` do stuff that uv developers possibly understand, but I don't --> always use `uv init --build-backend hatch` to create new projects.
</details>


---

_Comment by @zanieb on 2025-03-09 19:33_

Unfortunately we can't add links like this, because it'll be present in the `uv help` output in the CLI. We still haven't decided on the best way to support links from the reference to the rest of the documentation.

---

_Comment by @dd-ssc on 2025-03-10 05:52_

Yeah, I thought as much.

---

_Closed by @dd-ssc on 2025-03-10 05:52_

---
