```yaml
number: 6068
title: "fix: added visible long flag aliases in help"
type: pull_request
state: merged
author: GilShoshan94
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2025-07-09T23:01:54Z
updated_at: 2025-07-30T02:16:25Z
url: https://github.com/clap-rs/clap/pull/6068
synced_at: 2026-01-12T16:14:17Z
```

# fix: added visible long flag aliases in help

---

_@GilShoshan94_

fix https://github.com/clap-rs/clap/issues/6067

fix https://github.com/clap-rs/clap/issues/6073

---

_Review comment by @epage on `tests/ui/arg_required_else_help_stderr.toml`:12 on 2025-07-10 14:38_

The fact that no source changed in this commit suggests these commits are not atomic (tests should pass on every commit)

---

_@epage reviewed on 2025-07-10 14:38_

---

_@epage reviewed on 2025-07-10 14:40_

---

_Review comment by @epage on `clap_builder/src/output/help_template.rs`:1 on 2025-07-10 14:40_

`tests/builder/flag_subcommands.rs` has a test for `visible_short_flag_aliases`, could you
- Add a parallel test for long flag aliases?
- Add a test for both for help output in `tests/builder/help.rs`?

Like normal, ideally new tests would be added in a commit before this one and would pass on that commit.

---

_Review comment by @GilShoshan94 on `tests/ui/arg_required_else_help_stderr.toml`:12 on 2025-07-10 15:20_

Oh, ok, I will squash them together as one commit.

---

_@GilShoshan94 reviewed on 2025-07-10 15:20_

---

_@GilShoshan94 reviewed on 2025-07-10 15:32_

---

_Review comment by @GilShoshan94 on `clap_builder/src/output/help_template.rs`:1 on 2025-07-10 15:32_

@epage 
Yes, I will add the tests, no problem.

I'm not sure I understand what you want with the commit/git history exactly.

From what I understand, you want:

- __1st commit__: add the tests for `long_flag_aliases` in `tests/builder/flag_subcommands.rs` and `tests/builder/help.rs` and fix the tests in UI ?
This commit would result in a failed CI and that would be normal.

- __2nd commit__: add the fix in `clap_builder/src/output/help_template.rs` to shows the visible long flag aliases.
This commit would pass the CI.

So in total 2 commits, one to fix and update the tests, the second to fix the source code. 

Did I understand correctly? 
Shall I do that?

---

_Review comment by @GilShoshan94 on `tests/ui/arg_required_else_help_stderr.toml`:12 on 2025-07-10 15:32_

I will wait for your answer about the commits order you want (in the other comment)

---

_@GilShoshan94 reviewed on 2025-07-10 15:32_

---

_@epage reviewed on 2025-07-10 15:53_

---

_Review comment by @epage on `clap_builder/src/output/help_template.rs`:1 on 2025-07-10 15:53_

Commit 1: Adds tests, `cargo test` should pass: this means the test shows the current behavior

Commit 2: Change behavior, update tests to reflect the change in behavior.

#5520 serves as an example of this

---

_@GilShoshan94 reviewed on 2025-07-14 19:35_

---

_Review comment by @GilShoshan94 on `clap_builder/src/output/help_template.rs`:1 on 2025-07-14 19:35_

Hi @epage,

I followed your example.
I added a test in `tests/builder/flag_subcommands.rs`.

I am working now on tests in `tests/builder/help.rs`.

Sorry for the delay, I was not available.

---

_@GilShoshan94 reviewed on 2025-07-14 21:58_

---

_Review comment by @GilShoshan94 on `tests/ui/arg_required_else_help_stderr.toml`:12 on 2025-07-14 21:58_

I fixed it, now the commits are atomic (I check that the tests pass on every commit).

---

_@GilShoshan94 reviewed on 2025-07-14 22:01_

---

_Review comment by @GilShoshan94 on `clap_builder/src/output/help_template.rs`:1 on 2025-07-14 22:01_

I add the tests on the aliases in 'tests/builder/help.rs'.

I also followed your instructions, 2 atomic commits, first that add the tests, the second that change the behavior + update in the tests.

---

_@epage reviewed on 2025-07-18 17:28_

---

_Review comment by @epage on `tests/builder/help.rs`:955 on 2025-07-18 17:28_

An alias functionality test isn't appropriate in this file

---

_@GilShoshan94 reviewed on 2025-07-18 18:32_

---

_Review comment by @GilShoshan94 on `tests/builder/help.rs`:955 on 2025-07-18 18:32_

Should I move it to another file or simply remove it completely?

---

_@epage reviewed on 2025-07-18 18:38_

---

_Review comment by @epage on `tests/builder/help.rs`:955 on 2025-07-18 18:38_

We have a lot of existing alias tests.  If a case isn't covered, we should add a test similar to those next to where the related tests are.

---

_@GilShoshan94 reviewed on 2025-07-21 19:59_

---

_Review comment by @GilShoshan94 on `tests/builder/help.rs`:955 on 2025-07-21 19:59_

Hi,
I looked into the tests and the tests for aliases are distributed between several files:

For `Arg`:
-`tests/builder/arg_aliases_short.rs`
-`tests/builder/arg_aliases.rs`
It covers well everything I think regarding aliases.

For `Command` (on the subcommand):
-`tests/builder/flag_subcommands.rs` 
-`tests/builder/subcommands.rs` 
It does not 100% fully cover in my opinion as it is missing the following cases:
  - [visible_aliases](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_aliases) (but `visible_alias` is tested, but only to match the help, not tested with a `try_get_matches_from`)
  - [visible_short_flag_alias](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_short_flag_alias) (but `visible_short_flag_aliases` is properly tested)
  - [visible_long_flag_alias](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_long_flag_alias) (but `visible_long_flag_aliases` is properly tested)
  
 I will open an issue for that and remove this alias test from this PR so you can merge

---

_@GilShoshan94 reviewed on 2025-07-21 21:27_

---

_Review comment by @GilShoshan94 on `tests/builder/help.rs`:955 on 2025-07-21 21:27_

In the end, I just edited my first commit to incorporate the missing cases.
I will close the issue https://github.com/clap-rs/clap/issues/6073 as well.

I removed the alias functionality test from this place as you asked

---
