---
number: 6069
title: Missing dynamic completions for arguments containing shell expansions
type: issue
state: open
author: krobelus
labels:
  - A-completion
  - S-waiting-on-design
assignees: []
created_at: 2025-07-12T18:15:41Z
updated_at: 2025-08-11T20:53:20Z
url: https://github.com/clap-rs/clap/issues/6069
synced_at: 2026-01-07T13:12:20-06:00
---

# Missing dynamic completions for arguments containing shell expansions

---

_Issue opened by @krobelus on 2025-07-12 18:15_

Repro (using Jujutsu CLI, I can provide a minimal repro later):

```
$ bash
$ COMPLETE=bash jj __this-command-does-not-exist >? tmp.sh
$ . ./tmp.sh
$ jj --config-file ~/<Tab>
```

There are no completions. Same on `jj --config-file $HOME/` etc.

Two options to resolve this:
1. Don't compute any completions for `AnyPath` args but tell the shell to
   complete as if there were no custom completions. This is what Cobra does.
2. Try to expand and unexpand shell syntax ourselves, so we can actually compute completions.

This is quite tricky.
I wonder if we want 1 as "temporary" measure.

---

_Comment by @epage on 2025-08-07 20:59_

Hmm, for bash, if I do `ls $HOME/<TAB>`, I don't get completions either.

I'd be fine with us adding support for `~/` completions.  That can easily be done in our own code.  Expanding `~foo/` might be more tricky but I suspect thats rare enough to not matter.  While there are issues with [`std::env::home_dir`](https://doc.rust-lang.org/std/env/fn.home_dir.html) on Windows with versions of Rust lower than our MSRV, I think it might be reasonable to go ahead and just use that.

Unsure what all it would look like to try to get it to work to delegate to the shell, especially without feature loss.

---

_Label `A-completion` added by @epage on 2025-08-07 20:59_

---

_Label `S-waiting-on-design` added by @epage on 2025-08-07 20:59_

---

_Referenced in [clap-rs/clap#6094](../../clap-rs/clap/pulls/6094.md) on 2025-08-07 21:11_

---

_Comment by @epage on 2025-08-07 21:30_

`clap_complete` 4.5.56 is now out with completing of `~/` paths.  I did not close the issue in case you had enough need for completing other types of paths referenced here.

---

_Comment by @krobelus on 2025-08-09 20:39_

> Hmm, for bash, if I do `ls $HOME/<TAB>`, I don't get completions either.

I do get them on bash 5.3.3, even in "bash --norc -x"

> I'd be fine with us adding support for `~/` completions.

thanks, that's probably the biggest offender 

> Unsure what all it would look like to try to get it to work to
> delegate to the shell, especially without feature loss.

In fish it's possible with something like "complete -n try-to-compute-completions -fa '$computed_completions'"
but yeah, delegation would cause feature-loss,
for example the shell's default wouldn't know if we want file paths or directory paths;
but IME false positive completions are much better than false negatives.


For my quick design of a long-term solution, consider 

	jj --repository $repo {abandon,x

We can solve this by
1. have the shell expand this to
   (we can't do it ourselves since we don't know $repo)

	jj --repository /path/to/repo abandon x

2. ask this clap CLI for completions for that command.
3. Pass the results (something like "x1 x2 ...")
   back to the shell.
4. The shell will apply completions to the original (unexpanded) commandline.
   Cycling through completions will turn it into something like

	   jj --repository $repo {abandon,xFOO

   where "xFOO" is the completion.

This would first require changes to shells.

We can't use the `COMPREPLY` interface as-is because
1. we don't want to replace the entire whitespace-delimited token
2. we may not want to escape special characters in the completion

There are some edge cases I didn't mention yet.
I think we can address them if we allow adding metadata to completions (similar to how LSP completions work).
So each completion candidate will be a structured data object.

I also use clap dynamic completions in [Kakoune's jj integration](https://github.com/krobelus/kakoune/blob/594c23754503924c6c16ec01f70c9689d4569379/rc/tools/jj.kak#L319),
which uses yet another shell-like language - so this could
benefit too.
(It'd be ideal if the ompletion candidate object would be easy to parse from shell.)

It's unclear how many shells will want to implement this.
I think it would be nice; it would fix a bunch of long-standing issues.
Not yet sure if it will be worth the complexity or when I'll have time to work on this.


---

_Comment by @epage on 2025-08-11 20:53_

Ah, I'm on bash 5.1.16

> We can solve this by

This sounds pretty complicated.  We're unlikely to take on investigating this but welcome others doing so.

> I think we can address them if we allow adding metadata to completions (similar to how LSP completions work).
> So each completion candidate will be a structured data object.
> (It'd be ideal if the ompletion candidate object would be easy to parse from shell.)

My concerns about providing a generic interface for querying completions
- Completion inputs are very shell specific and we may have reason to change how we do it
- For parsing completion outputs, coming up with a format that every shell can easily work with is non-ideal.  Similarly, we'd need to come up with a schema for that format.  I'm also not thrilled about having these in our compatibility interface specially because that would then be propagated out to the interface for the users of clap which would make it a lot harder to change than just bumping to `clap_complete` v6.

---

_Referenced in [clap-rs/clap#5998](../../clap-rs/clap/pulls/5998.md) on 2025-08-26 19:46_

---
