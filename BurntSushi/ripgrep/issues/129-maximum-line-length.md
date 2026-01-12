```yaml
number: 129
title: maximum line length
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - help wanted
  - libripgrep
assignees: []
created_at: 2016-09-28T17:47:19Z
updated_at: 2017-03-13T01:21:29Z
url: https://github.com/BurntSushi/ripgrep/issues/129
synced_at: 2026-01-12T16:13:21Z
```

# maximum line length

---

_@BurntSushi_

I'd like `ripgrep` to have the ability to either hide or trim lines that are very long. Some lines take up my entire screen and are borderline useless to look at. It's possible that finding an intelligent way to shorten them would be best, since my guess is that the actual matched text is much smaller than the full line. However, this is harder to implement.

I don't think this should be enabled by default. It seems a little surprising for `ripgrep` to hide lines like that. In general, I like the work flow of, "run a search, see huge lines, confirm that I don't care about them and run ripgrep again with an option to hide them." It may however be plausible to enable this limit if results are being dumped to a terminal (we already enable colors, line numbers and file headings).


---

_Label `enhancement` added by @BurntSushi on 2016-09-28 17:47_

---

_Comment by @kaushalmodi on 2016-09-29 00:46_

A very good use case is searching in minified js/css. 


---

_Comment by @BurntSushi on 2016-09-29 00:47_

@kaushalmodi Yup. I'm pretty sure that was precisely the thing I hit. :-)


---

_Comment by @tjayrush on 2016-10-25 18:01_

I'd rather see a Linux-like approach to this. Pipe the file through something like 'tr' first, then pipe the output through 'cut'  Like this: cat file.js | sed 's/;/;\n/' | ripgrep whatever | cut -c1-100

edited: to correct command line to use sed


---

_Comment by @BurntSushi on 2016-10-25 18:07_

@tjayrush What is the purpose of `tr` there?

In any case, `rg` has the opportunity to show better output here, for example, by only showing the match and a few characters around it.


---

_Comment by @tjayrush on 2016-10-25 18:56_

I meant for 'tr' to convert some character into new lines (I forgot to finish editing my comment) to alleviate the long line problem. If 'rg' can already limit to the context that's better. 

The larger point was that instead of adding a new command line option, pipe the output (or input) into another already-existing command line program. 

---

Thomas Jay Rush
jrush@greathill.com

> On Oct 25, 2016, at 2:07 PM, Andrew Gallant notifications@github.com wrote:
> 
> @tjayrush What is the purpose of tr there?
> 
> In any case, rg has the opportunity to show better output here, for example, by only showing the match and a few characters around it.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub, or mute the thread.


---

_Comment by @BurntSushi on 2016-10-25 21:55_

I think my comment still stands.

Also, rg searches recursively by default. In that case, it doesn't make
sense to modify the input.

I don't think you need to argue in favor of piping. I'm all for it. But it
isn't always sufficient.

On Oct 25, 2016 2:56 PM, "Thomas Jay Rush" notifications@github.com wrote:

> I meant for 'tr' to convert some character into new lines (I forgot to
> finish editing my comment) to alleviate the long line problem. If 'rg' can
> already limit to the context that's better.
> 
> The larger point was that instead of adding a new command line option,
> pipe the output (or input) into another already-existing command line
> program.
> 
> ---
> 
> Thomas Jay Rush
> jrush@greathill.com
> 
> > On Oct 25, 2016, at 2:07 PM, Andrew Gallant notifications@github.com
> > wrote:
> > 
> > @tjayrush What is the purpose of tr there?
> > 
> > In any case, rg has the opportunity to show better output here, for
> > example, by only showing the match and a few characters around it.
> > 
> > —
> > You are receiving this because you were mentioned.
> > Reply to this email directly, view it on GitHub, or mute the thread.
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/129#issuecomment-256140440,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34iE21_xLvAOvcbGrT8TAZgCpsleNks5q3lDmgaJpZM4KJHLX
> .


---

_Comment by @BurntSushi on 2016-11-19 15:05_

OK, in the interest of moving forward, here is a proposed specification.

Add a new flag, `-M/--max-columns NUM`, which is disabled by default. When enabled, lines with more than `NUM` columns are suppressed from output. Columns are counted as the number of bytes in a line.

---

