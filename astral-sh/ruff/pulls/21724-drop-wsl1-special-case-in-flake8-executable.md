```yaml
number: 21724
title: Drop WSL1 special case in flake8-executable
type: pull_request
state: open
author: K900
labels: []
assignees: []
base: main
head: wsl1-is-very-dead
created_at: 2025-12-01T11:31:17Z
updated_at: 2025-12-12T10:11:18Z
url: https://github.com/astral-sh/ruff/pull/21724
synced_at: 2026-01-10T16:42:11Z
```

# Drop WSL1 special case in flake8-executable

---

_Pull request opened by @K900 on 2025-12-01 11:31_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

WSL1 is very very dead, and WSL2 handles permissions properly. Fixes `cargo test` on Ruff itself failing on WSL2, and removes what's generally a high potential footgun.

Fixes #10084 in a simpler way than #17584.

## Test Plan

`cargo test` on a WSL2 system.

---

_Review requested from @ntBre by @amyreese on 2025-12-01 18:23_

---

_@amyreese approved on 2025-12-01 18:24_

---

_Comment by @ntBre on 2025-12-01 19:04_

This feels like it could be a breaking change to me (so we could try it in preview first), but I'm curious what others think. We may also need to update the rule docs, which both mention WSL.

Otherwise the change looks reasonable to me, and it's nice to drop a dependency!

cc @MichaReiser who reviewed #17584.

---

_Comment by @K900 on 2025-12-01 20:18_

Oops, missed the doc change. Fixed it now.

---

_Comment by @MichaReiser on 2025-12-02 09:22_

@K900 would you mind testing the example in https://github.com/astral-sh/ruff/issues/5445


I don't use WSL myself, so I'm not very familiar with how it works but what I understand is that all files on a windows disk `/mnt/...` always have the executable bit set, in which case we'd flag every single file on that disk if they miss a shebang. 

I agree, that we should preview gate this change.

---

_Comment by @K900 on 2025-12-02 09:27_

It's a little more complicated than that on WSL2. Linux permissions are persisted on Windows partitions, as NTFS xattrs, but files without any xattrs, e.g. ones created on the Windows side, are defaulted to 0777. So if you, say, run `git clone` on Windows to clone a repo and then try to access it on WSL, it will have 0777 permissions on everything, but if you `git clone` the same repo to the same path from the WSL side, it will have correct permissions on both sides. This is not entirely obvious behavior, but it will also throw off git (if used from WSL) and basically any other tool, and I don't think Ruff has to try and work around this.

---

_Comment by @K900 on 2025-12-02 09:29_

To clarify:

