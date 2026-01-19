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
updated_at: 2026-01-19T13:26:21Z
url: https://github.com/astral-sh/ruff/pull/21724
synced_at: 2026-01-19T14:36:03Z
```

# Drop WSL1 special case in flake8-executable

---

_@K900_

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

_Comment by @MusicalNinjaDad on 2026-01-11 09:02_

I hope it's OK to chime in here as the author of #10084 and someone with a lot of experience both using WSL and looking at potential solutions to this issue.

@ntBre, you are correct - this *is* a breaking change and will cause false positives for a subset of users, where the status quo causes false negatives for a different subset of users. (I'll add a table summary to #10084 and copy it here too)

@MichaReiser's original argumentation when discussing this was that false negatives are worse, as settings are mainly on a per-project basis - so it's hard to disable the rule only for some contributors.

@K900 - this is definitely the easiest of "solutions" and I agree with you, and initially argued myself, that it has long been a strong recommendation by MS, that WSL users avoid using mounted windows partitions for performance reasons (see the note at the end of the description on #17584 for example timings)

My personal opinion, FWIW, is that identifying default file permissions is the best: covers all use-cases, actually improves performance. (This is not what I first thought, when beginning to discuss and look at this issue; I actually agreed with the approach proposed in the PR when I first opened the issue).

---

_@MusicalNinjaDad reviewed on 2026-01-11 09:09_

---

_Review comment by @MusicalNinjaDad on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:35 on 2026-01-11 09:09_

To be absolutely clear: the underlying problem is purely a **file-system issue** and in **no way inherent to WSL**.

The reason it comes up in relation to WSL is that this is the most common real-life scenario of a linux user storing their code on an FS without *nix permission bits *and* where the user doesn't understand the consequences. (A linux user storing code on an xFAT UBS stick is more likely to understand why they are getting this problem and simply ignore it - but see #12941)

---

_Comment by @K900 on 2026-01-11 09:12_

The problem I have with identifying file permissions is that to do that we need to:

- find a place to write a test file to (we can't always do that)
- be reasonably confident that the filesystem will behave the same way as the file we just wrote (drvfs doesn't)

Edit: to be clear, #17584 does not in fact work for the "WSL, source root on drvfs, cloned from Windows" use case, because it will create a file with 644 permissions, drvfs will record that and give us back the correct permissions, but none of the other files will have the correct xattrs.

---

_Comment by @MusicalNinjaDad on 2026-01-11 09:21_

Overview of currently proposed solutions:

| | WSL user, WSL FS (#10084) | Linux user USB stick (#12941) | WSL user, Win FS (#3110, #5445) |
| -- | -- | -- | -- |
| Status Quo | :x: (false negatives) | :x: (false positives) | :x: (false negatives) |
| #17548 | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| #21724 | :white_check_mark: | :x: (false positives) | :x: (false positives) |

https://github.com/astral-sh/ruff/issues/10084#issuecomment-3734292000

---

_Comment by @MusicalNinjaDad on 2026-01-11 09:32_

> The problem I have with identifying file permissions is that to do that we need to:
> 
> * find a place to write a test file to (we can't always do that)
> * be reasonably confident that the filesystem will behave the same way as the file we just wrote (drvfs doesn't)

Did you take a look at the approach in the other PR?

1. check if pryproject.toml is executable (if not, then we can guarantee that we have a settable executable-bit)
2. touch a file in project_root (could fail if project root is read-only, although this is a more acceptable edge case than current status quo and only triggers on FS with no exe-bit)

What specifically do you have in mind by cases where we can't write a test file?

I've not tested drvfs, do you have a test setup to see whether #17584 fails in that case? My understanding is that it is only relevant for WSL1 (as you say, now dead) and the above test would disable the lints at step 1 anyway.

---

_Comment by @K900 on 2026-01-11 09:36_

drvfs is still what WSL2 calls the 9p stuff internally, the case I'm imagining is: user clones repo from Windows, then runs ruff in WSL. What happens then is:

- pyproject.toml is executable, so we continue
- we touch a file in project root, which is 644 by default because umask, that's recorded in NTFS xattrs, reading back the permissions gives us 644
- all the other files are still marked executable because they have no xattrs

---

_Comment by @MusicalNinjaDad on 2026-01-11 09:45_

> drvfs is still what WSL2 calls the 9p stuff internally, the case I'm imagining is: user clones repo from Windows, then runs ruff in WSL. What happens then is:
> 
> * pyproject.toml is executable, so we continue
> * we touch a file in project root, which is 644 by default because umask, that's recorded in NTFS xattrs, reading back the permissions gives us 644
> * all the other files are still marked executable because they have no xattrs

imagining or tested?

I have tested exactly this setup both locally, and in CI (https://github.com/MusicalNinjaDad/forks-ruff/actions/runs/14663820841/job/41154168824) and in both cases the touched file is executable and the lints are disabled.

If you have a specific setup which leads to the behaviour you describe then I'd be really interested to hear the config in place, both from general curiosity and to help with this case.

(EDIT: just tested this manually, again, on WSL2 with `cd /mnt/c/Users.../Temp; touch foo; ls -al foo` and `foo` has `0777` permissions)

---

_Comment by @K900 on 2026-01-11 09:52_

<img width="1860" height="303" alt="image" src="https://github.com/user-attachments/assets/0672d31d-50cf-4aca-948e-77978a60b5d3" />

On my end, the file touched from WSL is 0644.

---

_Comment by @K900 on 2026-01-11 10:00_

Also here's a run with ruff from your branch (and a few dbg's sprinkled in):

```
d/test/chinatsu-master
❯ ~/ruff/target/debug/ruff check --no-cache --select EXE
[crates/ruff_linter/src/rules/flake8_executable/helpers.rs:29:9] is_executable(&settings.project_root.join("pyproject.toml")) = Ok(
    true,
)
[crates/ruff_linter/src/rules/flake8_executable/helpers.rs:33:37] is_executable(tmpfile.path()) = Ok(
    false,
)
chinatsu/__init__.py:1:1: EXE002 The file is executable but no shebang is present
(etc)
```

---

_Comment by @K900 on 2026-01-11 10:01_

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_executable/helpers.rs b/crates/ruff_linter/src/rules/flake8_executable/helpers.rs
index 717c805721..47e1e7352d 100644
--- a/crates/ruff_linter/src/rules/flake8_executable/helpers.rs
+++ b/crates/ruff_linter/src/rules/flake8_executable/helpers.rs
@@ -26,11 +26,11 @@ static EXECUTABLE_BY_DEFAULT: OnceLock<bool> = OnceLock::new();
 #[cfg(target_family = "unix")]
 pub(super) fn executable_by_default(settings: &LinterSettings) -> bool {
     *EXECUTABLE_BY_DEFAULT.get_or_init(|| {
-        is_executable(&settings.project_root.join("pyproject.toml")).unwrap_or(true)
+        dbg!(is_executable(&settings.project_root.join("pyproject.toml"))).unwrap_or(true)
             // if pyproject.toml is executable or doesn't exist, run a slower check too:
             && NamedTempFile::new_in(&settings.project_root)
                 .map_err(std::convert::Into::into)
-                .and_then(|tmpfile| is_executable(tmpfile.path()))
+                .and_then(|tmpfile| dbg!(is_executable(tmpfile.path())))
                 .unwrap_or(false) // Assume a normal filesystem in case of read-only, IOError, ...
     })
 }
```

