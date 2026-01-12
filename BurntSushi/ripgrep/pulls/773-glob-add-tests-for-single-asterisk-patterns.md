```yaml
number: 773
title: "[glob] Add tests for single-asterisk patterns"
type: pull_request
state: closed
author: behnam
labels: []
assignees: []
base: master
head: glob-asterisk
created_at: 2018-02-05T02:29:45Z
updated_at: 2018-02-07T03:27:26Z
url: https://github.com/BurntSushi/ripgrep/pull/773
synced_at: 2026-01-12T18:23:13Z
```

# [glob] Add tests for single-asterisk patterns

---

_@behnam_

@Diggsey reported in <https://github.com/rust-lang/cargo/issues/4268#issuecomment-362919612> that the single-asterisk patterns are over-matching into deeper directories.

Looking at the resources I found, looks like that's a right claim, so here I've added the unit tests which show the problem. (The diff breaks the build as it is and should not be landed before a fix is in place.)

I haven't looked into a fix yet, since I'm not 100% sure about the right behavior. If so, would be great to fix it. (Unfortunately, I cannot promise I'll find the time to work on it at the moment.)

What do you think, @BurntSushi?

---

_Closed by @BurntSushi on 2018-02-06 22:44_

---

_Comment by @BurntSushi on 2018-02-06 22:46_

@behnam Thanks for reporting this! This would be *quite* a bad bug. Thankfully, this is expected behavior. By default, `*` matches a `/`. To disable that behavior, you need to enable the [`GlobBuilder::literal_separator`](https://docs.rs/globset/0.2.1/globset/struct.GlobBuilder.html#method.literal_separator) method. This is [documented in the syntax section of the `globset` docs](https://docs.rs/globset/0.2.1/globset/#syntax).

---

_Comment by @BurntSushi on 2018-02-06 22:46_

@behnam I fixed your commit by setting the appropriate option and merged it: https://github.com/BurntSushi/ripgrep/commit/706323ad8f2600db7d46482a7d95972b1416c0eb

---

_Comment by @behnam on 2018-02-07 02:58_

Thanks, @BurntSushi, for the info and committing the tests!

I didn't realize that `literal_separator` is not default behavior. So, now wondering which ones's the most common behavior in the glob world?

The reason I'm asking is for which one Cargo should use, if we use globs for in inclusion/exclusion logic.

AFAIU, gitignore-style (`ignore`) doesn't have this issue and always behaves like `literal_separator`. Right?

---

_Comment by @BurntSushi on 2018-02-07 03:15_

Unfortunately, things are more subtle. Gitignore only enables this option when there is no literal `/` in the glob. That is, `*.rs` can match through a `/` but `a/*/b` cannot.

globset has the same default behavior as glob.

If you want gitignore style for Cargo, then it should be easy enough to just use the ignore crate. I don't know what the right choice is for Cargo though.

---

_Branch deleted on 2018-02-07 03:22_

---

_Comment by @behnam on 2018-02-07 03:27_

Thanks for the details, @BurntSushi! Got it.

In Cargo, we're in the process to replace the glob-like matching with a gitignore-like one. We think it's easier to use because of similarity with git configurations, and provides more control, when needed.

---
