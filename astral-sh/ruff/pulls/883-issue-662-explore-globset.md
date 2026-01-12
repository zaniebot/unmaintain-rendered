```yaml
number: 883
title: Issue 662 explore globset
type: pull_request
state: merged
author: CelebrateVC
labels: []
assignees: []
merged: true
base: main
head: issue-662-explore-globset
created_at: 2022-11-23T00:38:52Z
updated_at: 2022-12-11T17:04:13Z
url: https://github.com/astral-sh/ruff/pull/883
synced_at: 2026-01-12T05:36:31Z
```

# Issue 662 explore globset

---

_Pull request opened by @CelebrateVC on 2022-11-23 00:38_

Thank you for submitting this pull request! We appreciate you spending the time to work on these changes.

## What is the motivation?

In #662 Charlie was asking if [globset](https://docs.rs/globset/0.4.9/globset/) could speed up runtimes of file matching. After discussing a test strategy for the use case was decided.

## What does this change do?

swaps out `glob` for `globset` entirely, Strips code of `FilePattern` as those are now compiled `Glob`s which instead of being stored in `Vec<FilePattern>`s are grouped together in a `GlobSet`. `is_excluded` is modified to reflect this change and now takes in GlobSets.

## What is your testing strategy?

Compiled the base image using `glob` and compiled altered code, per Contributing guide, benchmarked using hyperfine. Based on the discussion in #662 kneecapped the linter.rs#lint_path function to immediately return default Diagnostic, in order to exclusively test path discovery. below are current code state benchmarks.


