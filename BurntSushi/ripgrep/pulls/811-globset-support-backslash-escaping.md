```yaml
number: 811
title: "globset: support backslash escaping"
type: pull_request
state: closed
author: bmalehorn
labels: []
assignees: []
base: master
head: glob-backslash
created_at: 2018-02-17T04:36:56Z
updated_at: 2018-03-10T14:31:10Z
url: https://github.com/BurntSushi/ripgrep/pull/811
synced_at: 2026-01-12T18:23:13Z
```

# globset: support backslash escaping

---

_@bmalehorn_

From `man 7 glob`:

    One can remove the special meaning of '?', '*' and '[' by preceding
    them by a backslash, or, in case this is part of a shell command
    line, enclosing them in quotes.

Conform to glob / fnmatch / git implementations by parsing `\?`, `\*`
and `\[` as literal `?`, `*` and `[`. If a backslash is followed by
anything else, parse it as before.

Closes #526

---

_Comment by @BurntSushi on 2018-02-17 04:45_

@bmalehorn Thanks for putting the work into this! #526 raised a number of issues with this, particularly with respect to Windows. Could you explain how those issues are addressed in this PR? I also see a test that is only run on Unix, but no comment explaining why. Could you elaborate on that as well?

---

_Comment by @okdana on 2018-02-17 05:10_