I mentioned above it might be cool to only show the context around each match (within a line), but I think that's a complicated feature that deserves its own issue/motivation/specification. I think the above flag solves the most pressing problem.


---

_Label `help wanted` added by @BurntSushi on 2016-11-19 15:05_

---

_Comment by @hpsu on 2016-11-22 09:31_

ag has
```-W --width NUM          Truncate match lines after NUM characters```
but yeah, I would really like this!

---

_Comment by @forgottenswitch on 2016-11-22 15:52_

Spec
------
Make length of printed lines limit-able with conventional COLUMNS environment variable.
If the latter is not a parseable integer, query the tty or Console width.

Every output line should consist of a contiguous input.

Amount of context before first match on output line should be equal to that after last.
Add the `--min-context NUM` parameter which specifies minimal context length.

Print at least one match on every output line.
If contexts of a single match do not fit, decrease the min-context for them.
If a single match itself does not fit, drop its end.

---

_Comment by @BurntSushi on 2016-11-22 16:48_

> If the latter is not a parseable integer, query the tty or Console width.

I would prefer to not add more code that does this to ripgrep. There's already too much of it and it has been a *terrible* pain to maintain. It's sucked up unbelievable amounts of time. Let's just use `COLUMNS` and/or a user-providable setting.

> Amount of context before first match on output line should be equal to that after last.
Add the --min-context NUM parameter which specifies minimal context length.

How does this interact with character encodings? You can't just arbitrarily cut a byte sequence, since you may wind up cutting a UTF-8 sequence in half, for example. While the input isn't necessarily valid UTF-8, our output *better* be valid UTF-8 if the input is also valid UTF-8.

> Print at least one match on every output line.
If contexts of a single match do not fit, decrease the min-context for them.
If a single match itself does not fit, drop its end.

How does this generalize to multiple matches on the same line?

What about lines emitted from `--context/--before-context/--after-context`?



---

_Comment by @forgottenswitch on 2016-11-22 16:58_

> Let's just use COLUMNS and/or a user-providable setting.

But what can go wrong? Explicit setting is meant for editor plugins.

> How does this generalize to multiple matches on the same line?

This paragraph only applies to the case of the single one (on the *output* line).

Lines from `--context` get trimmed at end.

---

_Comment by @BurntSushi on 2016-11-22 17:02_

> But what can go wrong?

I spent my entire weekend dealing with tty stuff on Windows. I don't want to do it again.

---

_Comment by @forgottenswitch on 2016-11-22 17:03_

It's already been dealt with in mingw-w64, and isn't really rust/ripgrep issue as they do obey the tty api.

The tty fiddling (size/isatty) should be in a stable separate crate.

---

_Comment by @BurntSushi on 2016-11-22 17:14_

@forgottenswitch ripgrep has to work with MSVC too.

I don't really feel like debating this much further. I'd like to leave it out for now. We can revisit it later.

---

_Comment by @forgottenswitch on 2016-11-22 17:26_

> How does this interact with character encodings?

All printing is done in UTF-8 characters.
The font is assumed to be monospaced, i.e. not to have characters that take up fractional number of columns.
Each incorrect byte is assumed to take up 1 column.
Each character is assumed to take up 1 column, with the exception of Tab taking 8.
(as long as there is no `Char.len_in_glyphs`; some characters take 0, but ignore it).

---

_Comment by @BurntSushi on 2016-11-22 17:40_

Consider the following line where `foo` corresponds to the match:

```
☃☃☃foo☃☃☃
```

Its UTF-8 encoding looks like this:

```
\xe2\x98\x83\xe2\x98\x83\xe2\x98\x83foo\xe2\x98\x83\xe2\x98\x83\xe2\x98\x83
```

Now consider a `--min-context` of 2. Does `2` refer to Unicode codepoints? Or bytes? If bytes, you'd end up splitting the snowmen on either side of `foo`.

If `2` refers to Unicode codepoints, then I guess you also need to consider graphemes? Should we worry about that? And even if you figure all of that out, you have to realize that the input may not be valid UTF-8.

I see, OK, you updated your comment. You want the `2` to refer to Unicode codepoints, and you want each codepoint to be 1 column. I think that's OK, but we probably want to use the [`unicode-width`](https://crates.io/crates/unicode-width) crate to estimate the display columns of a single codepoint. [But it also works with `&str`, which is nice.](https://unicode-rs.github.io/unicode-width/unicode_width/trait.UnicodeWidthStr.html)

