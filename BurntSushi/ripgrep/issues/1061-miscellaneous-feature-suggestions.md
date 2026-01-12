```yaml
number: 1061
title: Miscellaneous feature suggestions
type: issue
state: closed
author: BatmanAoD
labels:
  - question
assignees: []
created_at: 2018-09-21T19:12:17Z
updated_at: 2018-11-24T13:14:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1061
synced_at: 2026-01-12T16:13:22Z
```

# Miscellaneous feature suggestions

---

_@BatmanAoD_

These are just some ideas about what features I think would make a recursive-grep tool such as RipGrep more useful.

I've looked through the issues list (both open and closed) for any duplicates, and I did find one (which I deleted from my list), but otherwise I believe these are unique. If any of them are of interest, let me know if you'd like me to write up a separate tracking issue for them.

And if you think any of them would be reasonably straightforward to implement, perhaps I could help with that as well!

#### What version of ripgrep are you using?

0.10.0

#### How did you install ripgrep?

`cargo` with the `pcre2` feature, using Rust 1.29

#### What operating system are you using ripgrep on?

Windows 10, "Fall creators update"

#### Describe your question, feature request, or bug.

 * `--post`, comparable to `--pre`; for each file, launch the specified command with the filename as an argument, and *pipe* the output *from that file* as input
    * I'm surprised I didn't see this in the issues list already; is it there and I just didn't see it?
    * Together with `replace` and a bit of scripting, this would provide an easy and possibly more performant replacement for `rg -l <pattern> | perl -pi -e 's/pattern/repl/g'`. It would also *guarantee* no surprise inconsistencies between RipGrep's and Perl's interpretations of `<pattern>` (though in practice such inconsistencies are probably vanishingly rare), and could prevent typos from duplicating the pattern on the command line (though this can be avoided by creating a "recursive search-and-replace" shell function).
 * Some way to provide arguments to a `--pre` (or `--post`, if implemented) command
 * Some way to provide context about nearby lines, e.g. "do not match lines that have `<pattern>` in the *previous* line." This would probably be especially useful in conjunction with #875.
 * A way to "pre-package" pattern elements. These could then be saved in the config file. For instance (adopting an arbitrary Perl-like syntax for the feature):

       --define-pattern "win-path /(?:[A-Z]:)?\w+(?:\\?\w+)*

   I'm honestly not sure that's an accurate representation of Windows paths yet, and it's already pretty hideous. It could be used as follows (again adopting an arbitrary syntax):

       rg "[[{win-path}]]"       # This would find lines with windows paths

---

_Comment by @okdana on 2018-09-21 19:20_

The second one was discussed and rejected in #978.

The ~~last~~ third one is kind of the same thing as #1008, which was also rejected. Now that searching across lines is supported i guess that could be used in some (probably simpler) cases.

---

_Comment by @BatmanAoD on 2018-09-21 19:20_

(Update: added `--define-pattern` suggestion)


---

_Comment by @BatmanAoD on 2018-09-21 19:30_

@okdana Thanks for pointing me to the discussion about `--pre` arguments. I don't see any example use-cases there; I was thinking about doing something like `rg --pre 'rg -v <bad>' <good>`.

---

_Comment by @okdana on 2018-09-21 19:35_

You would make a script like this (call it `filter` for example):

```
#!/bin/sh
rg -v bad ${1+"$1"}
```

And then use it like:

```
rg --pre /path/to/filter good
```

---

_Comment by @BurntSushi on 2018-09-21 21:19_

> `--post`, comparable to `--pre`; for each file, launch the specified command with the filename as an argument, and pipe the output from that file as input

As described, I don't think I understand how `--post` differs from `--pre`. `--pre` is literally, "called with the file path being searched, along with its contents piped to stdin; stdout of that program is then search as if it were the original file."

> Some way to provide arguments to a `--pre` (or `--post`, if implemented) command

This is another example where I was on the fence about a specific feature (`--pre`), but relented because it enabled some really cool use cases in exchange for fairly little work and implementation complexity, but because I relented, that feature is now in turn generating even more feature requests. I'm just going to say No. It isn't necessary and it's _already_ a niche feature that I suspect most users of ripgrep don't even know about, much less use. It isn't worth making it more complex than the simple implementation that it is now.

> Some way to provide context about nearby lines, e.g. "do not match lines that have <pattern> in the previous line." This would probably be especially useful in conjunction with

