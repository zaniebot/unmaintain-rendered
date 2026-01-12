```yaml
number: 1847
title: "Help text for --hidden should clarify what \"hidden\" means"
type: issue
state: closed
author: dbjorge
labels:
  - doc
assignees: []
created_at: 2021-04-15T19:37:15Z
updated_at: 2021-04-15T23:21:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1847
synced_at: 2026-01-12T16:13:24Z
```

# Help text for --hidden should clarify what "hidden" means

---

_@dbjorge_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Issue repros with both `scoop install ripgrep` and `cargo install ripgrep` versions

#### What operating system are you using ripgrep on?

Windows 10 Version 20H2 (OS Build 19042.867)

#### Describe your bug.

On Windows, ripgrep (via `ignore`) considers files which have the "hidden" attribute OR whose basename starts with a `.` to be "hidden" for the purposes of the `--hidden` flag

https://github.com/BurntSushi/ripgrep/blob/64ac2ebe0f2fe1c8967e7ec550bc32466bf40a07/crates/ignore/src/pathutil.rs#L22-L46

I can understand why ripgrep might want to ignore `.dotfiles` on Windows for consistency (and that changing it now would be breaking), but as a Windows user, I was surprised by this behavior because it uses a meaning of the term "hidden" which is different from the OS's without documenting that it's going to do so. It would be nice if `rg --help`'s `--hidden` section could clarify what ripgrep considers to be a "hidden" file, similar to the comment in `pathutil.rs`.

#### What are the steps to reproduce the behavior?

```
> rg --help | rg -A 7 -e --hidden
```

#### What is the actual behavior?

```
        --hidden
            Search hidden files and directories. By default, hidden files and directories
            are skipped. Note that if a hidden file or a directory is whitelisted in an
            ignore file, then it will be searched even if this flag isn't provided.

            This flag can be disabled with --no-hidden.

        --iglob <GLOB>...
```

#### What is the expected behavior?

Something along the lines of

```
        --hidden
            Search hidden files and directories. By default, hidden files and directories
            are skipped. Note that if a hidden file or a directory is whitelisted in an
            ignore file, then it will be searched even if this flag isn't provided.

            A file or directory is considered hidden if its base name starts with a dot
            character ('.'). On operating systems which support a `hidden` file attribute,
            like Windows, files with this attribute are also considered hidden.

            This flag can be disabled with --no-hidden.
```

My initial attempts at searching through the ripgrep documentation to understand why it was skipping `.directories` by default involved `rg --help | rg -e dot`, which pointed me to the `--no-ignore-dot` flag (which sounds promising but isn't actually relevant to ignoring dotfiles generally). If that feels like a mistake that other users might reasonably be expected to make, it might be worth additionally updating the `--no-ignore-dot` help section with a pointer along the lines of `This does **not** affect whether ripgrep will ignore files and directories whose name begins with a dot. For that, see --hidden and --no-hidden.`

I also noticed that the `--no-hidden` flag the `--hidden` section mentions doesn't have a top-level help entry; not sure if that's intentional.


---

_Comment by @BurntSushi on 2021-04-15 22:38_

> but as a Windows user, I was surprised by this behavior because it uses a meaning of the term "hidden" which is different from the OS's without documenting that it's going to do so

I agree it should be better documented. The reason why ripgrep does this wasn't an accident though. The prefix-dot convention is also used on Windows. It just isn't recognized (AFAIK) by Windows itself.

Your doc update LGTM.

---

_Comment by @dbjorge on 2021-04-15 22:52_

Thanks! I can definitely imagine going either way on an initial decision about how to treat dotfiles on Windows; now that it's a backcompat concern to change it, I think it would be hard to argue for changing the behavior and agree that sticking to just a doc update makes the most sense.

I'll send a PR with the doc update. Do you have a preference on whether to include the pointer in the `--no-ignore-dot` section?



---

_Comment by @BurntSushi on 2021-04-15 22:59_

> I'll send a PR with the doc update. Do you have a preference on whether to include the pointer in the `--no-ignore-dot` section?

Ah yup, that SGTM too! We can hash out the wording if necessary in the PR.

---

_Closed by @BurntSushi on 2021-04-15 23:21_

---

_Label `doc` added by @BurntSushi on 2021-04-15 23:21_

---
