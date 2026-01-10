```yaml
number: 10416
title: Re-code flake8-trio and flake8-async rules to match upstream
type: pull_request
state: merged
author: augustelalande
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.5
head: merge-trio-async
created_at: 2024-03-15T01:29:30Z
updated_at: 2024-06-26T14:49:53Z
url: https://github.com/astral-sh/ruff/pull/10416
synced_at: 2026-01-10T21:55:59Z
```

# Re-code flake8-trio and flake8-async rules to match upstream

---

_Pull request opened by @augustelalande on 2024-03-15 01:29_

## Summary

Re-code flake8-trio and flake8-async rules to match upstream. Resolves #10303.

**`TRIO` rules**

* `TRIO` has been renamed to `ASYNC1`
* `TRIO100` -> `ASYNC100`
* `TRIO105` -> `ASYNC105`
* `TRIO109` -> `ASYNC109`
* `TRIO110` -> `ASYNC110`
* `TRIO115` -> `ASYNC115`

**`ASYNC` rules**

* `ASYNC116` unchanged
* `ASYNC100..ASYNC102` has been renamed to `ASYNC2`
* `ASYNC100` moved to `ASYNC210`, 
* `ASYNC101` was split into `ASYNC220`, `ASYNC221`, `ASYNC222`, `ASYNC230`, and `ASYNC251`
* `ASYNC102` was merged into `ASYNC220` and `ASYNC221`

## Behavior changes

* `ASYNC210` (previously `ASYNC100`): Now detects `urllib3.request` calls
* `ASYNC230`: Now also detects calls to `io.open`, `io.open_code`

## Test Plan

<!-- How was it tested? -->


---

_Comment by @augustelalande on 2024-03-15 01:34_

This is mostly done, except for the previous ASYNC101 and ASYNC102 which don't exactly match any the of the new ASYNC rules. The closest matches are ASYNC220 and ASYNC222, but the behaviors don't match exactly. Part of the problem is that the previous rules should be split into multiple rules (this is easy). The other problem which I'm not sure how to resolve is that it doesn't seem like any of the new ASYNC rules check for `time.sleep` or ~`open`~ (nevermind ASYNC230 covers `open`), but this is a useful check that we should keep, so the question is what should I do?

---

_Comment by @github-actions[bot] on 2024-03-15 01:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @augustelalande on 2024-03-15 05:04_

Ok so I modified the implementation of ASYNC220 and ASYNC222, as well as added ASYNC221 so that the three rules would match the behavior of flake8-async. For now I added a check for time.sleep as part of ASYNC222.

---

_Comment by @jakkdl on 2024-03-15 09:32_

Oh, I'd missed that we don't check for time.sleep anymore. It's a bad match for ASYNC222, as the recommended replacement is using `[trio/anyio/asyncio].sleep()`, and not to wrap it in `to_thread_run_sync` or `run_in_executor`. I'll implement it quickly as ASYNC251

---

_Comment by @augustelalande on 2024-03-15 13:20_

Ok I moved the time.sleep check to a newly created ASYNC251

---

_Comment by @Zac-HD on 2024-03-15 16:40_

We've just released ASYNC251 upstream, so this is back to matching the merged flake8-async plugin.

---

_Comment by @zanieb on 2024-03-15 16:55_

Note that we'll need to add "redirects" for all of these rules, see [rule_redirects.rs](https://github.com/astral-sh/ruff/blob/1fadefa67b26508cc59cf38e6130bde2243c929d/crates/ruff_linter/src/rule_redirects.rs). See https://github.com/astral-sh/ruff/pull/9756 for example.

We may be able to make use of https://github.com/astral-sh/ruff/pull/9691 as well, not sure yet.


---

_Comment by @augustelalande on 2024-03-15 21:51_

Ok I added redirects for the old TRIO rules. But as mentioned the old ASYNC rules don't directly correspond to new ASYNC rules.

---

_@charliermarsh reviewed on 2024-03-18 20:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_redirects.rs`:115 on 2024-03-18 20:29_

It looks like some ASYNC rules were also moved from one code to another.

---

_@charliermarsh reviewed on 2024-03-18 20:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:331 on 2024-03-18 20:34_

I think these probably _shouldn't_ be in preview.

---

_@augustelalande reviewed on 2024-03-18 23:18_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rule_redirects.rs`:115 on 2024-03-18 23:18_

ASYNC100 moved to ASYNC210, (but the ASYNC100 code was taken over by TRIO100)
ASYNC101 was split into ASYNC220, ASYNC221, ASYNC222, ASYNC230, and ASYNC251
ASYNC102 was merged into ASYNC220 and ASYNC221

---

_@augustelalande reviewed on 2024-03-18 23:18_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/codes.rs`:331 on 2024-03-18 23:18_

Up to you

---

_@augustelalande reviewed on 2024-03-18 23:19_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/codes.rs`:331 on 2024-03-18 23:19_

