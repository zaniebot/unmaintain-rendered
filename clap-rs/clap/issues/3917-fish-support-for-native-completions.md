---
number: 3917
title: fish support for native completions 
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-completion
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $20"
assignees: []
created_at: 2022-07-13T14:16:21Z
updated_at: 2025-03-28T14:05:21Z
url: https://github.com/clap-rs/clap/issues/3917
synced_at: 2026-01-10T01:27:49Z
---

# fish support for native completions 

---

_Issue opened by @epage on 2022-07-13 14:16_

See #3166 for more context

- [code](https://github.com/clap-rs/clap/blob/master/clap_complete/src/env/shells.rs)
- [tests](https://github.com/clap-rs/clap/blob/master/clap_complete/tests/testsuite/fish.rs)
- [argcomplete](https://pypi.org/project/argcomplete/) and [cobra](https://github.com/spf13/cobra) can serve as examples

Tasks
- [x] #5048 
- [ ]  Identify and resolve gaps with static completions

---

_Label `C-enhancement` added by @epage on 2022-07-13 14:16_

---

_Label `A-completion` added by @epage on 2022-07-13 14:16_

---

_Label `E-easy` added by @epage on 2022-07-13 14:16_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2022-07-13 14:20_

---

_Referenced in [clap-rs/clap#4177](../../clap-rs/clap/pulls/4177.md) on 2022-09-02 18:16_

---

_Comment by @ModProg on 2022-09-02 18:18_

I wanted to start on this, looking at the current structure with bash, would it make sense to move the bash mod into a separate file, and could/should some of the bash code be reused instead of duplicated, and therefore extracted into a shared module for utils?

---

_Comment by @epage on 2022-09-02 18:26_

I mostly kept it a single file for ease of prototyping.  It will make sense to split it into another file.  Please do that in a separate commit from other work so its easier to track whats going on

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:35_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:29_

---

_Comment by @ModProg on 2022-10-03 11:28_

Have the PR setup now for review. Still open things, especially in the handling of more complex functionality in handling parsing errors or arguments taking multiple values.

Also there might be some ways to unify code with claps actual parsing logic, but as I'm not familiar with it, I skipped that for now.

---

_Comment by @epage on 2022-10-03 19:29_

> Also there might be some ways to unify code with claps actual parsing logic, but as I'm not familiar with it, I skipped that for now.

Yeah, I hope to find a way to merge it one day but I figure its better for us to learn what is expected first

---

_Comment by @ModProg on 2022-10-04 10:08_


> Yeah, I hope to find a way to merge it one day but I figure its better for us to learn what is expected first

Yeah makes sense. There is probably also a bunch of parsing/error handling that can be skipped for completions.

---

_Referenced in [clap-rs/clap#5048](../../clap-rs/clap/pulls/5048.md) on 2023-07-28 10:16_

---

_Comment by @ModProg on 2023-07-31 18:38_

- [ ] fish integration #5048 
- [ ] make globs in paths work with completion:
  - complete containing globs: `*/ba` would complete to `*/bash.rs` and `*/banana.rs` (given the paths `a/bash.rs`, `a/banana.rs`, and `b/bash.rs` exist)
  - expand globs `a-*` would expand to `a-b a-c`.
  - complete substrings (not only prefixes) `rc/he/ba` would complete to `src/shell/bash.rs`.
- [ ] only complete options if current argument starts with `-` or `--` #5058
- [ ] show help in completions #5056

---

_Referenced in [MilesCranmer/rip2#39](../../MilesCranmer/rip2/issues/39.md) on 2024-06-25 21:22_

---

_Referenced in [clap-rs/clap#3916](../../clap-rs/clap/issues/3916.md) on 2025-02-07 14:56_

---
