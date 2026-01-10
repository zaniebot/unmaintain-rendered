```yaml
number: 17435
title: Relativize path in github annotations
type: pull_request
state: closed
author: sochotnicky
labels: []
assignees: []
base: main
head: fix-relativize-github-annotations
created_at: 2025-04-16T21:10:36Z
updated_at: 2025-05-26T09:38:44Z
url: https://github.com/astral-sh/ruff/pull/17435
synced_at: 2026-01-10T18:51:01Z
```

# Relativize path in github annotations

---

_Pull request opened by @sochotnicky on 2025-04-16 21:10_

-------

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Without relative paths GH actions will not correctly display inline
comments in the right place.

It's possible this is only affecting GitHub enterprise or self-hosted
runners.

See #17433

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

`ruff check --output-format=github` should show annotations with paths relative to root of the project instead of absolute paths in the filesystem.

---

_Comment by @ntBre on 2025-04-16 21:27_

Thanks for the report, and for the PR! Have you been able to test if this fixed your issue? And is it possible to add a test in ruff to demonstrate it, or is the difference only apparent on the GitHub runners?

I'm also wondering if we should track the `project_dir` like in the GitLab version:

https://github.com/astral-sh/ruff/blob/0d29246eb2822617e962c203233216eefc278319/crates/ruff_linter/src/message/gitlab.rs#L79-L81

I'm not that familiar with this part of the codebase, so these are just a couple of quick ideas.

---

_Comment by @github-actions[bot] on 2025-04-16 21:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sochotnicky on 2025-04-16 22:02_

> Thanks for the report, and for the PR! Have you been able to test if this fixed your issue? 

Yes, I actually realized that the annotations were set, but comments were not shown because the paths didn't match. So testing this is in the end easy. Compile ruff, run it in a project with github output and make sure paths are relative to project root not absolute filesystem paths.

There's probably some corner cases but this code seems to work as expected for my case at least

> And is it possible to add a test in ruff to demonstrate it, or is the difference only apparent on the GitHub runners?

I am sure it's possible, just not sure how to do that right now. All current tests seem to have filenames without paths in them so I'd need to dig a bit how that works

> I'm also wondering if we should track the `project_dir` like in the GitLab version:
> 
> https://github.com/astral-sh/ruff/blob/0d29246eb2822617e962c203233216eefc278319/crates/ruff_linter/src/message/gitlab.rs#L79-L81
> 
> I'm not that familiar with this part of the codebase, so these are just a couple of quick ideas.

That gitlab example is a good one actually. I agree that making this consistent makes sense. Will take a few days to get back to this to finish it off. Feel free to adjust ahead of time if you feel like it :-)

---

_Comment by @sochotnicky on 2025-04-17 07:31_

So what's missing is tests. It's doable, one way might be to tweak https://github.com/sochotnicky/ruff/blob/4d735c496b8dd02dde5b24c72d702b30742a151b/crates/ruff_linter/src/message/mod.rs#L376 method so that `fib.py` and `undef.py` actually have a path. Something like `/projects/project/fib.py` and `/projects/project/undef.py`. 

Doing that unconditionally would change outputs of other tests too though. Adding an argument to `create_messages` that would be Option<String> for "base path" for the files doesn't feel quite right either. 

Any preferences/thoughts? 

Edit: I've pushed a commit with the `/projects/project/...` change to better illustrate. If the approach is acceptable I can polish it a bit (i.e. get the tests working - they are broken in GH CI since the code tries to get relative path for a file that has no path

---

_@sochotnicky reviewed on 2025-04-17 08:06_

---

_Review comment by @sochotnicky on `crates/ruff_linter/src/message/snapshots/ruff_linter__message__gitlab__tests__output.snap`:15 on 2025-04-17 08:06_

Alternatively - provide the `project_dir` same as in github test and this won't change, instead we could add a new test

---

_Comment by @sochotnicky on 2025-04-18 06:46_

Re-thinking, I actually think changing the test cases to use some full absolute path would make sense? That's what gets actually used in real usage (at least in my case).

---

_Comment by @ntBre on 2025-04-18 16:52_

