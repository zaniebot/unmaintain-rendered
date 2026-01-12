```yaml
number: 188
title: support fancier regex features such as look-around and backreferences
type: issue
state: closed
author: Mange
labels: []
assignees: []
created_at: 2016-10-19T08:30:59Z
updated_at: 2025-10-12T18:09:48Z
url: https://github.com/BurntSushi/ripgrep/issues/188
synced_at: 2026-01-12T16:13:21Z
```

# support fancier regex features such as look-around and backreferences

---

_@Mange_

One area that makes `ag` unable to be replaced by `ripgrep` is that `ag` supports negative lookahead. For example:

```
ag 'foo(?!.*bar)'
```

This would find all rows with "foo", but not with "bar" somewhere after that on the same line.

`ripgrep` gives the following error message:

```
› rg 'foo(?!.*bar)'
Error parsing regex near 'foo(?!.*ba' at character offset 5: Unrecognized flag: '!'. (Allowed flags: i, m, s, U, u, x.)
```

I guess this is actually a problem in the `regex` crate, and I can open the same issue there if you wish.


---

_Comment by @BurntSushi on 2016-10-19 12:26_

> I guess this is actually a problem in the regex crate, and I can open the same issue there if you wish.

Indeed it is, but it's not a "problem" per se, it's a conscious and explicit trade-off that I made. Notably, [this is the very first paragraph of the `regex` docs](https://doc.rust-lang.org/regex/regex/index.html):

> This crate provides a native implementation of regular expressions that is heavily based on RE2 both in syntax and in implementation. Notably, backreferences and arbitrary lookahead/lookbehind assertions are not provided. In return, regular expression searching provided by this package has excellent worst-case performance. The specific syntax supported is documented further down.

If you'd like to read more about this, please consult Russ Cox's excellent series on the matter (I'd also be happy to answer specific questions): https://swtch.com/~rsc/regexp/

> One area that makes ag unable to be replaced by ripgrep is that ag supports negative lookahead.

Note that this isn't an unfettered win, it's a _trade off_. For example:

```
$ cat foo
c
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
c
$ ag '(a*)*c' foo
1:c
$ rg '(a*)*c' foo
1:c
3:c
```

The Silver Searcher completely drops the second match because it's using a backtracking regex engine, which is required* for supporting arbitrary lookaround.

> This would find all rows with "foo", but not with "bar" somewhere after that on the same line.

You can satisfy this particular use case relatively easily with `rg foo | rg -v 'foo.*bar'`. However, I acknowledge that this is inconvenient for a number of reasons. I also acknowledge that there are other use cases of lookaround that aren't so easily worked around.

\* - Strictly speaking, there is [some theory](http://laurikari.net/ville/spire2000-tnfa.ps) that supports it without backtracking, but I'm not aware of a practically fast implementation of it. (There is TRE and regex-tdfa, of which I have used neither, but I've never seen any compelling evidence that either was fast. Come to think of it, I don't actually know if regex-tdfa supports arbitrary lookaround.)


---

_Closed by @BurntSushi on 2016-10-19 12:26_

---

_Comment by @Mange on 2016-10-24 13:40_

Thank you for the detailed response. I'll keep `ag` installed for these cases. :-)

This specific case would be pretty hard to do using pipes since I used it in my text editor.


---

_Comment by @BurntSushi on 2016-10-25 02:27_

Note that this is mentioned in the [anti-pitch](http://blog.burntsushi.net/ripgrep/#anti-pitch), which I should add to the README.


---

_Comment by @rsmolkin on 2018-04-17 19:11_

Hi, I just heard that your project was included into the latest version of VSCode for text searching.  Just recently I was trying to do a RegEx with a negative lookbehind across our entire code base.  I could not get it to work in VSCode, but it worked for me in Sublime Text.  I'd prefer to stay in VSCode, and at first was very excited to hear that they added a better text search.  But then saw this issue where negative lookahead was discussed.  I'm not sure if my specific case could also be re-written with pipes.  I was trying to find an instance of "IN" or 'IN' with double or single quotes, but not as in type="in" cause that was the most common occurrence and not related.  I also wanted to find a way where it was followed by Indiana or India shortly after, but didn't find a regex way to do that.  Can you maybe give some more info on how to use the pipes with some examples, or maybe add the lookahead and lookbehind options?

---

_Comment by @BurntSushi on 2018-04-17 21:33_

> Can you maybe give some more info on how to use the pipes with some examples

`rg -i "['\"]in['\"]" | rg -v "type="`

> or maybe add the lookahead and lookbehind options?

No.

---

_Comment by @rsmolkin on 2018-04-18 13:56_

Thanks, I guess that won't work in VSCode as is just on commandline.  

---

_Renamed from "Negative lookahead" to "support fancier regex features such as look-around and backreferences" by @BurntSushi on 2018-08-19 13:58_

---

_Reopened by @BurntSushi on 2018-08-19 13:58_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Comment by @timotheecour on 2018-08-23 07:34_

@BurntSushi 
how do I enable PCRE2 ? 
`rg --pcre2` gives: `PCRE2 is not available in this build of ripgrep`

* NOTE: I've installed `rg` via `brew install --HEAD ripgrep` to get the HEAD version of rg.
* the ripgrep formula is here: https://github.com/Homebrew/homebrew-core/blob/master/Formula/ripgrep.rb
* the relevant line is probably here: `system "cargo", "install", "--root", prefix`
How do I change this to support `--pcre2` ?

* i read this but not sure how to proceed: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#how-do-i-use-lookaround-andor-backreferences
> If you installed ripgrep through a different means (like your system's package manager), then please reach out to the maintainer of that package to see whether it's possible to enable the PCRE2 feature

obviously the package maintainer of homebrew will have the same question as me...


---

_Comment by @BurntSushi on 2018-08-23 09:49_

The build instructions are in the README.

On Thu, Aug 23, 2018, 03:34 Timothee Cour <notifications@github.com> wrote:

> @BurntSushi <https://github.com/BurntSushi>
> how do I enable PCRE2 ?
> rg --pcre2 gives: PCRE2 is not available in this build of ripgrep
>
>    -
>
>    NOTE: I've installed rg via brew install --HEAD ripgrep to get the
>    HEAD version of rg.
>    -
>
>    the ripgrep formula is here:
>    https://github.com/Homebrew/homebrew-core/blob/master/Formula/ripgrep.rb
>    -
>
>    the relevant line is probably here: system "cargo", "install",
>    "--root", prefix
>    How do I change this to support --pcre2 ?
>    -
>
>    i read this but not sure how to proceed:
>    https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#how-do-i-use-lookaround-andor-backreferences
>
> If you installed ripgrep through a different means (like your system's
> package manager), then please reach out to the maintainer of that package
> to see whether it's possible to enable the PCRE2 feature
>
> obviously the package maintainer of homebrew will have the same question
> as me...
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/188#issuecomment-415320792>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34i2UI3pwvgaw5VjAdpUUveFPkZ7Uks5uTlshgaJpZM4KashK>
> .
>


---

_Comment by @BurntSushi on 2018-08-23 11:54_

For Homebrew, technically, the simplest possible change is to replace this

```
system "cargo", "install", "--root", prefix
```

with this

```
system "cargo", "install", "--root", prefix, "--features", "pcre2"
```

Which is [consistent with the build instructions in the README](https://github.com/BurntSushi/ripgrep#building). Note that you cannot do this with the current release, since only master has the `pcre2` feature.

Note that if you do this, ripgrep will, by default, attempt to link with the system version of PCRE2. So you either need to add PCRE2 itself as an optional dependency of the `ripgrep` formula, or you need to force static compilation of PCRE2. Either will work. The build instructions also tell you how to do that, which is done by setting the environment variable `PCRE2_SYS_STATIC=1`.

---

_Comment by @timotheecour on 2018-08-23 13:32_

thanks, `system "cargo", "install", "--root", prefix, "--features", "pcre2"` did work. 

>  Note that you cannot do this with the current release, since only master has the pcre2 feature

works with `brew install --HEAD ripgrep`

NOTE: This option mentioned in readme (`--features 'pcre2 simd-accel avx-accel'`) did not work


---

_Comment by @BurntSushi on 2018-08-23 13:33_

> NOTE: This option mentioned in readme (--features 'pcre2 simd-accel avx-accel') did not work

That doesn't mean anything to me. How am I supposed to diagnose "did not work" and help you fix it?

---

_Comment by @timotheecour on 2018-08-23 13:40_

using https://github.com/timotheecour/homebrew-timutil/blob/master/ripgrep_temp.rb

brew tap timotheecour/timutil
brew reinstall --HEAD timotheecour/timutil/ripgrep_temp

```
==> Reinstalling timotheecour/timutil/ripgrep_temp
==> Cloning https://github.com/BurntSushi/ripgrep.git
Cloning into '/Users/timothee/Library/Caches/Homebrew/ripgrep_temp--git'...
==> Checking out branch master
Already on 'master'
Your branch is up to date with 'origin/master'.
==> cargo install --features pcre2 avx-accel --root /Users/timothee/homebrew/Cellar/ripgrep_temp/HEAD-f1e0258
Last 15 lines from /Users/timothee/Library/Logs/Homebrew/ripgrep_temp/01.cargo:
error[E0554]: #![feature] may not be used on the stable release channel
 --> .brew_home/.cargo/registry/src/github.com-1ecc6299db9ec823/simd-0.2.2/src/lib.rs:4:1
  |
4 | #![feature(cfg_target_feature, repr_simd, platform_intrinsics, const_fn)]
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: aborting due to previous error

For more information about this error, try `rustc --explain E0554`.
error: Could not compile `simd`.
warning: build failed, waiting for other jobs to finish...
error: failed to compile `ripgrep v0.9.0 (file:///private/tmp/ripgrep_temp-20180823-73604-1pwlm5r)`, intermediate artifacts can be found at `/private/tmp/ripgrep_temp-20180823-73604-1pwlm5r/target`

Caused by:
  build failed

If reporting this issue please do so at (not Homebrew/brew or Homebrew/core):
```


---

_Comment by @BurntSushi on 2018-08-23 13:57_

@timotheecour Ah yes, thank you. The README says:

> If you have a Rust nightly compiler and a recent Intel CPU, then you can enable additional optional SIMD acceleration like so:

I've updated the text to make it a little more clear: https://github.com/BurntSushi/ripgrep/commit/1bb8b7170f4e35da87cd73efdf1e4d3a5c3a416c

---
