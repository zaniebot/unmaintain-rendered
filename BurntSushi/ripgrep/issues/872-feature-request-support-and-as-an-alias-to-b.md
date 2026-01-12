```yaml
number: 872
title: "[Feature request] Support \\< and \\> as an alias to \\b"
type: issue
state: closed
author: bstaletic
labels:
  - enhancement
  - question
assignees: []
created_at: 2018-03-30T19:02:32Z
updated_at: 2018-08-07T11:49:29Z
url: https://github.com/BurntSushi/ripgrep/issues/872
synced_at: 2026-01-12T16:13:22Z
```

# [Feature request] Support \< and \> as an alias to \b

---

_@bstaletic_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### What operating system are you using ripgrep on?

Gentoo linux

#### Describe your question, feature request, or bug.

The `\<` and `\>` style word boundaries come from Vi (ex?) and work in grep. A reason to have these supported is to allow seamless use of Vim's regexes when a Vim user has `set grepprg=ripgrp\ --vimgrep`. The most annoying use case is the following:

- User has the cursor over a `word` he would like to search for in the file and presses `*`.
- After looking through the file, user decides to search the whole project for that word.
- User types `:grep <C-r>/` and is greeted with `:grep -r  \<word\>`.
- User presses enter.
- At this point user might expect the pattern to work, but is greeted with an error.

Yes, there are workarounds for this, but it would be realy nice if Vim's word boundary matching "just worked".

---

_Comment by @BurntSushi on 2018-03-30 22:31_

In principle, I am not opposed to this, but I will put these things out there:

* `\<` and `\>` are very explicitly *not* aliases of `\b`, but rather, match only the beginning and end of words, respectively. `\b` matches any word boundary, either beginning or end.
* This isn't really something that is implemented in ripgrep, but rather, in the [regex engine](https://github.com/rust-lang/regex).
* It is quite likely that even if I agree to add this, it will be a very very long time before I personally work on it. Moreover, the way word boundaries work in the current regex engine is quite brittle and is a source of (outstanding) bugs, and therefore complicating it further in its current form would be a bad idea IMO.

Given the above, I suggest you find a way to make peace with their absence for now. :-)

---

_Comment by @okdana on 2018-03-30 22:57_

Also, adding non-alphanumeric escape sequences to the regex syntax would be a break from Perl-style syntaxes, where any escaped non-word character is always equivalent to the literal character (`\.` is literal `.`, `\/` is literal `/`, `\#` is literal `#`, &c.). `regex` doesn't support that currently (it's an error if the escaped character is not a recognised meta-character), but you do have the option to add it, without breaking BC, in the future — if you implement `\<` `\>` you'll be committing yourself to the (rather confusing, IMO) notion that escaped non-alphanumerics are sometimes literal and sometimes not.

Not sure if that would bother you at all (and it doesn't sound immediately relevant anyway), but it was something that occurred to me when i saw this.

---

_Comment by @bstaletic on 2018-03-30 23:06_

> `\<` and `\>` are very explicitly *not* aliases of `\b`

Strictly speaking, that's true. My thoughts were "it's close enough".

> This isn't really something that is implemented in ripgrep, but rather, in the regex engine.

I wasn't aware regex engine is a separate project.

> Moreover, the way word boundaries work in the current regex engine is quite brittle and is a source of (outstanding) bugs, and therefore complicating it further in its current form would be a bad idea IMO.

We have all fought messy code before, so your point of view is more than reasonable.

> Given the above, I suggest you find a way to make peace with their absence for now. :-)

I've been using ripgrep, ag and grep without knowing that grep supports `\<` until recently, so I can live without that support, but I must admit that it would be very convenient.

---

_Comment by @BurntSushi on 2018-03-30 23:46_

@okdana Ah good point! The escaping strategy used in `regex` was intentionally designed that way, exactly for cases like these where we can add new escape sequences in a backwards compatible way. :-)

I hadn't considered the notion about escaping non-alphanumerics being confusing though, that's an interesting point.

---

_Comment by @BurntSushi on 2018-03-30 23:48_

@bstaletic One thought here, for your sake: if your use case revolves around one very specific type of invocation of ripgrep in your vim workflow, you might be able to write a wrapper script that replaces `\<` and `\>` with `\b`. I realize that's a pain of course, and I don't necessarily suggest it to the exclusion of adding proper support for `\<` and `\>`, but as a way to hold you over.

---

_Comment by @bstaletic on 2018-03-31 05:54_

Heh, why didn't I think of making some sort of key binding that would replace `\<` and `\>` with `\b`? Thanks for the suggestion.

I have managed to rebind enter to do this substitution. The `TODO:` below doesn't bother me too much as I never use `:vimgrep`.

```viml
function! slash#SubstituteWordBoundaries() abort
	let l:cmd = getcmdline()
	" TODO: make sure this is calling grep, not vimgrep
	return substitute(l:cmd, '\\[<>]', '\\\\b', 'g')
endfunction

function! slash#callBlink() abort
	if getcmdtype() == '/' || getcmdtype() == '?'
		return "\<cr>:call slash#blink(3, 100)\<cr>"
	else
		if &grepprg != 'grep'
			return "\<C-\>eslash#SubstituteWordBoundaries()\<cr>\<cr>"
		endif
		return "\<cr>"
	endif
endfunction
```

The above snippet is straight from my `.vim/autoload/slash.vim`, which isn't properly named now when I fiddle with `:` and not only with `/`.

The function above gets called from my `.vimrc` like this: `cnoremap <silent> <expr> <enter> slash#callBlink()`. The only downside - it actually changes what was typed, so history doesn't contain what the user typed, but something similar.

@BurntSushi Thanks for the suggestion. If you decide not to allow `\<` and `\>`, feel free to close this feature request.

---

_Comment by @BurntSushi on 2018-04-23 23:46_

I've opened an issue against the regex crate: https://github.com/rust-lang/regex/issues/469

This feature is basically entirely contained in the regex crate, so I'm not going to track it here.

---

_Closed by @BurntSushi on 2018-04-23 23:46_

---

_Label `enhancement` added by @BurntSushi on 2018-04-23 23:46_

---

_Label `question` added by @BurntSushi on 2018-04-23 23:46_

---

_Comment by @toonn on 2018-08-06 23:27_

Could you add something to the man page about this? Currently searching the manpage for either `word boundary` or just `boundary` doesn't turn up *any* results. It's also somewhat hard to google for turning up a bunch of results about `-w` which don't seem related, i.e. I wanted to use boundaries in a larger regexp so `-w` was not an option.

Ideally "word boundary" and "\<" and "\>" would all turn up at least one result in the man page.

---

_Comment by @BurntSushi on 2018-08-06 23:33_

@toonn The man page should direct you to the description of the regex syntax, which can be found here: https://docs.rs/regex/1.0.2/regex/#syntax

With that said, yes, it would be nice to be in the man page. Actually, it would be nice if the entire regex syntax were documented in the man page, but that's a lot of documentation to have duplicated. Although, it shouldn't change very often.

I would be OK with a new section, `REGULAR EXPRESSION SYNTAX` that included the aforementioned link and any additional callouts that might be useful to folks, in particular, differences from other regex engines, in lieu of including the full syntax description.

---

_Comment by @bstaletic on 2018-08-07 02:59_

Another option would be creating a completely new man page for the syntax and then creating a script that would convert the page above into the man page.

---

_Comment by @BurntSushi on 2018-08-07 11:49_

@bstaletic Neat idea. I'd be open to that.

---
