```yaml
number: 1540
title: Add support for files tagged as binary in .gitattributes
type: issue
state: open
author: ghost
labels:
  - enhancement
assignees: []
created_at: 2020-04-03T19:07:07Z
updated_at: 2025-01-11T17:35:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1540
synced_at: 2026-01-12T16:13:23Z
```

# Add support for files tagged as binary in .gitattributes

---

_@ghost_

Ripgrep usually ignores binary files quickly and correctly. Sometimes a versioned file may be tagged as binary in `.gitattributes`, because it contains binary data only towards the end of the file, because the file is text but noisy/meaningless (e.g. some postscript files), or because the file is generated and meant to be (typically) ignored. `git` will treat such files as binary for the purposes of displaying in diff/grep/etc, `rg` does not.

Ripgrep should treat such files as binary unless `--text` is passed, the same as `git grep`.

---

_Label `enhancement` added by @BurntSushi on 2020-04-03 19:49_

---

_Comment by @BurntSushi on 2020-04-03 19:50_

I'll mark this as an desirable enhancement for now, but I'm still unsure about this. I'm unsure about trying more of ripgrep's behavior to git. Particularly since there are some fairly accessible work-arounds, such as putting said files into your `.gitignore` or `.ignore`.

---

_Comment by @Uroc327 on 2021-10-19 09:14_

But especially with helpers such as `git-lfs`, one often does not want to ignore binary files in a git repo (anymore).

---

_Comment by @jcubic on 2024-05-12 13:37_

There are no workaround for this. I have minifed files in git repo and there is no way to ignore them. They are marked as binary to don't show but they are not ignored by `ignore`.

---

_Comment by @BurntSushi on 2024-05-12 18:18_

> There are no workaround for this. I have minifed files in git repo and there is no way to ignore them. They are marked as binary to don't show but they are not ignored by `ignore`.

Why can't you add them to a `.rgignore` file?

---

_Comment by @jcubic on 2024-05-12 20:08_

Does it work outside of `ripgrep`. I was looking into Guide was testing `.ignore` and `.rgignore` but it doesn't work with difftastic which uses `ignore` crate.

---

_Comment by @BurntSushi on 2024-05-12 20:16_

Yes it works. The support for .ignore and .rgignore is in the ignore crate.

---

_Comment by @jcubic on 2024-05-12 20:36_

In which version it was added? Because when I installed difftastic it didn't work.

Does it work out of the box, or do you need to configure the `ignore` library to obey the ignore files?

---

_Comment by @BurntSushi on 2024-05-12 20:40_

This isn't difftastic's issue tracker. Support for `.rgignore` has been there since the beginning. Support for `.ignore` was added shortly thereafter.

I cannot tell you why difftastic doesn't work, especially when there are zero details about what is being tried. I've never even heard of difftastic before.

---

_Comment by @jcubic on 2024-05-12 22:05_

I'm just trying to figure out what is needed to use `.ignore` file. This is bug tracker for `riggrep` and `ignore` so I don't see any other place where I can find help for those packages.

The issue of ignoring binary files in difftastic was created a year ago and no one noticed that there is a library that can handle this. So I don't think they can help figure it out.

Difftastic use this code:

```rust
use ignore::WalkBuilder;

    WalkBuilder::new(dir)
        .hidden(false)
        .build()
        .filter_map(Result::ok)
        .map(|entry| Path::new(entry.path()).to_owned())
        .filter(|path| !path.is_dir())
        .map(|path| path.strip_prefix(dir).unwrap().to_path_buf())
        .collect()
```

Will it work when I add:

```rust
ignore(true)
```

to the chain?

It would be much better if there was `binary(true)` method that would ignore binary files from `.gitattributes`, so users don't need to create extra files just to ignore minifed files.

---

_Comment by @BurntSushi on 2024-05-12 23:23_

> It would be much better if there was `binary(true)` method that would ignore binary files from `.gitattributes`, so users don't need to create extra files just to ignore minifed files.

Hence why this is an open issue labeled as an enhancement. What you said is that there is no work around and no way to ignore files. But this is false.

If `ignorr(true)` isn't working in a minimal Rust program then open a new bug issue with full reproduction details, as that is unrelated to this feature request. It should be a reasonably minimal reproduction, not "difftastic doesn't work."

Please keep this issue related to gitattributes support. This issue isn't about general ignore rules or debugging specific scenarios where it isn't working like you expect.

---

_Comment by @robinmoussu on 2024-12-05 09:59_

@jcubic Did you made any progress on this?

---

_Comment by @jcubic on 2024-12-05 12:15_

@robinmoussu no, maybe I should revisit this.

**EDIT**: just found that I actually tested the `.ignore(true)`, and it doesn't work with difftastic. But I didn't have time to create reproduction. I'm not even sure if I will know how, since I don't know Rust.

---

_Comment by @frankitox on 2025-01-02 18:24_

I just stumbled into this.

> I'm unsure about trying more of ripgrep's behavior to git.

Maybe adding a flag like  `--follow-gitattributes` (default off) may palliate this problem? Using `.rpignore` for now, which works wonders!

---

_Comment by @frankitox on 2025-01-11 17:35_

Hi Andrew (@BurntSushi)!

Would you be interested in a PR to address this? Based on the [.gitattributes docs](https://git-scm.com/docs/gitattributes) each line goes like this:

```
pattern attr1 attr2 ...
```

Turns out that the `binary` keyword is a macro that expands to `-diff -merge -text`, with `-text` being the relevant `attr`. To parse these attribute files I was thinking of looking for `-text` or `binary` in the attrs, trying to reuse as much of the code that already reads gitignore files.

---