---

_Comment by @forgottenswitch on 2016-11-22 17:51_

OK, will update the pull when done.

---

_Comment by @BurntSushi on 2016-11-22 17:57_

Here are some thoughts that I would like you to consider:

1. I still consider this an incredibly complex feature with lots of edge cases. Could you please write up the full spec in one comment so it's easy to follow? (Editing a prior comment is fine.)
2. Hacking it into the existing printer without a better abstraction is something I *don't* want to maintain.
3. The existing printer, in fact, isn't really something I want to maintain. I would like to refactor it and possibly move it to an external library. I would like you to consider holding off implementing this until then.

---

_Comment by @forgottenswitch on 2016-11-22 18:15_

Spec
------
Make length of printed lines limit-able with conventional COLUMNS environment variable.

Every output line consist of a contiguous input.

Amount of context before the first match on output line should be equal to that after last.
Add the `--min-context NUM` parameter which specifies minimal context length on each side.

Print at least one match on every output line.
If only one match on this output line:
- if contexts do not fit, decrease the min-context for them
- if the match itself does not fit, drop its end

Lines from `--context` get trimmed at end.

All printing is done in UTF-8 characters.
The font is assumed to be monospaced, i.e. not to have characters that take up fractional number of columns.
Each incorrect byte is assumed to take up 1 column.
How many columns each character takes is determined with `unicode-width` crate.

---

_Comment by @forgottenswitch on 2016-11-22 18:18_

> Each incorrect byte is assumed to take up 1 column.
How many columns each character takes is determined with unicode-width crate.

Unclear. Implementing should bring more light.

---

_Comment by @samuelcolvin on 2017-01-06 14:33_

This would be extremely useful. I've just searched a directory which happened to include a few `.min.js` files - the output is basically useless.

I agree `COLUMNS` is the best way to do, preferably without a required switch so things "just work". 

A few suggestions:
* just truncate lines at `COLUMNS x 3` unless a `--no-truncate` flag is set
* show `[2 matches found in line of length 12,345,678 characters]` instead of showing the line if the line is greater than `COLUMNS x 3`.
* use `-uu` or `-uuu` flag. `*.min.js` or any file with only very long lines can basically be considered equivalent to binary (?), or at least they are in my head.

---

_Comment by @forgottenswitch on 2017-01-06 17:31_

> I agree COLUMNS is the best way to do, preferably without a required switch so things "just work".

They don't show up in `env` output. Therefore at least `export COLUMNS` would be required in `.bashrc`. But they are only needed util there is a (reliable) `tty` crate.

Also, `--min-context` should probably be a fraction not to easily exceed COLUMNS/2.

---

_Comment by @samuelcolvin on 2017-01-06 17:36_

COLUMNS works fine on linux:

```
~ || echo "number of columns: $COLUMNS"
number of columns: 80
```

Is anyone serious really using another OS and trying to work in the terminal?

In 2017, I mean really?

---

_Comment by @forgottenswitch on 2017-01-06 17:36_

```
$ env | grep COL
$
```

---

_Comment by @samuelcolvin on 2017-01-06 17:40_

What OS is that?

`env` doesn't work for me either, I need to either reference `COLUMNS` as above or use `printenv`.

---

_Comment by @forgottenswitch on 2017-01-06 17:42_

It is linux, and `man bash` does not say that `COLUMNS` are exported, only that it is "set by the shell".

---

_Comment by @samuelcolvin on 2017-01-06 17:43_

ye, i see the problem now, COLUMNS is available in the shell but not in a subprocess.

`tty` is needed. My mistake, sorry.

---

_Comment by @forgottenswitch on 2017-01-06 17:44_

But maybe `--columns` should still be there to change output width of `rg ... > file`.

---

_Comment by @BurntSushi on 2017-01-06 18:46_

> Is anyone serious really using another OS and trying to work in the terminal?

> In 2017, I mean really?

This issue tracker isn't the place to start an OS war. In particular, ripgrep has first class support for Windows, Mac and Linux. That isn't changing. Comments like "In 2017, I mean really?" aren't helpful.

We can add a `--columns NUM` flag that is disabled by default. If you want it on by default, then create an alias or wrapper script that sets it.

