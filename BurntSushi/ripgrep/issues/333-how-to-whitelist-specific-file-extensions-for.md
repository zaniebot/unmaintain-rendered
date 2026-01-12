```yaml
number: 333
title: "How to **whitelist** specific file extensions for listing with '--files'"
type: issue
state: closed
author: n00bmind
labels:
  - question
assignees: []
created_at: 2017-01-18T13:52:55Z
updated_at: 2024-03-26T19:07:35Z
url: https://github.com/BurntSushi/ripgrep/issues/333
synced_at: 2026-01-12T16:13:21Z
```

# How to **whitelist** specific file extensions for listing with '--files'

---

_@n00bmind_

Hi.
I'm using ripgrep primarily inside vim/ctrlp.
I'm currently working on a very big repository (tens of thousands of files) with a, let's say "relaxed" policy about what should be in version control and what should not, meaning I have to filter out a lot of binaries, build byproducts and whatnot.
I'm of course using a .ignore file at the root, which helps with speeding up the listings, but I need an additional layer of filtering to weed out so much "noise", so I'm trying to find an adequate syntax to use for the 'ctrlp_user_command' variable in vim which would _whitelist_ just what I'm interested in (only certain types of source files).
So far, I've obtained the best results by using the 'type-add' switch using the curly braces notation to include several file extensions, like this:
`rg . --files --type-add "source:*.{h,cs,c}" -tsource`

This _almost_ works, I can see most of the files listed have the extensions I specified, but there's also many unwanted files, like these two for instance:
`tools\webscarab\src\org\owasp\webscarab\plugin\scripted\script.bsh`
`tools\win32\python27\Lib\site-packages\data\themes\tools\icons48.code.tga`

For the first one I have no explanation. The second one would seem as if the '.code' part at the end was matching my '.c'.. Wild speculation, of course.

Any ideas?

---

_Comment by @BurntSushi on 2017-01-18 14:07_

What happens if you try:

```
rg . --files --type-add "source:*.{h,cs,c}" -tsource --debug
```

and

```
$ rg . --files -g '*.{h,cs,c}' --debug
```

---

_Comment by @BurntSushi on 2017-01-18 14:07_

Note that the `--debug` flag might cause a lot of output to `stderr`. If you can stick that in a gist or a pastebin, that would be great.

---

_Label `question` added by @BurntSushi on 2017-01-18 14:07_

---

_Comment by @n00bmind on 2017-01-18 18:11_

I don't feel too comfortable sharing details about this particular project, so I'll try to find a suitable example and provide the info you requested..

---

_Comment by @n00bmind on 2017-01-18 18:35_

Ok, I'm using the Android SDK for this..
https://gist.github.com/chopsueysensei/bd0d7908f8d1b628cbb47b33a9b551b5

