```yaml
number: 953
title: "`Walk` from `ignore` returns an Ok(...) and an Err(...) for the same permission-denied subdirectory"
type: issue
state: closed
author: sashaweiss
labels:
  - doc
assignees: []
created_at: 2018-06-16T15:38:32Z
updated_at: 2018-08-30T19:33:17Z
url: https://github.com/BurntSushi/ripgrep/issues/953
synced_at: 2026-01-12T16:13:22Z
```

# `Walk` from `ignore` returns an Ok(...) and an Err(...) for the same permission-denied subdirectory

---

_@sashaweiss_

Skipped some of the preamble, since this isn't strictly a `ripgrep` question!

I'm using `ignore 0.4.2`, on Mac OSX 10.13.4.

#### Describe your question, feature request, or bug.
When I use a `Walk` to iterate a directory containing a subdirectory with restricted permissions, it yields two separate entries for that subdirectory - one with an `Ok(DirEntry {...})` and one with an `Err(WithPath {...})`.

Also interestingly, calling the `DirEntry::error()` method from the `DirEntry` in the `Ok` returns `None`.

#### If this is a bug, what are the steps to reproduce the behavior?
[This example repo](https://github.com/sashaweiss/ignore_permissions_example) will demonstrate this behavior!

#### If this is a bug, what is the expected behavior?
I think I expected that calling `DirEntry::error()` on the `DirEntry {...}` in the `Ok` would give me back the same `WithPath` error, or some other indicator that `ignore` won't be able to recur into that directory.

Alternatively, if it's possible for a `Walk` to build a `DirEntry` even for a directory that is permission-restricted, maybe it makes sense for there to be another `Error` variant that contains a `DirEntry`? A `WithDirEntry {...}` perhaps?

I think in either case it'd make sense to eliminate the duplicate entry - in the first case by not returning the `Err`, and in the second case by not returning the `Ok`. Off the bat, not sure which would make the most sense. It's also not immediately clear to me how to make sure a solution to this makes sense for permission-restricted _files_, since so far my discussion of it has been focused on directories (and the fact that `ignore` won't recur into them).

My intuition says that the error comes about since `ignore` can detect the restricted directory while searching its parent directory, and that the error comes about when it tries to dive in - since this doesn't come up for restricted files. If you agree that this is something that should change, I'd be happy to try to help with a PR if you'd like!

Thanks in advance for your advice! `ignore` has been awesome to use and exactly what I needed - just want to confirm this behavior with you before building a workaround.

---

_Comment by @BurntSushi on 2018-06-16 16:11_

TL;DR - This looks like expected behavior to me. Thanks for the good bug report though!

> When I use a Walk to iterate a directory containing a subdirectory with restricted permissions, it yields two separate entries for that subdirectory - one with an `Ok(DirEntry {...})` and one with an `Err(WithPath {...})`.

It's a bit more subtle than that. When you list the contents of `sandbox`, part of that listing includes `restricted_dir`, and there is no problem _observing_ the existence of that directory. The subsequent error that occurs comes from attempting to _descend_ into that directory, which is prevented from happening because of the permissions on that entry. In other words, this seems like correct behavior to me. That is, you aren't seeing duplicate entries.

> I think I expected that calling `DirEntry::error()` on the `DirEntry {...}` in the `Ok` would give me back the same `WithPath` error, or some other indicator that ignore won't be able to recur into that directory.

The `DirEntry::error()` method exists to return errors unrelated to traversing the directory itself, but rather, related to any ancillary behavior required to support filtering. For example, in order to respect `.gitignore` rules, those files must be opened and parsed. Errors can occur while reading data from that file or while parsing data from that file. These errors don't impact traversal specifically, and shouldn't prevent traversal from continuing. These errors also shouldn't be suppressed completely since the command line application may want to surface them depending on the use case.

The documentation on the `error` method isn't great. It gives an example of an error, but is still somewhat cryptic. I think improving the documentation on that method would be a great idea.

---

_Label `doc` added by @BurntSushi on 2018-06-16 16:11_

---

_Comment by @sashaweiss on 2018-06-18 04:33_

Gotcha - thanks so much for the detailed response! Glad to hear this is expected. In that case, I'll build a workaround. Shouldn't be a big deal!

Just for my own clarity - were you asking for help improving the docs on the `error` method? I'd be skeptical that my suggestions would be useful or accurate, especially given my earlier misinterpretation, but would be happy to try if that's what you meant.

Thanks again!

---

_Comment by @BurntSushi on 2018-06-18 10:38_

> Just for my own clarity - were you asking for help improving the docs on the error method? I'd be skeptical that my suggestions would be useful or accurate, especially given my earlier misinterpretation, but would be happy to try if that's what you meant.

Actually, suggestions from you would be far more useful than what I could come up with. You're the one using the crate, and you understand your use far better than I do. :-)

You don't have to submit a PR or anything, but if you did want to help, just leaving some suggestions for things to include in the doc string would be much appreciated! Submitting a PR would be fine, and of course, doing nothing is fine too. :-) I will come up with something.

---

_Comment by @sashaweiss on 2018-06-18 13:17_

Ok - sounds good! I'm happy to come up with some ideas. I'll keep you posted!

---

_Comment by @sashaweiss on 2018-06-18 15:54_

Having read a little deeper, it seems like the only time `error` would be non-`None` is in the case that this entry is a poorly-formatted `.[git]ignore` or `exclude`. (I initially thought it'd also error if the current entry is a directory containing a poorly formatted one of the above, but I no longer think that's true?)

If that is the case, it's not clear to me that there's a lot to be added to the docstring for `error`. However, it might reduce ambiguity to be more concrete about the kind of errors that could be returned?

I think my confusion came from the ambiguity inherent in "An example of an error is...". If the only errors are ignore parsing errors, maybe the docstring could be more explicit about that? Unless I misread, of course. In that case, if the set of errors is still relatively discrete maybe the docstring could enumerate them, or say something more along the lines of "This method is intended for detecting that parsing an `ignore` file errored. While other errors such as `<other error case>` may occur, these should be relatively rare."

Re: the fact that `Walk` will return two entries for a permission-restricted subdirectory, I was largely able to come to the same conclusion you replied with while I was writing the bug report (i.e. that this could be expected behavior) - hopefully anyone who hits that in the future will be able to do the same, or can use this thread as a reference! So I think that that part of my issue has been documented here already.

Thanks again for your help!

---

_Comment by @BurntSushi on 2018-08-21 23:59_

@sashaweiss The issue here is that I don't want to say, "this will _only ever_ produce `ignore` related errors," because that ties my hands in terms of the public API contract. However, I will updated it to say this, which I hope is a little clearer:

```rust
    /// Returns an error, if one exists, associated with processing this entry.
    ///
    /// An example of an error is one that occurred while parsing an ignore
    /// file. Errors related to traversing a directory tree itself are reported
    /// as part of yielding the directory entry, and not with this method.
```

---

_Closed by @BurntSushi on 2018-08-22 03:05_

---

_Comment by @sashaweiss on 2018-08-30 19:33_

Awesome - thanks @BurntSushi!

---