![image](https://user-images.githubusercontent.com/27698263/203447092-d1f7a48c-466b-444b-8202-31b02ff95534.png)

## Is this related to any issues?

#662 - as above mentioned

## Have you read the [Contributing Guidelines](https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md)?

- [x] I have read the [Contributing Guidelines](https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md)

---

_@charliermarsh reviewed on 2022-11-23 03:07_

Nice! It seems like the code both got simpler _and_ there are meaningful performance gains here (I'm seeing a 10ms difference on the benchmark on my machine, which is a > 3% speed-up).

---

_Review comment by @charliermarsh on `src/settings/user.rs`:82 on 2022-11-23 03:10_

I think we should remove `Exclusion` and just inline the string, now that `basename` and `absolute` are identical.

---

_Review comment by @charliermarsh on `src/settings/user.rs`:51 on 2022-11-23 03:11_

I think we need to map these to strings somehow -- the current output of `cargo run resources/test/fixtures/ --show-settings` isn't very user-friendly:

![Screen Shot 2022-11-22 at 10 10 44 PM](https://user-images.githubusercontent.com/1309177/203462366-43255dde-4a8b-4b79-8fcb-4871486d711f.png)


---

_Review comment by @charliermarsh on `src/fs.rs`:163 on 2022-11-23 03:14_

Nit: can we add more expansive expectations here? Like `.expect("Failed to build GlobSet")`?

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:108 on 2022-11-23 03:25_

Maybe a little tighter as:

```rust
let extend_exclude = {
    let mut set = globset::GlobSetBuilder::new();
    for path in options.extend_exclude.unwrap_or_default() {
        set.add(from_user(&path, project_root)?);
    }
    set.build()?
};
```

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:47 on 2022-11-23 03:26_

I wonder if this should just be a `GlobSet`, rather than a vector of `Glob`?

---

_Review comment by @charliermarsh on `src/main.rs`:234 on 2022-11-23 03:26_

I think if we're going to `for`-loop here, maybe just make this part of the body, rather than doing a comprehension with the loop too. What do you think?

---

_Review comment by @charliermarsh on `src/settings/types.rs`:47 on 2022-11-23 03:27_

This name made more sense when it was part of `FilePattern`, since `FilePattern::from_user` had clear semantics. Maybe we rename to `create_glob` or `glob_from_user_pattern`?

---

_Review comment by @charliermarsh on `src/settings/types.rs`:62 on 2022-11-23 03:28_

I think this merits a comment. Is this creating a pattern that's effectively a union of the absolute and base patterns?

Would it be clearer to instead return two Globs, one for each of those two patterns?

---

_@charliermarsh reviewed on 2022-11-23 03:28_

---

_Comment by @charliermarsh on 2022-11-23 03:36_

Thank you for this PR! Let me know if you're up for tackling the comments.

---

_@CelebrateVC reviewed on 2022-11-23 03:39_

---

_Review comment by @CelebrateVC on `src/settings/user.rs`:51 on 2022-11-23 03:39_

Good points all around, I'll go over everything you have shared and take your advice on improvements, this especially should absolutely be cleaned up to something readable ( all those numbers are character codes so it shouldn't be too hard to get strings back out).

---

_Review comment by @charliermarsh on `src/settings/user.rs`:51 on 2022-11-23 03:44_

Sounds good! Thank you for your contribution :)

If at any point you feel like you won't have time to get to the comments, just let me know and we can figure out a plan.


---

_@charliermarsh reviewed on 2022-11-23 03:44_

---

_Review comment by @CelebrateVC on `src/settings/user.rs`:51 on 2022-11-23 19:50_

e4d537d and 9e59346 - Had to keep an uncompiled version of the globs to get the strings needed but settings now look much cleaner

---

_@CelebrateVC reviewed on 2022-11-23 19:50_

---

_Review comment by @CelebrateVC on `src/settings/configuration.rs`:108 on 2022-11-23 19:52_

 [f4660bd](https://github.com/charliermarsh/ruff/pull/883/commits/f4660bd72125f2fb61d8d07abfb3681483409dcb) - great suggestion

---

_@CelebrateVC reviewed on 2022-11-23 19:52_

---

_Review comment by @CelebrateVC on `src/settings/configuration.rs`:47 on 2022-11-23 19:56_

This *was* a great idea, except that we need to use the globs to make settings look nice, but I did clean this up to not include `Result` anymore just a `Lazy<Vec<Glob>>>`

---

_@CelebrateVC reviewed on 2022-11-23 19:56_

---

_@CelebrateVC reviewed on 2022-11-23 20:00_

---

_Review comment by @CelebrateVC on `src/main.rs`:234 on 2022-11-23 20:00_

Absolutely, the map was just an artifact of old code, fixed in 708d408 and further improved by cc32661

---

_@CelebrateVC reviewed on 2022-11-23 20:02_

---

_Review comment by @CelebrateVC on `src/settings/types.rs`:62 on 2022-11-23 20:02_

2 Globs made much more sense to me, so I basically reimplemented `FilePattern::Complex` in cc32661 🤣 

---

_@CelebrateVC reviewed on 2022-11-23 20:05_

---

_Review comment by @CelebrateVC on `src/fs.rs`:163 on 2022-11-23 20:05_

With the changes in f4660bd, this .expect is no longer needed. (or really it's now a `?` to waterfall errors up)

---

_Review comment by @CelebrateVC on `src/settings/user.rs`:82 on 2022-11-23 20:07_

cc32661 re-split basename and absolute, we should continue to use `Exclusions` 

---

_@CelebrateVC reviewed on 2022-11-23 20:07_

---

_Comment by @CelebrateVC on 2022-11-23 20:17_

I think that's all your reviews addressed. 

Benchmarks for me are still showing ~10 ms improvement even after changes. Do LMK if there are any other changes you'd like to see. I won't be on here over Thanksgiving weekend, but we can continue going back and forth on changes after that if any are needed. 

---

_Comment by @charliermarsh on 2022-11-24 04:24_

Great, thanks! I'll try to get this merged in the next few days.

---

_Merged by @charliermarsh on 2022-11-25 03:31_

---

_Closed by @charliermarsh on 2022-11-25 03:31_

---

_Comment by @charliermarsh on 2022-11-25 03:32_

@CelebrateVC - Thank you for this! I ended up making some tweaks to restore `FilePattern` (rather than using a tuple with `Option`), and moved the `GlobSet` stuff out of `Configuration` and into `Settings` which (I think) simplified a few things.

---

_Branch deleted on 2022-12-11 17:04_

---