Thanks again for working on this. I've been checking in on it when I receive notifications but haven't done a full review yet. I'm also interested in @MichaReiser's thoughts when he gets back next week.

---

_Comment by @sochotnicky on 2025-04-19 09:21_

> Thanks again for working on this. I've been checking in on it when I receive notifications but haven't done a full review yet. I'm also interested in @MichaReiser's thoughts when he gets back next week.

No rush, I have a workaround in place. I'll pause work until I hear back so I don't waste time. I realize the junit tests fail on windows. If the current approach would be acceptable I'd have a look and fix that up. Otherwise I can submit the fix without the test addition.

---

_Comment by @MichaReiser on 2025-04-22 07:59_

I haven't looked at the implementation yet because I wanted to consult the GitHub documentation on annotations first but it always returns a *Too many requests* errors. Well... https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions

Do you have any reference for the github annotations?

---

_Comment by @MichaReiser on 2025-04-22 14:55_

I'm a bit hesitant merging this change without a reproduction in the issue. Mainly because I'm surprised that GitHub annotations don't work when using absolute paths (which I think is what we currently have) but do work when using relative paths. Unless the problem is that paths aren't relative nor absolute. 

A reproduction on GitHub would also be great to verify that this change works as expected.

---

_Comment by @sochotnicky on 2025-04-23 14:28_

> I'm a bit hesitant merging this change without a reproduction in the issue. Mainly because I'm surprised that GitHub annotations don't work when using absolute paths (which I think is what we currently have) but do work when using relative paths. Unless the problem is that paths aren't relative nor absolute.

I understand. I've done some manual testing with various echo calls that use relative/absolute paths and the absolute paths simply do not work on our self-hosted runners. The only thing I can think of is that there are several different absolute paths to the work directory (due to symlinks). And maybe for some reason this messes up GH runner code and that's where the actual bug is.

I'd need to dig through https://github.com/actions/runner/blob/c21dc6090bd630da629d79c492a2c9616be71e64/src/Runner.Worker/Handlers/OutputManager.cs and perhaps https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/troubleshooting-workflows/enabling-debug-logging to confirm the above theory to some degree though

> A reproduction on GitHub would also be great to verify that this change works as expected.

I don't have anything public I can share since this is all on private GH enterprise instance. I could redact logs etc. Or perhaps start a dedicated runner and connect it to a public GH repository for testing.

I don't think I'll have much time in coming days to dig deeper though. At some point I'll circle back.

---

_Comment by @dhruvmanila on 2025-04-23 15:19_

We could ask a question in https://github.com/orgs/community/discussions.

> I'd need to dig through [actions/runner@`c21dc60`/src/Runner.Worker/Handlers/OutputManager.cs](https://github.com/actions/runner/blob/c21dc6090bd630da629d79c492a2c9616be71e64/src/Runner.Worker/Handlers/OutputManager.cs) and perhaps [docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/troubleshooting-workflows/enabling-debug-logging](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/troubleshooting-workflows/enabling-debug-logging) to confirm the above theory to some degree though

You're possibly aware of this but for GitHub docs, you'd need to search in the enterprise documentation, the link that you've provided is for the public instance. You can change that using a dropdown menu in the top.

---

_@MichaReiser reviewed on 2025-04-24 06:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/github.rs`:17 on 2025-04-24 06:54_

> The default working directory on the runner for steps, and the default location of your repository when using the [checkout](https://github.com/actions/checkout) action. For example, /home/runner/work/my-repo-name/my-repo-name.

I'm concerned that this will break users that clone their repository to a non-default location. 

It makes me wonder if we need to transform the paths twice:

1. Make the path absolute relative to the current working directory
2. Make the path relative to `GITHUB_WORKSPACE` 

Or are the paths already absolute? 

(I'm sorry, I'm not very familiar with the entire Message infrastructure)

---

_Comment by @MichaReiser on 2025-05-26 09:38_

It seems this PR has stalled. Feel free to comment if you plan to continue working on this.

---

_Closed by @MichaReiser on 2025-05-26 09:38_

---