Done

---

_@augustelalande reviewed on 2024-03-28 22:57_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rule_redirects.rs`:115 on 2024-03-28 22:57_

@charliermarsh any advice on this?

---

_Assigned to @charliermarsh by @zanieb on 2024-03-29 03:37_

---

_Assigned to @zanieb by @zanieb on 2024-03-29 03:37_

---

_Comment by @jakkdl on 2024-05-17 16:46_

I've got some comments on the human-friendly rule names you have, and it might be good if we synced them to ease transition between flake8-async and ruff. See https://github.com/python-trio/flake8-async/pull/248#issuecomment-2117984198

(I'm sorry we're moving so quickly as of late :grin:)

---

_Comment by @JelleZijlstra on 2024-05-23 02:11_

This PR has some merge conflicts now, anything I can do to help?

I'm motivated to add (the new) ASYNC101 to Ruff because of the problems mentioned in the draft [PEP 789](https://github.com/python/peps/pull/3782).

---

_Comment by @charliermarsh on 2024-05-23 02:15_

The main issue is that it's going to be a breaking change for users (e.g., `ASYNC100` continues to exist but is now a different rule). We may be able to include it in v0.5.0 though which will be ~soon.

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-05-23 02:16_

---

_Comment by @charliermarsh on 2024-05-23 02:16_

@zanieb - let's include this in v0.5.0 since it's starting to cause problems.

---

_Comment by @charliermarsh on 2024-05-23 02:16_

@augustelalande -- I'm happy to take responsibility for resolving any conflicts.

---

_@charliermarsh reviewed on 2024-05-23 02:17_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_redirects.rs`:115 on 2024-05-23 02:17_

I think what you have here is right... The TRIO rules are gone, so lets redirect them away. But the ASYNC rules probably can't be safely redirected.

---

_Comment by @augustelalande on 2024-05-23 02:34_

@charliermarsh I'll do it now. I'm also gonna update the rules with the new names from https://github.com/python-trio/flake8-async/pull/248#issuecomment-2117984198

---

_Comment by @augustelalande on 2024-05-23 02:57_

Nevermind the last part. Updating all the rule names is a significant effort so should be a separate PR.

---

_Comment by @MichaReiser on 2024-06-25 12:14_

Okay, I'm done with the rebase and I very much hope I didn't mess anything up. I now need to get a better understanding of the change itself to be able to review it :)

---

_Comment by @MichaReiser on 2024-06-25 12:54_

I'm trying to get my head around over all the changes and are having a hard time. What's unclear to me is:

* What's the exact mapping from old rules to new rules
* What I understand is that our rules don't precisely match `flake8-async`. Which rules are different and to what extend? 

---

_Comment by @MichaReiser on 2024-06-25 13:47_

From the chanegelog: `Added asyncio support for several error codes`. I think that' the case for most of the TRIO rules. This isn't something we have to change as part of this PR but important for the changelog

---

_Comment by @MichaReiser on 2024-06-25 15:45_

Okay, I think I now have a good overview of what has changed. I plan on reviewing this PR tomorrow (and hopefully merge). 

I don't think there's anything we can do about mapping the old `ASYNC` rules. 

**Trio** to **ASYNC**

* `TRIO100` -> `ASYNC100` : 
  * No support for `anyio` or `asyncio`
  * Ruff: `TrioTimeoutWithoutAwait`
  * Flake8: `cancel-scope-no-checkpoint`
* `TRIO105` -> `ASYNC105`
  * Ruff: `TrioSyncCall`
  * Flake8: `missing-await`
  * Both only support trio
* `TRIO109` -> `ASYNC109`
  * I think the flake8 rule also supports `anyio` and `asyncio`. 
  * Ruff: `TrioAsyncFunctionWithTimeout`
  * Flake8: `async-function-with-timeout`
* `TRIO110` -> `ASYNC110`
  * Ruff: `TrioUnneededSleep`
  * Flake8: `async-busy-wait`
  * Missing `anyio` support
* `TRIO115` -> `ASYNC115`
  * Ruff: `TrioZeroSleepCall`
  * Flake8: `async-zero-sleep`
  * Missing `anyio` support
* `TRIO116` -> `ASYNC116`
  * Ruff: `SleepForeverCall`
  * Flake8: `long-sleep-not-forever`
  * Missing `anyio` support



**ASYNC** rule changes

* `ASYNC100` -> `ASYNC210`:
  * Ruff: `BlockingHttpCallInAsyncFunction`
  * Flake8: `blocking-http-call`
  * Looks about right