---

_Comment by @samuelcolvin on 2017-01-06 21:10_

sorry, OS comment was a joke really. the "I mean really?" was supposed to exaggerate the point and demonstrate I wasn't being serious. That was lost I guess.

`--columns NUM` would be a good start.

---

_Comment by @BurntSushi on 2017-01-06 21:12_

@samuelcolvin Ah sorry about the misunderstanding! I missed that. Thanks for the clarification. :-)

---

_Comment by @forgottenswitch on 2017-01-07 15:22_

#### Spec

Make length of printed lines limit-able with `--columns NUM`.

Also add `--context-columns NUM[,NUM]`.
The amount is interpreted as mandatory width of match contexts.
Two amounts apply to left and rights contexts, respectively.

Default is `--context-columns 20`.
If a single match does not fit on the output line, the latter becomes
 `<left context><match beginning>[..]<right context>`.

At most `--truncate NUM` (3 by default) lines are printed per input line.
When there still are matches left, an `[N matches at this M characters line]`
pseudo-match, ignoring `--context-columns`, is printed (where M is a decimal-separated number of Unicode codepoints).

When `--truncated-is-binary` is given, a file whose line's matches exceed `--truncate` is considered binary.

In output, invalid codepoints are replaced with a space.
Width of graphemes is determined with the `unicode-width` crate.
The font is assumed to be monospaced, i.e. not to have characters that take up fractional number of columns.
Lines produced by `--context` are trimmed at the end.

---

_Comment by @BurntSushi on 2017-01-07 15:37_

I think i'd like to start with something simpler that just provides a maximum line length. Also "context" already has a meaning in ripgrep.

---

_Comment by @samuelcolvin on 2017-01-07 15:45_

simpler is definitely best IMHO.

The only suggestion I would make would be that with `--truncate NUM` `NUM` defaults to a large but reasonable number. eg. 300. 

You could then set `--truncate 0` for infinite or `--truncate to 5000` for a much larger but finite number.

Eg. the limit is on by default. I know we can configure with aliases but I would guess very few users actually do that, best to have a sensible default. 

As long has you have a clear ellipsis (perhaps in a different colour) the truncation will be clear.

I also think `--truncate` is clearer than `--max-columns` or `--columns`.

---

_Comment by @forgottenswitch on 2017-01-07 15:55_

Spec

Make length of output lines limit-able with `--truncate NUM`.
Fit from start of first match up to end of the last, with even context on both sides, as much as `--truncate` permits.
If fit is impossible, file is considered binary.
When binary files are still searched, `[N matches at this M characters line]` is printed.

In output, invalid codepoints are replaced with a space.
Width of graphemes is determined with the unicode-width crate.
The font is assumed to be monospaced, i.e. not to have characters that take up fractional number of columns.

---

_Comment by @BurntSushi on 2017-01-07 15:57_

