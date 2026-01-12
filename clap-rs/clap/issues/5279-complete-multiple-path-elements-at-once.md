```yaml
number: 5279
title: Complete multiple path elements at once
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-completion
  - E-easy
assignees: []
created_at: 2024-01-02T19:58:34Z
updated_at: 2025-08-28T17:11:23Z
url: https://github.com/clap-rs/clap/issues/5279
synced_at: 2026-01-12T16:14:16Z
```

# Complete multiple path elements at once

---

_@epage_

#3166 is adding Rust-implemented completions.  In reviewing nu's changelog, they added support for offering `./target/debug/incremental` as a completion for `tar/de/inc` (see https://www.nushell.sh/blog/2023-10-17-nushell_0_86.html#improving-the-completions-in-the-repl-toc).  It could be really nice for us to include that as well.

---

_Label `C-enhancement` added by @epage on 2024-01-02 19:58_

---

_Label `A-completion` added by @epage on 2024-01-02 19:58_

---

_Label `E-easy` added by @epage on 2024-01-02 19:58_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2024-01-02 19:58_

---

_Renamed from "Path abbreviation completions" to "Complete multiple path elements at once" by @epage on 2024-01-02 19:59_

---

_Comment by @nair-sreerag on 2025-04-13 16:39_

Hello @epage, 
I went through the nushell demo (the gif demonstrating the feature).I would like to pick this issue up but  i have a few questions before I start.

```
root
    - target1
        - src1
            - main.rs
        - src2
            - main.rs
    - target2
        - src
            - main.rs
        - src2
            - main.rs
    - target3
        - src
            - main.rs
        - src2
            - main.rs
```

If I write  **./ta/src/m** and press TAB, it should show a list of 6 entries. Now this is a small generated list. But in real life scenarios, the list generated can be huge. So should there be a limit to the list that can be displayed? If yes, then how much? 

And on what ordering condition should the list be generated? I am thinking alphabetical, but would like to hear your thoughts.


One small help I would like is to understand the usecase of this feature that I will be building. Are there any related examples in the codebase that I can play around with??

---

_Comment by @epage on 2025-04-14 15:16_

> If I write ./ta/src/m and press TAB, it should show a list of 6 entries. Now this is a small generated list. But in real life scenarios, the list generated can be huge. So should there be a limit to the list that can be displayed? If yes, then how much?

I wouldn't limit the list.  If the person is in a situation where its too much, they can expand one of the sections to be less ambiguous

> And on what ordering condition should the list be generated? I am thinking alphabetical, but would like to hear your thoughts.

I would prioritize matches where a path element has an exact match.  The more exact matches, the higher the priority.  Maybe we could just put this in the `display_order` and have `clap_complete` do the ordering for us.  Huh, why is that a `usize`?  Could be much easier as an `isize`.

---

_Comment by @epage on 2025-04-14 15:29_

> One small help I would like is to understand the usecase of this feature that I will be building. 

So I've not used it with nushell and fish (which also has it) but my assumption is that this is to allow short-hands without explicitly tabbing / selecting at every level.

Say I'm using `cargo` in the root of https://github.com/toml-rs/toml

```
root
- crates
  - benchmarks
    - Cargo.toml
  - serde_spanned
    - Cargo.toml
  - serde_toml
    - Cargo.toml
  - toml
    - Cargo.toml
  - toml_datetime
    - Cargo.toml
  - tom,_edit
    - Cargo.toml
  - toml_edit_fuzz
    - Cargo.toml
```

I could then do
```console
$ cargo check --manifest-path c/b/C[TAB]
$ cargo check --manifest-path crates/benchmarks/Cargo.toml
$ cargo check --manifest-path c/s/C[TAB]
crates/serde_spanned/Cargo.toml
crates/serde_toml/Cargo.toml
$ cargo check --manifest-path c/t/C[TAB]
crates/toml/Cargo.toml
crates/toml_datetime/Cargo.toml
crates/toml_edit/Cargo.toml
crates/toml_edit_fuzz/Cargo.toml
```

---

_Comment by @nair-sreerag on 2025-04-19 20:19_

Thanks for the reply, @epage. My question was more of as to how this feature will be used in clap. I wanted an example (or more like a short tutorial) on its usage in clap




---

_Comment by @nair-sreerag on 2025-04-19 21:35_

Is there some kind of implementation for such completion; like in my example above, if i enter **ro**[TAB], it should complete it to **root/** .Pressing [TAB] again will list all the files and folders in root.

Again, I am asking this because we are emulating this feature from nushell which is a full fledged shell like zsh or bash whereas clap is not a shell, its an argument parser. So I am having difficulty fitting this path expansion feature in clap.

---

_Comment by @epage on 2025-04-21 14:38_

All this issue is responsible for is updating `complete_path` to report all `CompletionCandidate` assuming that each path component is a prefix (prioritizing exact matches)
https://github.com/clap-rs/clap/blob/f88be5738e33018f3298fabb7b67835eefbc55e0/clap_complete/src/engine/custom.rs#L285-L348

Any other behavior is either owned by the shell or the completion engine itself.

---

_Comment by @Unique-Usman on 2025-08-27 20:19_

Hi @epage, 

I will be working on this simultaneously with the other issue.

Thank you.

Also, is it okay to model our solution just like the way Nushell implemented theirs ? 

Thank you. 

---

_Comment by @epage on 2025-08-28 15:19_

> Also, is it okay to model our solution just like the way Nushell implemented theirs ?

As I mentioned, I'm not fully aware of how nushell does it.  I think I heard that fish will take `cb/ad` and will check for any  directory that matches `.*c.*b.*` and then any file that matches `.*a.*d.* and this frustrated people because it caused irrelevant matches.

I would lean towards prefix checks (`cb/ad` -> `cb.*` and `ad.*).  That could be annoying if there is a long common prefix to a path element and a lot in common among the children directories.

---

_Comment by @Unique-Usman on 2025-08-28 17:11_

Ok, thank you, I will work towards implementing the prefix check. 

---