(I hope you can see all of it, it's pretty long)
I can see that it whitelists several things like images, jars and other things.
I'd say that paths that include a '.' somewhere are causing some trouble..

---

_Comment by @BurntSushi on 2017-02-18 19:18_

@chopsueysensei Could you please include the command you ran? Could you also tell me how to clone the repo you're searching?

---

_Comment by @BurntSushi on 2017-02-18 19:19_

I need enough information to reproduce the problem.

---

_Comment by @n00bmind on 2017-02-22 15:34_

The commands are the ones you asked me to run.
"rg_g_debug.txt" contains all the output from the command `rg . --files -g '*.{h,cs,c}' --debug`, while "rg_t_debug.txt" contains all the output from `rg . --files --type-add "source:*.{h,cs,c}" -tsource --debug`.

The tree is just my current Android SDK folder.

---

_Comment by @BurntSushi on 2017-02-22 15:39_

> The tree is just my current Android SDK folder.

Could you please tell me how to get it?

---

_Comment by @n00bmind on 2017-02-22 15:41_

Just install Android SDK anywhere in your HD.. maybe also open "SDK Manager.exe" located in the root folder and download a couple platform versions / optional components to add some more content to it..

---

_Comment by @BurntSushi on 2017-02-22 15:45_

@chocolateboy I'm not on Windows. I've never used the Android SDK before. Can you please link me to some instructions on how to acquire it? *I need to be able to reproduce your problem*.

---

_Comment by @n00bmind on 2017-02-22 15:53_

https://developer.android.com/studio/index.html#downloads

However, if you're not on windows, you probably won't get the same output right?

---

_Comment by @n00bmind on 2017-02-22 15:54_

Download the one under 'get just the command line tools'.
It should be a matter of unzipping then running.

---

_Comment by @cheater on 2018-01-26 19:40_

The android sdk is a red herring. You should be able to just create a file called foo.code.tga which is eg an ascii file with C inside it (for example) and it should be able to instruct rg to not find it.

---

_Comment by @cheater on 2018-01-26 19:42_

(by C i mean C source of course)

imo the perfect resolution would be to add something like gnu find syntax for specifying file names. At least -path, -name, -ipath, and -iname as well as -not, -o, -a, -\(, and -\).

Or at least -ipath and -iname for starters.

---

_Comment by @okdana on 2018-01-26 19:56_

* `rg`'s `--glob` and `--iglob` are effectively the same thing as `find`'s `-name`/`-path` and `-iname`/`-ipath` (though the semantics\* are slightly different obviously)
* `!`-prefixed patterns are effectively the same thing as `-not`
* as in `find`, multiple glob patterns are combined with an implicit `-a` by default

\* The pattern functionality is mostly as described here: https://git-scm.com/docs/gitignore

---

_Comment by @cheater on 2018-01-26 19:58_

Then I guess the issue can be closed as resolved? With the caveat that the original reporter should be able to reopen it if they are unhappy with the features rg provides.

---

_Comment by @n00bmind on 2018-01-28 11:34_

Is --glob the same as -g?
In that case, I already tried that as commented in my original post, and it still had some issues. It almost worked, but some files gave false positives..
My intention when opening this was more in the direction of bug catching & fixing.. I since have not used vim again in large codebases, so cannot attest as to how rg behaves currently in that scenario.


---

_Comment by @BurntSushi on 2018-01-31 02:04_

I am going to close this because it's not reproducible. If someone can come up with a contained example that uses something more accessible than the entire Android code base, then I can take a look and re-open this.

---

_Closed by @BurntSushi on 2018-01-31 02:04_

---

_Comment by @alper on 2020-11-12 12:48_

Thte manpage says:

    Only search files matching TYPE.

That does not help me to figure out what TYPE can be. I figured out in my case I have to say `-tgo` but that's fairly counterintuitive.

---

_Comment by @BurntSushi on 2020-11-12 12:50_

@alper How is `-tgo` counter intuitive? Please consider reading the [guide's section on filtering with file types](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types).

---

_Comment by @alper on 2020-11-12 12:55_

Oh cool. The guide has examples so that makes it a lot easier. The man page does not.

Values concatenated onto the flag is not something I see a lot? I would expect: `-t go` but that could be just me.

---

_Comment by @BurntSushi on 2020-11-12 13:08_

> Values concatenated onto the flag is not something I see a lot? I would expect: `-t go` but that could be just me.

It's standard and idiomatic in UNIX command line since... forever. I don't know precisely when the convention started, but it is [specified by POSIX](https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html), which likely suggests the convention pre-dated POSIX. So it's probably been around for at least 32 years.

And `-t go` works as well.

---

_Comment by @disconnect3d on 2024-03-26 16:44_

@BurntSushi Can we at least extend the "USAGE" displayed when `rg` is invoked with no arguments or with `--help` so that it shows the `--type` flag, like:
```diff
USAGE:
    rg [OPTIONS] PATTERN [PATH ...]
-    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
+    rg [OPTIONS] [--type TYPE ...] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN
```

Ideally, it would be nice to provide an example like `--type markdown` but yeah.

---

_Comment by @BurntSushi on 2024-03-26 16:49_

That's not really what the usage is for. The usage is show the forms of allowable commands, not just prominent flags. Notice how the second form indicates that the only positional arguments are file paths, where as the first form indicates that the first positional argument is a pattern.

---

_Comment by @disconnect3d on 2024-03-26 16:54_

> That's not really what the usage is for. (...)

Yet, the usage/help fails to immediately show/explain to the user how to filter by filepaths/extensions.

I am pretty sure that many many people had the same issue and were annoyed that the usage doesn't show/explain `-g` or `-t`, but oh well.

It would be nice to address this somehow.

EDIT: I mean, sure, its in `--help`, but I still feel its hard to discover it. Random thought: ppl may not search for 'glob' (or know what it does) and instead look for 'filepath', 'file extension' etc.

---

_Comment by @BurntSushi on 2024-03-26 17:02_

> I am pretty sure that many many people had the same issue and were annoyed that the usage doesn't show/explain `-g` or `-t`, but oh well.

There are a _ton_ of things it doesn't show.

The `--help` page is very long, and if you start prioritizing things to fix issues like, then you end up with the opposite problem: things that really do need to be prioritized end up getting de-prioritized. Therefore, your suggestion is not _just_ "please prioritize this important feature," but it's _also_ "please also de-prioritize this other thing at the same time." In other words, a balance must be struct.

The user guide has several prominent sections on filtering. I think that's probably good enough IMO.

---

_Comment by @cheater on 2024-03-26 17:52_

As long as I can google for "ripgrep use type option" and there's something
exactly about what I need in the first 3 hits, I'm fine. Currently nothing
on the front page seems to be relevant. Obvious caveats about search
bubbles notwithstanding.

On Tue, Mar 26, 2024 at 6:03 PM Andrew Gallant ***@***.***>
wrote:

> I am pretty sure that many many people had the same issue and were annoyed
> that the usage doesn't show/explain -g or -t, but oh well.
>
> There are a *ton* of things it doesn't show.
>
> The --help page is very long, and if you start prioritizing things to fix
> issues like, then you end up with the opposite problem: things that really
> do need to be prioritized end up getting de-prioritized. Therefore, your
> suggestion is not *just* "please prioritize this important feature," but
> it's *also* "please also de-prioritize this other thing at the same
> time." In other words, a balance must be struct.
>
> The user guide has several prominent sections on filtering. I think that's
> probably good enough IMO.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/333#issuecomment-2020997509>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABPWPURD2TIR3INRLOXJUTY2GS5RAVCNFSM4C43UCJKU5DIOJSWCZC7NNSXTN2JONZXKZKDN5WW2ZLOOQ5TEMBSGA4TSNZVGA4Q>
> .
> You are receiving this because you commented.Message ID:
> ***@***.***>
>


---

_Comment by @BurntSushi on 2024-03-26 18:03_

The first result for me is the [GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md). And the GUIDE talks extensively about filtering. I don't understand how that isn't relevant. It is literally exactly the thing you would want to see.

---

_Comment by @cheater on 2024-03-26 18:45_

it's the #1 result for me too. the search result snippet doesn't talk about
it. note i said "seems to be relevant"

On Tue, Mar 26, 2024 at 7:03 PM Andrew Gallant ***@***.***>
wrote:

> The first result for me is the GUIDE
> <https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md>. And the
> GUIDE talks extensively about filtering. I don't understand how that isn't
> relevant. It is literally exactly the thing you would want to see.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/333#issuecomment-2021140174>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABPWPSM7IVS3WFLNJZXH33Y2GZ7RAVCNFSM4C43UCJKU5DIOJSWCZC7NNSXTN2JONZXKZKDN5WW2ZLOOQ5TEMBSGEYTIMBRG42A>
> .
> You are receiving this because you commented.Message ID:
> ***@***.***>
>


---

_Comment by @cheater on 2024-03-26 18:45_

not just for that reason alone i'd suggest splitting that massive document
up into smaller ones

On Tue, Mar 26, 2024 at 7:44 PM Damian ***@***.***> wrote:

> it's the #1 result for me too. the search result snippet doesn't talk
> about it. note i said "seems to be relevant"
>
> On Tue, Mar 26, 2024 at 7:03 PM Andrew Gallant ***@***.***>
> wrote:
>
>> The first result for me is the GUIDE
>> <https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md>. And the
>> GUIDE talks extensively about filtering. I don't understand how that isn't
>> relevant. It is literally exactly the thing you would want to see.
>>
>> —
>> Reply to this email directly, view it on GitHub
>> <https://github.com/BurntSushi/ripgrep/issues/333#issuecomment-2021140174>,
>> or unsubscribe
>> <https://github.com/notifications/unsubscribe-auth/AABPWPSM7IVS3WFLNJZXH33Y2GZ7RAVCNFSM4C43UCJKU5DIOJSWCZC7NNSXTN2JONZXKZKDN5WW2ZLOOQ5TEMBSGEYTIMBRG42A>
>> .
>> You are receiving this because you commented.Message ID:
>> ***@***.***>
>>
>


---

_Comment by @BurntSushi on 2024-03-26 18:53_

OK, well I don't control what snippet the search engine shows you.

I'm not splitting the guide into arbitrarily small pieces just so search engine results snippets are better.

The size of the document is large, which is why there is a table of contents. The table of contents is quick to scan and even includes the phrase "file types."

---

_Comment by @cheater on 2024-03-26 18:55_

It's not just that. Long documents are overwhelming to people, especially
ones with reading and focus disabilities, which means most people nowadays.
It's really worthwhile to split this up into small pieces that just talk
about one thing. For most of the things talked about in this document there
isn't a really good reason to have them all in a single document.

On Tue, Mar 26, 2024 at 7:53 PM Andrew Gallant ***@***.***>
wrote:

> OK, well I don't control what snippet the search engine shows you.
>
> I'm not splitting the guide into arbitrarily small pieces just so search
> engine results snippets are better.
>
> The size of the document is large, which is why there is a table of
> contents.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/333#issuecomment-2021236395>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABPWPTAQLL57DXOYVSPFSTY2G72FAVCNFSM4C43UCJKU5DIOJSWCZC7NNSXTN2JONZXKZKDN5WW2ZLOOQ5TEMBSGEZDGNRTHE2Q>
> .
> You are receiving this because you commented.Message ID:
> ***@***.***>
>


---

_Comment by @BurntSushi on 2024-03-26 19:07_

I think we'll have to agree to disagree.

---