We absolutely, positively, cannot make it the default. We *could* make it the default when ripgrep is emitting to a tty (like we've done for colors, line numbers and match groupings). We can't make it the true default because ripgrep (like grep) is useful for filtering data as well, and a tool that automatically drops lines just because they are too long does not sound good to me. I am still weary about making it the default even for just emitting to a tty. I worry that dropping data is a bridge too far. However, we've already kind of crossed that bridge by filtering corpora using `.gitignore` files.

I would be fine with using `--truncate` as the name. However, this does imply that some part of the line is still visible instead of being dropped completely, which I imagine is also useful. I'd like to only add a single flag for this.

Finally, when `--truncate` is given, does it specify bytes, Unicode codepoints or graphemes? There are performance *and* usability trade offs here. Simply hiding the lines is considerably simpler.

---

_Comment by @BurntSushi on 2017-01-07 15:59_

@forgottenswitch I would like to move away from "fitting" the lines. I think we should consider two options: simple truncation or complete hiding.

---

_Comment by @forgottenswitch on 2017-01-07 16:00_

I expected that as the simpler way, but isn't it the https://github.com/BurntSushi/ripgrep/issues/129#issuecomment-261718964 with columns counted as graphemes?

---

_Comment by @samuelcolvin on 2017-01-07 16:03_

I would say truncation was preferable and "columns" in the terminal (whatever that translates to) would be best but, at least for me.

---

_Comment by @BurntSushi on 2017-01-07 16:06_

@forgottenswitch I'm sorry, but I don't understand. [This comment](https://github.com/BurntSushi/ripgrep/issues/129#issuecomment-271091507) mentions "fitting" the lines and looking at matches.

What I'm trying to say is this: if we can avoid writing a specification that needs to do something that is *encoding aware*, then this feature becomes much simpler. The `--max-columns` flag, which simply drops lines that are too long, achieves this because it uses the length of a line in bytes. The `--truncate NUM` flag, however, poses interesting difficulties. If `NUM` is bytes, then it's possible to naively emit invalid UTF-8 even when the input is completely valid UTF-8.

@samuelcolvin "columns" in the terminal is a visual thing. A single column can contain an *arbitrary* number of bytes because of combining characters in Unicode. Checking if every line satisfies this limit is *not* feasible because of the performance hit it requires.

---

_Comment by @BurntSushi on 2017-01-07 16:08_

The reason why I like having `--max-columns` disabled by default (even for emitting to tty) is that it makes *dropping* the lines a specifically user driven action. The `--truncate NUM` approach satisfies this by marking truncatation with ellipsis, but then you wind up with other problems described in my previous comment. We could add a short flag, `-M`, to make this a bit easier to apply iteratively.

---

_Comment by @samuelcolvin on 2017-01-07 16:11_

I understand why don't want to apply it by default, fine with me.

For length, I guess just bytes is fine. The primary use case is just to make viewing easier where a line which shows 111 columns instead of 120 is no problem. I'm guessing 99% of ripgrep usage is on ascii code anyway.

---

_Comment by @BurntSushi on 2017-01-07 16:13_

> I'm guessing 99% of ripgrep usage is on ascii code anyway.

@samuelcolvin Possibly. I do track mentions of ripgrep on the Internet, and there seems to be quite a few mentions of it in otherwise Chinese, Russian and Japanese publications.

---

_Comment by @forgottenswitch on 2017-01-07 16:16_

> The --truncate NUM flag, however, poses interesting difficulties. If NUM is bytes, then it's possible to naively emit invalid UTF-8 even when the input is completely valid UTF-8.

Backtrack to beginning of the last character?
 In UTF-8, it is "backtrack to last non-10xxxxxx byte".

Also, `--truncate[-as-utf] NUM` (or `-MM`) for slow mode, considering graphemes.
But then again, -utf depends on unicode-width.

Fitting was to improve both readability and pager's scrolling of overly long lines
(dozens of screen lines, either from binary or minified/log files).
It probably doesn't matter at all because 1. terminal scroll could be used instead of pager 2. rg is for code search
It would have also be slow by requiring utf parsing.
If grapheme width determination was too slow, maybe a dedicated thread could have been tried.

Maybe ripgrep should just consider any file having a line longer than 256 bytes a binary.

---

_Comment by @samuelcolvin on 2017-01-07 16:19_

@forgottenswitch you seem determined to make this complicated. :-)

Surely best to start with the simplest and most obvious solution and make it more complicated when required.

utf8 counting would be nice but isn't required; anything else is surely excessive at this stage.

---

_Comment by @forgottenswitch on 2017-01-08 04:18_

OK, I tried addressing different problem - printing matches even in huge lines, instead of omitting them.

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-11 03:05_

---

_Comment by @forgottenswitch on 2017-01-26 16:33_

Context before a match could be ellipsized, and total number of matches on a line limited.
 [Implementation](https://github.com/BurntSushi/ripgrep/compare/master...forgottenswitch:hctx_compress), [benchmark](https://gist.github.com/forgottenswitch/a8575db29ca9c99633d582dca24c24a4).
Incorrect UTF-8 is handled by `String::to_utf8_lossy`-ing the context.

---

_Comment by @RalfJung on 2017-02-02 12:37_

I have to say I wouldn't find just dropping lines very useful; I still may want to know that these lines matched, but I also want to be able to see other matches. I hacked something quick-n-dirty together in <https://github.com/RalfJung/ripgrep/tree/longlines>, but I'm not proposing this as a solution to anything, I was just looking for an excuse to write some Rust code ;)

The simplest thing I can come up with that I'd actually like to use is something where we can set a `--max-width` or so, and if the line in bytes is longer than that, instead of printing the line, it prints how many matches were found in the line. So nothing would be entirely omitted. Would that be accepted? I'd be happy to give it a try.

Of course, something that actually ellipsizes long lines and prints them as "... context match1 context ... context match2 context ..." would be better, but then as you discussed above things quickly become really complicated because unicode.


---

_Comment by @BurntSushi on 2017-02-02 12:43_

> The simplest thing I can come up with that I'd actually like to use is something where we can set a --max-width or so, and if the line in bytes is longer than that, instead of printing the line, it prints how many matches were found in the line. So nothing would be entirely omitted. Would that be accepted? I'd be happy to give it a try.

@RalfJung I think I like that idea. It would have to be disabled by default though (and perhaps enabled by default when emitting to a tty).

---

_Comment by @RalfJung on 2017-02-02 14:09_

All right, I prototyped this at <https://github.com/RalfJung/ripgrep/commit/9618f87aefa8fc1ee821b4d3d7797a5542762f24>. This is just about the behavior if the option is set (to 80), obviously; what remains to be done is adding the CLI argument, deciding about the default, and wiring the printer to that.

There are some open questions however already on this level:
* What to do in "replace" mode? Right now, the option would not affect that mode at all. I think this makes sense. Probably, in replace mode, you actually want to do something with that replaced text.
* Dies ripgrep support any form of internationalization? I now plugged an English string right into there.
* Bikeshedding about the way the message is displayed. ;)
* How to write the newline after the message. I currently just call `write_eol`, but it seems like it could happen that `write_match` then calls `write_eol` again for the same line. What is the best way to avoid that? To avoid duplicate newlines, I could test the exact negative of what `write_match` tests when writing the "stuff omitted" message, but that sounds like an awful idea. I could move the check performed by `write_match` into `write_matched_line`, but that will lead to code duplication. So I guess the cleanest thing to do is to write a little function `write_eol_if_missing_from_buf` or so, that takes an `&[u8]` and calls `write_eol` if that buffer does not end in a newline.
* What to do with context lines? Should it just silently not write them (could lead to weird gaps, where some context line is printed and others are not), or print a message saying that a line has been omitted?