Aside from the Windows path issue, if i'm reading it right, this doesn't actually conform to the real-world behaviour of either `fnmatch(3)` or Git: A `\` should escape *any* character that follows it, even if it's not a meta-character. In other words, not only does the pattern `foo\*bar` match the string `foo*bar`, `foo\bar` also matches `foobar`.

(For comparison, [here](https://github.com/BurntSushi/ripgrep/issues/526#issuecomment-310754910) is where i provided my own attempt at fixing it, and as mentioned i think it behaves pretty well on UNIX, but it leaves the Windows issues unaddressed just like this one does.)

---

_Comment by @bmalehorn on 2018-02-17 05:50_

@okdana That's not quite correct, `foo\*bar` matches `foo*bar` but `foo\bar` will still only match `foo\bar`. If the character after `\` isn't one of the special characters, we emit both characters.

However I'm realizing now what you two probably realized a while ago, that this is going to break `-g` on Windows. Even though `src\foo.rs` will still work on Windows with this commit, globs like `src\*.rs` won't. Instead, you'd have to type `src/*.rs` or `src[\]*.rs`.

I suppose the real problem stems from using the same glob code for both `-g` and gitignores. `rg -g` is considerate to Windows and allows backslashes. Meanwhile Git demands .gitignores be written only with forward slashes, freeing up backslashes for escaping. Ditto for Mercurial and .hgignores.

So I suppose the only ways to parse .gitignores 100% accurately are:

1. (this PR) make `\` unusable in some situations; annoy Windows users, OR
2. only apply this glob change for .gitignores; parse `-g` and .gitignores differently; be inconsistent

Both of these kind of suck so I can see why this issue hasn't been closed yet.

Does that explanation make sense to everyone? Perhaps it's best to leave everything as it is: don't annoy Windows users, don't be inconsistent, but do parse a rare .gitignore pattern incorrectly.


---

_Comment by @okdana on 2018-02-17 06:03_

>@okdana That's not quite correct, `foo\*bar` matches `foo*bar` but `foo\bar` will still only match `foo\bar`

How did you come to that conclusion?

```
% cat fnmatch.c
#include <fnmatch.h>
#include <stdio.h>
int main(int argc, char *argv[]) {
  printf("fnmatch() returned %d\n", fnmatch(argv[1], argv[2], 0));
}

% gcc -o fnmatch fnmatch.c

% ./fnmatch 'foo\bar' 'foo\bar'
fnmatch() returned 1
% ./fnmatch 'foo\bar' 'foobar'
fnmatch() returned 0
```

Or with Git:

```
% mkdir rg811 && cd rg811 && git init
Initialized empty Git repository in /private/tmp/rg811/.git/
% touch .gitignore foobar 'foo\bar'

% git status --porcelain
?? .gitignore
?? "foo\\bar"
?? foobar

% echo 'foo\bar' > .gitignore; git status --porcelain
?? .gitignore
?? "foo\\bar"

% echo 'foo\\bar' > .gitignore; git status --porcelain
?? .gitignore
?? foobar
```

I personally would like `\` to work as expected on UNIX, and since the `gitignore` spec doesn't support `\` as a path separator on Windows i feel like it's kind of problematic that `globset` does. If i were a Windows user i think i would just expect `rg` to behave like Git. But i can imagine that making a unilateral change like that could break stuff, so idk

---

_Comment by @bmalehorn on 2018-02-17 08:42_

@okdana Sorry if I wasn't clear, but I was describing the behavior of my patch, not `fnmatch` or `git`. But I see now how my patch differs from `fnmatch`.

> I personally would like \ to work as expected on UNIX, and since the gitignore spec doesn't support \ as a path separator on Windows i feel like it's kind of problematic that globset does. If i were a Windows user i think i would just expect rg to behave like Git. But i can imagine that making a unilateral change like that could break stuff, so idk

I propose a compromise: on Windows, globset would behave as-is, but on Unix, we would correct it to behave like fnmatch (like in your patch). Windows users could still use `\` in pathnames, and Unix users would get exactly fnmatch behavior. Does that sound like a good idea to anyone?

---

_Comment by @okdana on 2018-02-17 17:40_

>Sorry if I wasn't clear, but I was describing the behavior of my patch

Oh, sorry.

>Does that sound like a good idea to anyone?

That's how PHP does it. Not sure if it's the best option or not, but it's an option...

---

_Comment by @BurntSushi on 2018-02-20 12:27_

> I propose a compromise: on Windows, globset would behave as-is, but on Unix, we would correct it to behave like fnmatch (like in your patch). Windows users could still use `\` in pathnames, and Unix users would get exactly fnmatch behavior. Does that sound like a good idea to anyone?

This sounds plausible, but we need to be fastidious in how we go about this. That is, I suspect this should be a [configuration knob on the `Glob` type](https://docs.rs/globset/0.3.0/globset/struct.GlobBuilder.html) so that users of the library can choose what behavior they want explicitly, if necessary. I'd probably call it:

```rust
fn forward_slash_separator(&mut sefl, yes: bool) -> &mut GlobBuilder<'a> {
```

We should document this to say that when a forward slash is a separator, then it will never be interpreted as the start of an escape sequence. But when disabled, `\` may be used to escape meta characters. We should document this in the syntax description in the main docs of the `globset` crate.

I'm tempted to say that this should be disabled by default on all non-Windows platforms and enabled by default on Windows platforms, but that kind of conditional logic in a somewhat fundamental crate like this seems... unwise. Alas, we already have some conditional logic in that we always treat `\` as a separator on Windows: https://github.com/BurntSushi/ripgrep/blob/597bf04a56d43aa9c0eb0f8fbb90c9d51c53656c/globset/src/glob.rs#L730

And looking at that more, explicitly calling out "forward slash" in the API seems wrong too, since we've broken the "path separator" abstraction by doing so.

What about this instead?

```rust
/// When enabled, a forward slash (`\`) may be used to escape
/// special characters in a glob pattern. Additionally, this will
/// prevent `\` from being interpreted as a path separator on all
/// platforms.
///
/// This is enabled by default on platforms where `\` is not a
/// path separator and disabled by default on platforms where `\`
/// is a path separator.
fn forward_slash_escape(&mut sefl, yes: bool) -> &mut GlobBuilder<'a> {
```

Regrettably, there is no way around a breaking change here, which means this will require another semver bump. /sigh

---

_Comment by @BurntSushi on 2018-02-20 12:30_

@retep998 Do you have any opinions on how `\` is handled in glob patterns on Windows?

---

_Comment by @okdana on 2018-02-20 12:47_

Nit-pick: `\` is the back-slash, not forward-slash

An option like that sounds reasonable to me for whatever it's worth. The only concerns i can think of with the platform-specific method are (1) it might trick Windows users into thinking that Git supports `\` as a separator (which it apparently doesn't), and (2) it still leaves them out in the cold when it comes to being able to escape things.

I never use Windows, so i have zero personal stake in how well `rg` works there, but if i *were* a Windows user i think i'd find that limitation somewhat bothersome. On the other hand, like i mentioned in the other thread, most of the glob meta-characters are illegal in Windows file names anyway (or they were last time i used it), so maybe it doesn't actually matter that much in practice...

---

_Comment by @BurntSushi on 2018-02-20 12:51_

> Nit-pick: \ is the back-slash, not forward-slash

:rofl: Thanks for that. Derp.

> I never use Windows, so i have zero personal stake in how well rg works there, but if i were a Windows user i think i'd find that limitation somewhat bothersome. On the other hand, like i mentioned in the other thread, most of the glob meta-characters are illegal in Windows file names anyway (or they were last time i used it), so maybe it doesn't actually matter that much in practice...

Yeah, I share your trepidation, definitely. I don't use Windows either, so I basically have no idea what the common or expected usage patterns are. Bascially, I hear your argument, but I also think, "Shouldn't Windows users be able to use their native path separator? Maybe they are just annoyed that git doesn't allow it?" But yeah, hopefully @retep998 can shed some light. @roblourens might also have opinions!

---

_Comment by @retep998 on 2018-02-20 18:19_

`.gitignore` is a cross platform file where the paths are expected to work everywhere, so requiring `/` in that is reasonable. However on the command line `git` is perfectly happy to work with my normal Windows paths that have `\` in them, and if you give `git` a glob path like `src\*.rs` on the command line, it will interpret `\` as a path separator and `*` is not escaped.

Any changes to make `\` an escape character on Windows would be exceptionally annoying for Windows users.

---

_Comment by @BurntSushi on 2018-02-20 18:30_

> Any changes to make `\` an escape character on Windows would be exceptionally annoying for Windows users.

I understand this for the CLI, definitely. But is it also true for `.gitignore` files as well? That is, the whole point of this PR is that `\` is used as an escape in globs in `.gitignore` files. Should we allow that on Windows too for `.gitignore`?

But that is a good point about the CLI. We may want to toggle this behavior differently depending on whether the globs are in .gitignore or the CLI.

---

_Comment by @retep998 on 2018-02-20 18:51_

On Windows, in `.gitignore`, `\` is indeed used as an escape character and not a path separator.

---

_Comment by @BurntSushi on 2018-02-20 18:56_

All right, I guess that's good enough for me.

@bmalehorn I think my comment here still seems mostly relevant: https://github.com/BurntSushi/ripgrep/pull/811#issuecomment-366962156 That is, we still need to make `\` as an escape character be a toggleable feature. I think enabling it by default on Unix and disabling it by default on Windows makes the most sense for `globset`. Then in the `ignore` crate, we can enable it unconditionally regardless of platform.

---

_Comment by @bmalehorn on 2018-02-22 07:20_

This all makes sense and sounds like the most reasonable solution. I'll update this PR tomorrow.

---

_Comment by @bmalehorn on 2018-03-04 19:37_

I probably should have commented when I updated the PR. @BurntSushi thoughts on the updated PR?

---

_Closed by @BurntSushi on 2018-03-10 14:31_

---
