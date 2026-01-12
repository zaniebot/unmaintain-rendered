```yaml
number: 1170
title: Add --git-ignore-case-insensitive
type: pull_request
state: closed
author: davidtorosyan
labels: []
assignees: []
base: master
head: git_ignore_case_insensitive
created_at: 2019-01-21T07:13:18Z
updated_at: 2019-01-23T01:34:04Z
url: https://github.com/BurntSushi/ripgrep/pull/1170
synced_at: 2026-01-12T18:23:13Z
```

# Add --git-ignore-case-insensitive

---

_@davidtorosyan_

This closes #1164 

---

_Comment by @davidtorosyan on 2019-01-21 07:24_

A few things I'm unsure about:
1. The flag is named `--git-ignore-case-insensitive`, but I'm applying the case insensitivity to `.ignore` files as well. Do I need a more inclusive name?
2. Do I need a short name for the flag?
3. I'm not sure I understand the "overrides the X flag" comments. For example, `flag_heading` says `--heading` overrides `--no-heading`, and `--no-heading` overrides `--heading`. That doesn't make sense to me, but I followed the pattern and just had `--git-ignore-case-sensitive` take precedence over `-insensitive`, since that's the priority for `-c` and `-i`.

---

_Comment by @okdana on 2019-01-21 15:26_

I think that based on the names of the other ignore-file-related options it would probably just be like `--ignore-case-insensitive`, right? And you usually don't need to document the option that negates it separately, especially when it's the default behaviour; see how options like `--no-ignore-messages` and `--no-multiline-dotall` are implemented, for example. (That's related to the `$no` thing in the completion function)

---

_Comment by @davidtorosyan on 2019-01-21 18:47_

I considered `--ignore-case-insensitive`, but thought it might be confused as a variant of `--ignore-case`. I didn't use `$no` because `--ignore-case` and `--case-sensitive` don't.

Maybe `--ignore-iglob` and `--no-ignore-iglob`?

---

_Comment by @okdana on 2019-01-21 19:06_

Ah, i didn't consider the name confusion. Mixing the concepts of ignore files and command-line globbing seems a bit suspect to me as well, but i'm not sure.

I do feel like the option should be documented/completed more like `--[no-]ignore-messages` than `--case-insensitive` &al., though, in any case. Those match-case options aren't quite analogous (there's three of them, for one thing).

---

_Comment by @davidtorosyan on 2019-01-21 19:13_

Both good points.

I think the clearest then would probably be `--[no-]ignore-file-case-insensitive`, but I'll wait for @BurntSushi to weigh in before updating the PR.

---

_Closed by @BurntSushi on 2019-01-23 01:04_

---

_Comment by @BurntSushi on 2019-01-23 01:12_

@davidtorosyan Thanks so much for working on this! I think you got the general approach correct, but there were a lot of details that I wanted to iron out, so I just did them myself here in https://github.com/BurntSushi/ripgrep/commit/718a00f6f2f88238546f7d33c1ea52217002495e. You can see my changes off your PR here: https://gist.github.com/BurntSushi/6456ad9be5ce650fa36bf02956cee14e

In summary:

* There were some formatting issues that were inconsistent with the rest of the code. In particular, there were some very long lines. I'd like to adhere to 79 columns inclusive.
* There were several breaking changes made to the public API of the `ignore` crate. This is not something we should do lightly. I do see why you did it though, because avoiding that did require a touch of refactoring that wasn't at all obvious. (`ignore` badly needs a facelift.)
* I decided on just using `--ignore-file-case-insensitive` as the flag name in order to avoid confusion with `--ignore-case`.
* The flag overrides just mean that all/most flags can be overridden. Following the pattern of `--ignore-case` is understandable, but `--case-sensitive` was added before we started getting more rigorous about creating flags for disabling other flags. These days, we consistently try to just prefix flags with a `--no-` in order to indicate disabling them. Moreover, since most flags have that variant, we hide them from the listing of flags since there are so many.
* Short names are in short supply. This definitely does not deserve a short name. There are literally only a few short flag names left.

I think that's it! Nice work!

---

_Comment by @davidtorosyan on 2019-01-23 01:30_

@BurntSushi thanks for the detailed feedback (and for merging the feature).

It didn't register to me that the signature for `case_insensitive` was public, I guess that means I've got to get more familiar with Rust!


---

_Branch deleted on 2019-01-23 01:30_

---

_Comment by @BurntSushi on 2019-01-23 01:34_

@davidtorosyan Yup! Basically, you want to take a look at the public API in the docs, e.g., https://docs.rs/ignore. Your changes made sense though---those methods should not have been returning `Result`s. I added a TODO comment to update them on the next breaking release.

---
