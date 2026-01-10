---
number: 4201
title: Pager for help output
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-help
  - C-tracking-issue
  - S-waiting-on-design
assignees: []
created_at: 2022-09-10T11:20:45Z
updated_at: 2023-02-03T16:41:43Z
url: https://github.com/clap-rs/clap/issues/4201
synced_at: 2026-01-10T01:27:51Z
---

# Pager for help output

---

_Issue opened by @epage on 2022-09-10 11:20_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0

### Describe your use case

As [burntsushi put it](https://github.com/clap-rs/clap/issues/4132#issuecomment-1233080816)

> The underlining is nice, but it is pretty common for me to do things like cmd --help | less, in which case, I imagine the underlining will get disabled by default.

And as [@phillyphil91 pointed out](https://github.com/clap-rs/clap/discussions/4200)

> when having long help messages that extend over a page, the command syntax is hidden above the fold and a user has to scroll up first.

### Describe the solution you'd like

[clig.dev's output guidelines suggest](https://clig.dev/#output)

> Use a pager (e.g. less) if you are outputting a lot of text. For example, git diff does this by default. Using a pager can be error-prone, so be careful with your implementation such that you don’t make the experience worse for the user. You shouldn’t use a pager if stdin or stdout is not an interactive terminal.
> 
> A good sensible set of options to use for less is less -FIRX. This does not page if the content fills one screen, ignores case when you search, enables color and formatting, and leaves the contents on the screen when less quits.
> 
> There might be libraries in your language that are more robust than piping to less. For example, [pypager](https://github.com/prompt-toolkit/pypager) in Python.

### Alternatives, if applicable

_No response_

### Additional Context

This was split out of https://github.com/clap-rs/clap/issues/4132#issuecomment-1242373265

---

_Label `C-enhancement` added by @epage on 2022-09-10 11:20_

---

_Label `A-help` added by @epage on 2022-09-10 11:20_

---

_Label `C-tracking-issue` added by @epage on 2022-09-10 11:20_

---

_Label `S-waiting-on-design` added by @epage on 2022-09-10 11:20_

---

_Comment by @epage on 2022-09-10 11:22_

Unsure if there is something newer/fancier but I was looking at the [pager](https://crates.io/crates/pager) crate.

We should probably detect if a pager is even needed and only show it if it is.

The one downside is `ArgsRequiredElseHelp` outputs help to stderr because this is considered an error case and we assume the help output is the error message and not expected user output.  `pager` only handles it for stdout, so we wouldn't get a pager in this case.  `ArgsRequiredElseHelp` is automatically enabled by derives when a subcommand is required.

---

_Comment by @epage on 2022-09-10 11:40_

`pager` is unix-only and falls back to `more` rather than `less` which isn't great.  `bat` has a built-in pager but I doubt we want to take that on.   We could use [minus](https://crates.io/crates/minus)

---

_Comment by @epage on 2022-09-10 11:42_

We also need to decide if paging should happen on `error.print()` or just `error.exit()`.

We don't know enough about what the application is doing with `error.print()` to do paging but some people do use it to recreate `exit` and there wouldn't be a great way to say "page only if needed" with `error.print()`.

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-09-10 11:43_

---

_Comment by @tshepang on 2022-09-10 12:13_

> `bat` has a built-in pager but I doubt we want to take that on.

what does *take that on* mean


---

_Comment by @epage on 2022-09-10 21:09_

Implementing our own pager directly in clap

---

_Referenced in [build-trust/ockam#3434](../../build-trust/ockam/issues/3434.md) on 2022-09-11 18:18_

---

_Added to milestone `4.x` by @epage on 2022-09-13 12:21_

---

_Comment by @typecasto on 2022-09-24 01:13_

[`streampager`](https://github.com/markbt/streampager) seems relevant here, it seems like it avoids the issues you mention (it can page from an arbitrary file descriptor or anything that `impl Read`, and it can avoid paging unless the output fills the screen). Like minus, it's also usable as a rust library instead of a binary. 

---

_Comment by @epage on 2022-09-24 01:40_

Looks like `streampager` has the downside of `minus`: its the entire pager written in Rust.  I'd be worried what that would do to build times and code size.

Writing a wrapper around external pagers isn't too difficult, I just did it for [another project](https://github.com/epage/git-dive/blob/main/src/git_pager.rs).  Generalizing it into a library will take some finessing though.

---

_Comment by @typecasto on 2022-09-24 02:57_

My personal opinion on this issue as a whole is that it's not that hard to just use my own pager on the command line. If I want paged output, I just add `| sp` to the command line. `fish` even has a shortcut for it, by default (alt+p). Thus, the overhead that comes with adding a pager (or external pager support) is only really justified if it does something more than `some_app --help | $PAGER` would already do. 

My suggestion for that *something* is color/formatting support. Using an external pager usually turns off formatting, since the application doesn't see stdout going to a tty. Handling the pager in clap would allow the nicer output in a paged format. One problem with this is that, while `more` does handle formatted output, `less` does not, it just prints the escape codes by default. Building in a pager like streampager sidesteps this completely, making sure users always see the output formatted nicely, in a consistent way, as well as working cross-platform and not having any "external" dependencies.

My suggestion would be adding a `paged-help` feature to clap that pulls in streampager/minus and uses it to page output. From an application developer perspective it seems pretty ergonomic, if your `--help` isn't likely to use a full page, streampager never gets built and you avoid all that overhead. If in the future if you need one, all you have to do is add the feature flag and then it would "just work", paging the output if it's too tall, and printing it to stderr otherwise. 

One drawback I see to this approach is that a "full page" of output isn't a consistent unit. What might be a full page for the end user ends up being half a screen on the developer's machine, or vice versa, ending up with users needing to scroll the output themselves or having a slightly bigger binary. This also doesn't really account for really short terminals, like the ones in vscode, which need paging a lot more than usual. (though this is pretty much mitigated by these terminals having scrollback). It might be good to have a canonical recommendation for when application devs should turn this feature on in terms of lines of output. (might also be possible to have a lint/warning at build time if you have the feature disabled with >40 lines of help, not sure.)

---

_Comment by @epage on 2022-09-26 12:10_

> My personal opinion on this issue as a whole is that it's not that hard to just use my own pager on the command line. If I want paged output, I just add | sp to the command line. fish even has a shortcut for it, by default (alt+p). Thus, the overhead that comes with adding a pager (or external pager support) is only really justified if it does something more than some_app --help | $PAGER would already do.

For you, maybe.  Not all users use fish and not every user wants to think a priori whether they need to do `| less` or hit `alt+p`.

> One problem with this is that, while more does handle formatted output, less does not, it just prints the escape codes by default. 

`less`s defaults aren't a problem, we can always override them.  In fact, this is what git does.  See https://github.com/epage/git-dive/issues/24 for my research notes on this topic.  The code I linked to in my previous reply intends to re-implement the git logic 100%.  I have used it in practice and it works well, much better than more (had problems with some escape codes).

So I'm still not seeing the benefit of paying the cost of a fully built-in pager, especially one that the developer might not even be using in their application.

---

_Comment by @nyurik on 2023-02-03 16:21_

how much of an increase in size are we talking about?  A built-in pager could be an optional feature. A smaller app (fewer clap flags) is likely to be small in binary size, and it won't be affected because it won't use the feature. The apps with many flags are likely big enough not to notice a tiny size increase due to a built-in pager, so the devs can enable it.

---

_Comment by @epage on 2023-02-03 16:32_

Right now, I'm leaning towards delegating to an external pager like git rather than having a rust-implemented pager.  It keeps the binary size and compile times down and doesn't run into problems where people want to use a different pager and don't want to build two rust-implemented pagers (e.g. one reason ripgrep doesn't enable colors is it uses termcolor and clap v2 uses something else)

To show how small this can be, https://github.com/epage/git-dive/blob/main/src/git_pager.rs is a re-implementation of git's pager logic.

---

_Comment by @nyurik on 2023-02-03 16:41_

Thanks @epage, as long as both paging and colors work at the same time, life is great (i'm still hoping for v4 to restore color support :)).  And as always, the moment I commented above, Twitter showed that [delta](https://github.com/dandavison/delta) (pager) project is trending... not sure if that could be used here, esp if there could ever be some sort of a "stable help syntax" that can be highlighted just like any code file.

---

_Referenced in [clap-rs/clap#4813](../../clap-rs/clap/issues/4813.md) on 2023-03-31 00:37_

---

_Referenced in [sharkdp/bat#2887](../../sharkdp/bat/issues/2887.md) on 2024-03-08 20:49_

---