(for posterity)

---

_Comment by @MusicalNinjaDad on 2026-01-11 10:09_

Thanks for the confirmation.

Did you adjust your `[automount]` settings in `wsl.conf` to enable metadata and set a specific umask? Or are you running with default config and getting these results?

---

_Comment by @K900 on 2026-01-11 10:10_

I have `options=metadata,uid=1000,gid=100`, the uid/gid stuff is just NixOS weirdness for historical reasons.

---

_Comment by @MusicalNinjaDad on 2026-01-11 14:25_

Thanks - think I've got it now... I see that NixOS [defaults metadata enabled](https://nix-community.github.io/NixOS-WSL/options.html#wslwslconfautomountenabled), unlike the [base WSL default](https://learn.microsoft.com/en-us/windows/wsl/wsl-config?source=recommendations#automount-options), and for various reasons you have chosen/need to keep your code on an NTFS partition rather than the native linux one.

1. I'm assuming from your comments above that you usually clone with linux git. So in your case your system usually behaves like the native WSL case and either of the PRs give you valid lints, status quo gives you potentially false negatives (?)

2. I don't have git for windows installed, but from looking at [your example under `d/test/chaintsu-master`](https://github.com/astral-sh/ruff/pull/21724#issuecomment-3734341168) it appears that cloning to an NTFS partition under windows doesn't add xattr info to maintain the *nix exe-bit. I'm assuming this is not your usual workflow(?)
    - I'd expect that this could get messy quickly, as any future `git pull`s inside WSL will update file permissions for added/changed files that it retrieves from the remote, and any new files will not have 0644 perms. These perms will persist thanks to the metadata setting. **<-- _Could you confirm that easily?_**
    - As you say, there's no decent heuristic available to help in this situation. So the only option would be a command-line-option/environment-variable. I looked into this and dropped it as doing it nicely and well documented affects a lot more code and forces the option to also be exposed via the config files. It's doable but at a higher cost to the maintainers ...
    - There is an argument to say that anyone working this way hopefully understands the consequences of their setup choices and will be in a small minority. Personally, I never like taking that attitude if it can be avoided ...