* `ASYNC101` -> 
  * Used to test for 
    * `open` -> `ASYNC230`
    * `time.sleep` -> `ASYNC251`
    * `subprocess`: 
      * `subprocess.run`, -> `ASYNC221`
      * `subprocess.Popen` -> `ASYNC220`
      * `subprocess.call` -> `ASYNC221`
      * `subprocess.check_call` -> `ASYNC221`
      * `subprocess.check_output` -> `ASYNC221`
      * `subprocess.getoutput`, -> `ASYNC221`
      * `subprocess.getstatusoutput` -> `ASYNC221`
    * `os`
      * `wait` -> `ASYNC222`
      * `wait3` -> `ASYNC222`
      * `wait4` -> `ASYNC222`
      * `waitid` -> `ASYNC222`
      * `waitpid` -> `ASYNC222`
  * `ASYNC220`:
    * Also tests for `os.popen`, `os.spawn` (if not `p_wait`)
    * flake8: `blocking-create-subprocess`
    * Ruff: `CreateSubprocessInAsyncFunction`
  * `ASYNC221`: 
    * Also tests for `os.system`, `os.posix_spawn`, `os.posix_spawnp`
    * flake8: `blocking-run-process`
    * Ruff: `RunProcessInAsyncFunction`
    * Sometimes `os.spawn` (when not `ASYNC220`)
  * `ASYNC222`:
    * Ruff: `WaitForProcessInAsyncFunction`
    * Flake8: `blocking-process-wait`
  * `ASYNC230`: 
    * Ruff: `BlockingOpenCallInAsyncFunction`
    * flake8: `blocking-open-call`
  * `ASYNC251`:
    * Ruff: `BlockingSleepInAsyncFunction`
    * flake8: `blocking-sleep`

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-25 15:51_

---

_Unassigned @charliermarsh by @MichaReiser on 2024-06-25 15:51_

---

_Unassigned @zanieb by @MichaReiser on 2024-06-25 15:51_

---

_Label `breaking` added by @MichaReiser on 2024-06-25 15:51_

---

_Label `configuration` added by @MichaReiser on 2024-06-25 15:51_

---

_Comment by @jakkdl on 2024-06-25 17:13_

I'm not sure with "no support for x" you refer to upstream, or this PR. But in case there's any confusion about upstream support:
* ASYNC100 supports all of trio/anyio/asyncio
* ASYNC105 is trio-only. Since type checkers started catching this the rule is mostly redundant, which is the only reason we haven't cared to extend it for anyio/asyncio.
* ASYNC109 supports all of trio/anyio/asyncio
* ASYNC110 has trio&anyio support. asyncio support is TODO, but it's a trivial fix so you could probably go ahead and support it regardless of if/when I get around to it upstream
* ASYNC115&ASYNC116 has trio&anyio support. asyncio does not have equivalent constructs

---

_Comment by @MichaReiser on 2024-06-25 19:41_

@jakkdl Thanks for detailing the upstream features. I mainly tried to document with *no support* what features Ruff lacks compared to the upstream rules. I don't think we should or have to address these shortcomings in this PR but I want to be aware of it when reviewing (and we may even want to document it)

---

_Comment by @augustelalande on 2024-06-25 21:18_

Good summary from @MichaReiser and @jakkdl

There is still a significant amount of work to bring feature parity between `ruff` and `flake8-async`, but IMO the first step has to be resolving the merge between `flake8-async` and `flake8-trio` (i.e. this PR), everything else should follow in future PRs.

---

_Comment by @augustelalande on 2024-06-25 21:22_

> @jakkdl Thanks for detailing the upstream features. I mainly tried to document with _no support_ what features Ruff lacks compared to the upstream rules. I don't think we should or have to address these shortcomings in this PR but I want to be aware of it when reviewing (and we may even want to document it)

Not sure if it needs to be documented in the code base, or if simply opening an issue would be sufficient.

FYI `flake8-trio` is already being tracked in #8451

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_redirects.rs`:107 on 2024-06-26 07:25_

I think we should change `TRIO` to only map to `ASYNC1` because the old `ASYNC100` rules weren't part of the `ASYNC` selector. That's still not perfect because it will enable `ASYNC116` but it is closer to what users had before.

---

_@MichaReiser reviewed on 2024-06-26 07:25_

---

_@MichaReiser approved on 2024-06-26 07:44_

Sorry for the long wait. This looks really good. Thanks for going through the struggle of not just recoding the rules, but also splitting the existing rules and adopting their behavior to match the upstream rules.

---

_Comment by @MichaReiser on 2024-06-26 08:42_

I manually tested the redirection rules and it all looks good. Let's merge this.

---

_Merged by @MichaReiser on 2024-06-26 08:42_

---

_Closed by @MichaReiser on 2024-06-26 08:42_

---

_Branch deleted on 2024-06-26 14:08_

---

_Comment by @MichaReiser on 2024-06-26 14:49_

I think one nice follow up after the release would be to rename the rules to match the upstream names.

---