```
~
❯ cd /mnt/d/test/

/mnt/d/test
❯ ls
╭────────────╮
│ empty list │
╰────────────╯

/mnt/d/test
❯ touch beep

/mnt/d/test
❯ /mnt/c/Windows/System32/cmd.exe /c "echo > boop"  # this escapes back to Windows and creates a file "from Windows"

/mnt/d/test
❯ ls -la
╭────┬──────┬──────┬────────┬──────────┬───────────┬───────────┬────────────────────┬──────┬───────┬──────┬─────────┬─────╮
│  # │ name │ type │ target │ readonly │   mode    │ num_links │       inode        │ user │ group │ size │ created │ ... │
├────┼──────┼──────┼────────┼──────────┼───────────┼───────────┼────────────────────┼──────┼───────┼──────┼─────────┼─────┤
│  0 │ beep │ file │        │ false    │ rw-r--r-- │         1 │ 143552238122452878 │ k900 │ users │  0 B │         │ ... │
│  1 │ boop │ file │        │ false    │ rwxrwxrwx │         1 │ 133137663984158607 │ k900 │ users │ 13 B │         │ ... │
╰────┴──────┴──────┴────────┴──────────┴───────────┴───────────┴────────────────────┴──────┴───────┴──────┴─────────┴─────╯

/mnt/d/test
❯ chmod 0666 boop

/mnt/d/test
❯ ls -la
╭────┬──────┬──────┬────────┬──────────┬───────────┬───────────┬────────────────────┬──────┬───────┬──────┬─────────┬─────╮
│  # │ name │ type │ target │ readonly │   mode    │ num_links │       inode        │ user │ group │ size │ created │ ... │
├────┼──────┼──────┼────────┼──────────┼───────────┼───────────┼────────────────────┼──────┼───────┼──────┼─────────┼─────┤
│  0 │ beep │ file │        │ false    │ rw-r--r-- │         1 │ 143552238122452878 │ k900 │ users │  0 B │         │ ... │
│  1 │ boop │ file │        │ false    │ rw-rw-rw- │         1 │ 133137663984158607 │ k900 │ users │ 13 B │         │ ... │
╰────┴──────┴──────┴────────┴──────────┴───────────┴───────────┴────────────────────┴──────┴───────┴──────┴─────────┴─────╯


---

_Comment by @MichaReiser on 2025-12-02 09:31_

I'm not yet convinced that we should make this change but we should explain this caveat in the documentation and explain to users how they can "fix their setup" if they run into this situation (clone from within WSL)

---

_Comment by @K900 on 2025-12-02 09:34_

I don't know if I agree, to be honest. This is kind of similar levels of user error as, say, putting stuff on a noexec filesystem, or on a FAT32 or something. There's really no good way to _actually know_ if the filesystem has sane semantics. Maybe some sort of global detection that skips the check if all files come back executable? But even that feels iffy.

---

_Comment by @MichaReiser on 2025-12-02 09:47_

I'm not sure if you're misunderstanding me. What I'm suggesting is that we make this change in preview, but we also extend the rule documentation with a `WSL` section, explaining in which situations they might see violations for every single file and what they can do to fix it. I don't see what the issue is with helping users in case they see confusing behavior and I would also find this documentation useful for myself, if a user asks for help in the future.

The use case I'm worried about is users who switch between WSL and non-WSL, creating some files in Windows and others in WSL, but flake8-executable is enforced in their organisation. They would have to change their behavior. We could consider recommending that they turn off the rule at a user-level configuration.

---

_Comment by @K900 on 2025-12-02 09:58_

The thing here is that WSL preserves permissions correctly, if set on the WSL side, so just running autofix should repair any sort of WSL setup, _even if the files are then updated from the Windows side_, because xattrs stay. (wait, does this rule even have an autofix? maybe it should if it doesn't yet) However, there are also other kinds of weird setups that are already not caught by this check, and I'm not sure there's a good way to identify those in a general enough way to try and be smart about this particular check.

---

_Comment by @MichaReiser on 2025-12-02 10:15_

We don't necessarily have to agree here, but I see some documentation giving users a hint on how to solve this problem that users have run into with Ruff in the past, which is a prerequisite to merging.

---

_Comment by @K900 on 2025-12-02 10:23_

Sorry, to be clear, I'm not saying this exact change shouldn't have documentation, I'm kind of thinking out loud about whether there is a way to implement this that doesn't _need_ documentation. What do you think about adding an autofix for this, and then maybe explicitly documenting something like "if the autofix didn't work, your filesystem is probably being weird"?

---

_Comment by @MichaReiser on 2025-12-02 13:04_

I see. 

I don't see an obvious way for Ruff to detect this, given that it depends on where a file was created. I'm leaning towards making the change you suggestg with better documentation and we can always introduce a `skip_on_wsl` option

---

_Comment by @K900 on 2025-12-02 13:07_

Do you mean just this change, or also adding an auto fix? I can look into doing that in a couple hours. 

---

_Comment by @MichaReiser on 2025-12-02 13:11_

Just this change. Adding an auto fix seems unrelated to this PR

---

_Comment by @K900 on 2025-12-04 09:41_

The autofix is a good way to fix a messed up repo, possibly, if you cloned from Windows and ran ruff from WSL. Also, I wonder how hard it'll be to have git support the WSL xattrs... That would solve most issues before they even happen.

---

_Comment by @K900 on 2025-12-09 15:44_

Added some documentation that is hopefully generic enough to catch most causes of false positives. Sorry for the delay, caught a cold :(

---

_Comment by @K900 on 2025-12-09 15:45_

(and yes, NTFS filesystems mounted to WSL technically _do_ support Unix permissions, but this is a can of worms that is probably out of scope for Ruff rule documentation)

---

_@MichaReiser reviewed on 2025-12-11 09:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2025-12-11 09:25_

I understood that it's less about where the file is stored but more about how the file was created (WSL side or from within windows). 

Should we also document how users can fix their file system state, e.g. by running chmod from within WSL and setting the file to its default permissions?

---

_Comment by @MichaReiser on 2025-12-11 09:26_

> Added some documentation that is hopefully generic enough to catch most causes of false positives. Sorry for the delay, caught a cold :(

No need to feel sorry. Thanks for looking into this. I find it very helpful to have someone with deep WSL understanding to talk this change through.

---

_@K900 reviewed on 2025-12-11 09:30_

---

_Review comment by @K900 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2025-12-11 09:30_

It's kind of both, because WSL is not the only failure mode here, it's also native Linux drivers for filesystems that don't have the concept of POSIX permission bits (vfat etc). WSL in particular is extra weird because it stores POSIX permissions bits in NTFS xattrs and just assumes 0755 if the xattr isn't set, so it's extra confusing and Windows applications touching those files can also make xattrs be weird and I don't know if trying to explain all of this in a note for this lint is a good idea.

---

_@K900 reviewed on 2025-12-11 09:32_

---

_Review comment by @K900 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2025-12-11 09:32_

Or at least I really have no idea how to communicate this in a way that's both easy to understand and _actually technically correct_, which is what made me go for the "Just Don't" hint.

---

_@MichaReiser reviewed on 2025-12-12 10:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2025-12-12 10:09_

I see. I think my conclusion is we can't make this change and instead need a setting or we risk breaking users that use WSL in combination with such a file system.

---

_@K900 reviewed on 2025-12-12 10:11_

---

_Review comment by @K900 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2025-12-12 10:11_

It's not just WSL, you can mount a vfat partition on normal Linux, put a repo on it and it'll all be 0755 no matter what, because vfat just makes up the permission bits. I don't know if WSL will even _let you_ access non-NTFS partitions over drvfs?

---
