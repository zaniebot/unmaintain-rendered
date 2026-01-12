```yaml
number: 1106
title: "--files-without-match reports an error when used with a single file path"
type: issue
state: closed
author: ShadowIce
labels:
  - bug
assignees: []
created_at: 2018-11-14T17:06:00Z
updated_at: 2019-01-23T02:38:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1106
synced_at: 2026-01-12T16:13:22Z
```

# --files-without-match reports an error when used with a single file path

---

_@ShadowIce_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

I'm trying to get a list of files that contain one word, but not another word. To do that I call "rg -l foo" to get a list of files that I pass on to another call with "rg --files-without-match bar". The call looks something like this: rg -l foo | xargs -i -d '\n' rg --files-without-match bar {}

This doesn't work because --files-without-match cannot be used with a single file. The error message is " output kind PathWithoutMatch requires a file path". However I don't understand why that shouldn't be possible.
Maybe there's a better way to do this? If not I'd really appreciate it if that could be implemented.

---

_Comment by @BurntSushi on 2018-11-14 17:12_

Please provide a fully reproducible example, which should include a corpus
to search.

On Wed, Nov 14, 2018, 12:06 Maurice Gilden <notifications@github.com wrote:

> What version of ripgrep are you using?
>
> ripgrep 0.10.0
> -SIMD -AVX (compiled)
> How did you install ripgrep?
>
> cargo install ripgrep
> What operating system are you using ripgrep on?
>
> Windows 10
> Describe your question, feature request, or bug.
>
> I'm trying to get a list of files that contain one word, but not another
> word. To do that I call "rg -l foo" to get a list of files that I pass on
> to another call with "rg --files-without-match bar". The call looks
> something like this: rg -l foo | xargs -i -d '\n' rg --files-without-match
> bar {}
>
> This doesn't work because --files-without-match cannot be used with a
> single file. The error message is " output kind PathWithoutMatch requires a
> file path". However I don't understand why that shouldn't be possible.
> Maybe there's a better way to do this? If not I'd really appreciate it if
> that could be implemented.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1106>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34tQF8B0lL928CvtUt5Dd-mdoTz3Nks5uvE16gaJpZM4YeL2C>
> .
>


---

_Comment by @ShadowIce on 2018-11-14 17:41_

If you need an example, use the ripgrep source code with this (it lists all files with an impl block, but no tests):
rg -l -g '*.rs' "impl " | xargs -i -d '\n' grep -L 'cfg(test)' {}

What I would like is to use rg for the second part as well, which probably would be: rg -l -g '*.rs' "impl " | xargs -i -d '\n' rg --files-without-match 'cfg\\(test\\)' {}

My use case is searching in C++ code for includes that are not needed, e.g. rg -l "#include \<vector\>" | xargs -i -d '\n' grep -L "vector\<" {}

---

_Comment by @BurntSushi on 2018-11-14 17:55_

Thanks. As a suggestion, your `xargs` use is a bit indiosyncratic. `rg -l -g '*.rs' "impl " -0 | xargs -0 grep -L 'cfg(test)'` is simpler.

Also, `xargs` is not needed for a reproduction. Here's a very simple example:

```
$ rg --files-without-match 'cfg(test)' src/main.rs
src/main.rs: output kind PathWithoutMatch requires a file path

$ rg --files-without-match 'cfg(test)' src/main.rs src/main.rs
src/main.rs
src/main.rs
```

This is definitely a bug. Thanks for reporting it!

---

_Label `bug` added by @BurntSushi on 2018-11-14 17:55_

---

_Renamed from "Feature request: Parameters --files-without-match / --files-with-matches should work with single file" to "--files-without-match reports an error when used with a single file path" by @BurntSushi on 2018-11-14 17:55_

---

_Closed by @BurntSushi on 2019-01-23 02:38_

---
