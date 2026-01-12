```yaml
number: 1091
title: "--no-text didn't list as a single option in help"
type: issue
state: closed
author: networm
labels:
  - doc
assignees: []
created_at: 2018-10-25T16:27:59Z
updated_at: 2019-01-26T19:13:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1091
synced_at: 2026-01-12T16:13:22Z
```

# --no-text didn't list as a single option in help

---

_@networm_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

brew install ripgrep

#### What operating system are you using ripgrep on?

macOS Mojave 10.14

#### Describe your question, feature request, or bug.

`--no-text` didn't list as a single option in help

`--no-text` only appeared once in `-a, --text` description, I think it should be listed as well as others.

#### If this is a bug, what are the steps to reproduce the behavior?

rg --help

If possible, please include both your search patterns and the corpus on which
you are searching. Unless the bug is very obvious, then it is unlikely that it
will be fixed if the ripgrep maintainers cannot reproduce it.

If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

#### If this is a bug, what is the actual behavior?

There is no `--no-text` option in help.

#### If this is a bug, what is the expected behavior?

List `--no-text` as well as other options.


---

_Comment by @BurntSushi on 2018-10-25 16:29_

I sympathize, but almost every binary flag has a `--no-` variant. I don't want to flood the man page or the help output with all of them. The compromise we've made is to put them in the relevant flag's long form description.

Perhaps a better compromise would be to include a note about the existence of `--no` flags in the top-level docs, before the listing of flags?

---

_Comment by @networm on 2018-10-25 16:36_

I think it should be ok if there is `--no` flags in top level docs.
But there is so many `--no-*` options in man page by alphabetical order, they should be consistent. Or  there will be someone like me who is confused.

---

_Comment by @BurntSushi on 2018-10-25 16:41_

It's consistent, but perhaps not in the way you expect. Flags listed in the man page are really about changing the default behavior. Sometimes it makes sense to phrase those flags negatively. For example, ripgrep's default behavior is to respect ignore files, but that can be turned off via `--no-ignore`, and even turned back on again with `--ignore`.  But `--ignore` is much less useful than `--no-ignore` since it's already enabled by default, so `--no-ignore` is the one that gets the spotlight.

But yeah, we can add a note about this in the overview. I think at this point I'd be strongly opposed to adding all of the `--no` flags to the docs.

---

_Label `doc` added by @BurntSushi on 2018-10-25 16:41_

---

_Comment by @networm on 2018-10-25 16:46_

Thank you for your explanation. I have learned about it.
I think add a note in the overview is very good. So should I close issue now or wait for the added note?

---

_Comment by @BurntSushi on 2018-10-25 16:49_

Nah we can leave it open until it's fixed. :)

On Thu, Oct 25, 2018, 12:46 狂飙 <notifications@github.com> wrote:

> Thank you for your explanation. I have learned about it.
> I think add a note in the overview is very good. So should I close issue
> now or wait for the added note?
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1091#issuecomment-433124131>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34quMSNhMRkjyd5s25brE0i-dYhBjks5uoerWgaJpZM4X6qCX>
> .
>


---

_Closed by @BurntSushi on 2019-01-26 19:13_

---