If I've understood things correctly this means we have the following situation:
| | WSL user, WSL FS (#10084) | Linux user USB stick (#12941) | WSL user, Win FS (#3110, #5445) | WSL user, metadata (@k900) | WSL user, metadata, cloned on Win |
| -- | -- | -- | -- | -- | -- |
| Status Quo | :exclamation: (false negatives) | :x: (false positives) | :exclamation: (false negatives) | :exclamation: (false negatives) | :exclamation: (false negatives) |
| #17548 | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :exclamation: (false negatives) |
| #21724 | :white_check_mark: | :x: (false positives) | :x: (false positives) | :white_check_mark: | :exclamation: (false negatives) |

---

_Comment by @K900 on 2026-01-11 14:31_

To be clear, this is not my actual setup that I actually use, it's a setup that I assume people ran into which led to the initial change. However, it's also yet another proof that guessing "is the filesystem broken" in the general case is impossible (even ignoring all kinds of obviously-pathological things like FileStation 9000 which returns random permission bits on every read).

My normal usage of Ruff doesn't actually trigger this bug - I've run into it because I occasionally build NixOS from source on my WSL systems, and Ruff fails _in its own tests_ because those are not designed to run under WSL.

Also, Git knows nothing about the NTFS Unix-permission xattrs, so it never writes or updates them for any reason, meaning newly added files (if pulled from Windows) will be 0755, and any updated files will keep their existing permissions (unless moved, possibly?). Teaching Git about these xattrs is another thing that could probably improve things a lot, but not my can and not my worms.

---

_Comment by @MusicalNinjaDad on 2026-01-12 08:04_

Thanks. Yeah, no heuristic will be perfect for this. I'd see the "WSL user, metadata enabled, cloned on Win" as an unusual edge case which requires someone to actively adjust settings from defaults AND work against recommended practices, with consequences far beyond these lints - so reasonable to assume "they know what they're doing".

Could you confirm the below table matches your experience and I'll update the issue to reflect it:

| | WSL user, WSL FS (official MS recommendation) (#10084) | WSL user, NTFS metadata enabled (e.g. NixOs) [see #21724](https://github.com/astral-sh/ruff/pull/21724#issuecomment-3734350142) | Linux user exFAT USB stick (#12941) | WSL user, Win FS (#3110, #5445) |
| -- | -- | -- | -- | -- |
| Status Quo | :exclamation: (disabled) | :exclamation: (disabled) | :x: | :exclamation: (disabled) |
| #17548 | :white_check_mark: | :white_check_mark: | :exclamation: (disabled) | :exclamation: (disabled) |
| #21724 | :white_check_mark: | :white_check_mark: | :x: | :x: |

---

_Comment by @K900 on 2026-01-12 08:17_

This seems accurate yeah. I hate how the table just keeps growing... 

---

_Comment by @K900 on 2026-01-18 08:27_

So folks, should we make some kind of call here? I'm still leaning towards just removing the check and documenting it better that cursed filesystem setups are the end user's problem.

---

_Comment by @MichaReiser on 2026-01-18 10:50_

> So folks, should we make some kind of call here? I'm still leaning towards just removing the check and documenting it better that cursed filesystem setups are the end user's problem.

I don't feel a strong need to make a change here, especially since this has a very high chance of breaking someone's setup. 

I'd be open to adding a config option that allows opting in on WSL, and extending the documentaiton

---

_Comment by @K900 on 2026-01-19 10:11_

The other issue, and the one that actually led me to discovering this behavior, is that running `cargo test` on Ruff itself breaks on WSL, because it makes this assumption and the tests aren't designed for it. This could be fixed by skipping the tests on WSL, I guess?

---

_Comment by @MichaReiser on 2026-01-19 10:19_

> The other issue, and the one that actually led me to discovering this behavior, is that running cargo test on Ruff itself breaks on WSL, because it makes this assumption and the tests aren't designed for it. This could be fixed by skipping the tests on WSL, I guess?

Yes, I consider this to be a separate issue from changing the rule's behavior. Skipping it on WSL seems fine and something we can do in a seperate PR

---

_Comment by @K900 on 2026-01-19 13:26_

OK, so here's skipping the tests, for now: https://github.com/astral-sh/ruff/pull/22721

---