My stance on this is outlined [here](https://github.com/BurntSushi/ripgrep/issues/1008#issuecomment-411175925) in #1008. It hasn't changed.

> A way to "pre-package" pattern elements. These could then be saved in the config file. For instance (adopting an arbitrary Perl-like syntax for the feature):
--define-pattern "win-path /(?:[A-Z]:)?\w+(?:\?\w+)*
I'm honestly not sure that's an accurate representation of Windows paths yet, and it's already pretty hideous. It could be used as follows (again adopting an arbitrary syntax):
rg "[[{win-path}]]" # This would find lines with windows paths

I don't think this is a good fit. I'd rather see folks write wrapper scripts or aliases if they want quick shortcuts for common patterns. It is much more flexible. Moreover, the `-f/--file` flag already exists, which is perfectly capable of loading patterns from files.

I think the only thing in question here is the `--post` flag, on account of the fact that I don't understand the request. The rest has either been discussed or are things I currently don't think are a good fit.

---

_Label `question` added by @BurntSushi on 2018-09-21 21:20_

---

_Comment by @Snuggle on 2018-09-30 21:11_

Could I please add to this and ask for an equivalent of the `-Z` flag to be added for compatibility purposes? I believe its purpose is extremely similar to `-z`, which has already been added.

![image](https://user-images.githubusercontent.com/26250962/46262757-96c40980-c4fd-11e8-87f5-d2b86a55d2cd.png)
![image](https://user-images.githubusercontent.com/26250962/46262766-a80d1600-c4fd-11e8-8db8-c00c949b13b1.png)
https://linux.die.net/man/1/grep

---

_Comment by @BurntSushi on 2018-09-30 21:22_

Please don't keep adding new feature requests to the same issue. To that end, @BatmanAoD, please do not open one issue for multiple distinct requests.

@Snuggle The desired functionality already exists using literally the same flag: `--null` (its short flag is `-0`, not `-Z`).

---

_Closed by @BurntSushi on 2018-09-30 21:22_

---

_Comment by @Snuggle on 2018-09-30 21:36_

Thank you for the help, @BurntSushi, and apologies for thinking this issue was for multiple general feature requests.

---

_Comment by @BatmanAoD on 2018-10-02 00:10_

Sorry for the confusion on `--post` and, subsequently, for forgetting to answer your question.

`--post` would take *RipGrep's* output and pipe it into a new command. Since RipGrep is parallel by default, but Bash's `|` only forks one consuming process, there's no easy way without something like `--post` to parallelize processes that consume RipGrep output, or to distinguish which search-output came from which file.

---

_Comment by @BatmanAoD on 2018-10-02 00:21_

For my "pre-packaged patterns" idea; `--file` can't be used to load *parts* of patterns, can it? What I want is a way to take a pattern "segment" and integrate it into something longer. Here's a dumb example, using the same syntax as above:

    git diff | rg 'a[[{win-path}]]$'           # Find original names of files in a `git diff`

I suppose this isn't too difficult using `sh` variables, assuming a non-Windows shell:

    git diff | rg 'a'"${win_path}"'$'

....the quoting is pretty awkward, though.

---

_Comment by @cuongnv-ibl on 2018-11-23 07:43_

Does `rg` support clear duplicated text line in a text file?

---

_Comment by @BurntSushi on 2018-11-23 12:09_

I don't know what you're talking about, sorry. What relevance is your question too this specific issue? If there isn't any, then please open a new issue. Please include examples to help explain what you mean.

---

_Comment by @cuongnv-ibl on 2018-11-24 02:15_

sorry, my mistake, it means can I use `rg` to remove duplicate lines in a .txt file and save the new file as new.txt file.
alternative to `sort myfile |uniq -u|tee newfile.txt`
Because I tried use `sort` command but seem it doesn't work properly, some duplicate lines still exist

---

_Comment by @BurntSushi on 2018-11-24 13:14_

ripgrep is a search tool. It is not a tool for removing duplicate lines. If
you don't care about order, then sort | uniq is the usual way to do that.
If you're having a problem with it, I'd recommend seeking help elsewhere
(like stackoverflow).

On Fri, Nov 23, 2018, 21:15 cuongnv-ibl <notifications@github.com wrote:

> sorry, my mistake, it means can I use rg to remove duplicate lines in a
> .txt file and save the new file as new.txt file.
> alternative to sort myfile |uniq -u|tee newfile.txt
> Because I tried use sort command but seem it doesn't work properly, some
> duplicate lines still exist
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1061#issuecomment-441337720>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34m0MEZi5CE9DvWv8ZCQefPblFb5_ks5uyKvKgaJpZM4W0uLq>
> .
>


---