EDIT: Slightly updated version at <https://github.com/RalfJung/ripgrep/commit/50c07fc55a41100c9c10b1da57689428036029e6>

---

_Comment by @forgottenswitch on 2017-02-02 15:39_

> Of course, something that actually ellipsizes long lines and prints them as "... context match1 context ... context match2 context ..." would be better, but then as you discussed above things quickly become really complicated because unicode.

I posted implementation above; it does not seem to be so.

---

_Comment by @RalfJung on 2017-02-03 10:46_

Wow, impressive! That does look nice indeed. I somehow missed these links before, sorry.


---

_Comment by @BurntSushi on 2017-02-03 12:03_

I would like to avoid the contextual display. That should be a separate issue. I do not want to bring that code (if at all) until libripgrep is done. The code and the feature are too complex.

@RalfJung I don't have rock solid answers for your questions, but here's an attempt:

* If the `-r/--replace` flag is used, then ignoring `--max-width` doesn't seem quite right. I think it should still be applied to the *result* of the replacement.
* ripgrep has no internationalization.
* We can bikeshed in the PR.
* I'm not sure about the eol writing. I'd have to look more closely.
* For context lines, I think printing something like "context line omitted because of --max-width" is reasonable.

---

_Comment by @RalfJung on 2017-02-03 12:13_

> If the -r/--replace flag is used, then ignoring --max-width doesn't seem quite right. I think it should still be applied to the result of the replacement.

So should it also still show the number of matches? That'd be extra work, unless there is a way I don't know about to get this from `re.replace_all`.

---

_Comment by @BurntSushi on 2017-02-03 12:16_

@RalfJung `re.replace_all` accepts anything that satisfies the `regex::bytes::Replacer` trait (including `FnMut(&Captures) -> Vec<u8>` or your own type), which should enable you to both do the replacement and count the number of times it occurred.

---

_Comment by @RalfJung on 2017-02-11 17:58_

Right, I could use a closure or implement the trait for a custom type that also does the counting... but that will cost some performance, it seems, since we *want* to be called for every match and hence cannot use the `no_expansion` optimization. 

---

_Closed by @BurntSushi on 2017-03-13 01:21_

---
