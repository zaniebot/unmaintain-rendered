```yaml
number: 1709
title: Blank lines to delineate different files should not be emitted when using -NI flags
type: issue
state: closed
author: sheisnicola
labels:
  - doc
  - rollup
assignees: []
created_at: 2020-10-19T11:05:34Z
updated_at: 2023-11-25T20:04:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1709
synced_at: 2026-01-12T16:13:24Z
```

# Blank lines to delineate different files should not be emitted when using -NI flags

---

_@sheisnicola_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Homebrew.

#### What operating system are you using ripgrep on?

```
Darwin nicolaw-aframe 19.6.0 Darwin Kernel Version 19.6.0: Mon Aug 31 22:12:52 PDT 2020; root:xnu-6153.141.2~1/RELEASE_X86_64 x86_64
```

#### Describe your bug.

Blank lines are still printed between matching when using `-NI` flags to surppress the printing of the filename and line numbers. If filenames and numbers are being suppressed, then the blank line to separate each file has no meaning or context so I would not expect them to be printed.

#### What are the steps to reproduce the behavior?

Assuming that there are multiple files in your `/etc/` filesystem that contain the word `host`, then I would not expect any blank lines to be emitted from the following command; I would only expect to see lines that contain the word `host` emitted:

```
rg -NI host /etc
```

#### What is the actual behavior?

Blank lines are printed to delineate the results from one matching file to the next.

#### What is the expected behavior?

No blank lines should be emitted.

---

_Label `bug` added by @BurntSushi on 2020-10-19 18:32_

---

_Comment by @BurntSushi on 2020-10-19 18:46_

So I actually wrote out a bug fix for this, but now I'm second guessing whether this is really a bug or not. It just seems weird to me to disable headings in this particular case, but leave them present when `--no-filename` is given but line numbers are displayed. I guess in my view, having that gap between different files matching might actually be desirable? Or at least, if this bug were fixed, having that gap would be impossible. But today, you can remove the heading with the `--no-heading` flag (or by redirecting ripgrep's output away from a tty).

I'm inclined to leave ripgrep's behavior as is.

---

_Label `question` added by @BurntSushi on 2020-10-19 18:46_

---

_Comment by @sheisnicola on 2020-10-19 18:53_

That's a very fair point. Perhaps this is a command line help/documentation fix then.

As it turns out my use-case does involve pumping this into a pipe, but I immediately pushed it into another `grep` and `sort -u` to "fixup" the output that I first saw come out of `rg`. I didn't realise change in formatting in such a situation.

I suspect that many people will have `rg` recommended to them by a friend or colleague like myself when performing ad-hoc command line data-mining for various purposes, and thus will start out crafting an initial `rg` command line like I did for its speed, and then iterate to pump it into some loop and might make the same presumptions that I did.

Apologies if this wasted your time.

Thank you for the solution!

---

_Comment by @BurntSushi on 2020-10-19 18:58_

I see, yes. People are sometimes caught off guard by the change in formatting depending on whether ripgrep is printing to a tty or not. But even core commands do this occasionally. Compare the output of `ls` and `ls | cat` for example. It's subtle!

I'll switch this over to a documentation bug, although it's a little tricky. Maybe the tty style behavior can be mentioned more prominently in the docs. But I think we're getting close to a point where there are a few things being mentioned prominently, which of course weakens the notion of "prominent." :-/

---

_Label `bug` removed by @BurntSushi on 2020-10-19 18:58_

---

_Label `doc` added by @BurntSushi on 2020-10-19 18:58_

---

_Comment by @sheisnicola on 2020-10-19 19:05_

_nods_

Absolutely. Can't fault you there!

---

_Comment by @murlakatamenka on 2021-01-06 15:15_

Wow, finding that issue wasn't easy, but that's exactly the problem I encountered.

My situation: I extract 1 field from multiple json-like files. I only need the value, so I use replace like this:

```sh
rg --no-filename --no-line-number --color=never --max-count=1 '"name"\s+"(.*)"' "*.acf" --replace '$1'
```
And then I get values separated with empty lines that I don't need.

It wasn't trivial to find what caused it. I tried `--no-context-separator`, `--null`, `--null-data` flags, searched through discussions and issues here until I found this one.

So the solution is to use `--no-heading` flag and it appears that empty line was an empty heading. Not complaining, but that wasn't so obvious to figure it all out :)

---

_Comment by @BurntSushi on 2021-01-06 16:22_

@murlakatamenka Can you suggest a way to make it easier to figure out? Other than changing ripgrep's behavior (which as explained above is perhaps not such a good idea), what would have reduced the amount of time you spent tracking down the problem?

---

_Comment by @murlakatamenka on 2021-01-11 09:25_

Ah, yes, I should've been more constructive, sorry for that.

The problem is that I failed to find out how to change it, so it's discoverability thing. You see above I tried not-so-relevant flags. I did check tab-completion to `rg --no-<TAB>`. I also checked man page / issues for following keywords:

- separator
- delimiter
- blank / empty line
- multiline
- multiple (meaning multiple files)

all until I found this issue and that `a-ha!` moment: blank line belongs to the heading, `--no-heading` flag is what was looked for.

I think possible solution would be add more info to help / man page.

> --heading
           This flag prints the file path above clusters of matches from each file instead of printing the file path
           as a prefix for each matched line. This is the default mode when printing to a terminal.

Like here we don't have any mention that the heading includes that blank line - separator of clusters of matches.

---

I would say steps to reproduce the issue are:

1. I'd like to extract some data from files
2. `rg <pattern>`, check results in the output
3. no need for line numbers -> `--no-line-numbers`, cool
4. don't need filenames -> `--no-filename`, obviously
5. no blank lines, please -> `???`, user is stuck

---

I mostly agree with what @sheisnicola wrote:

> If filenames and numbers are being suppressed, then the blank line to separate each file has no meaning or context so I would not expect them to be printed.

In our use cases `-NI` meant "hey, `rg`, I don't care about _source / location_ of the data, fetch me just it" and those blank lines weren't desirable. Guess there can be situations where you don't need this metadata but still want the clustering of matches.

---

So for now `--heading` help / man page can be improved. Maybe some notice of how you control output when searching through multiple files? Otherwise Interactive TAB-completion is usually enough for me to discover ripgerp's options and flags.

---

_Label `question` removed by @BurntSushi on 2021-01-11 13:49_

---

_Comment by @markkrj on 2022-02-04 18:53_

Also, when showing colors and line numbers, there is no point in having a line break between files... It is clear enough to what file a result belongs. You can see with: `rg my_search --heading --pretty | sed '/^$/d'`


---

_Comment by @murlakatamenka on 2022-02-11 15:26_

@markkrj I like empty lines between results per file, for example, so this is personal preference

---

_Comment by @ltrzesniewski on 2022-08-16 16:51_

Since this issue is still open, I thought I'd add my 2 cents. 🙂 

I was also surprised by the default behavior when I asked ripgrep to "just show me the raw matches" by adding `-oNI`.

> It just seems weird to me to disable headings in this particular case, but leave them present when `--no-filename` is given but line numbers are displayed.

Actually, I was also surprised that line numbers were displayed by default when file names weren't. Line numbers don't seem to be very useful out of context (without a file name).

I guess what I really expected is for `-I` to be the equivalent of `--no-filename --no-line-number --no-heading`, but I suppose this would be too big of a change now. Anyway, having one convenient option for this would have been nice.

> Or at least, if this bug were fixed, having that gap would be impossible.

Wouldn't the `--heading` option enable this?



---

_Comment by @BurntSushi on 2022-08-16 18:20_

> Actually, I was also surprised that line numbers were displayed by default when file names weren't. Line numbers don't seem to be very useful out of context (without a file name).

This isn't true. If file names aren't shown, then it's because the file name is unambiguous. That is, only one file was searched. For example:

```
$ rg WARRANTY UNLICENSE LICENSE-MIT
LICENSE-MIT
15:THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR

UNLICENSE
16:THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,

$ rg WARRANTY UNLICENSE
16:THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
```

> I guess what I really expected is for `-I` to be the equivalent of `--no-filename --no-line-number --no-heading`, but I suppose this would be too big of a change now. Anyway, having one convenient option for this would have been nice.

Indeed, too big of a change and not one that makes sense to me personally. I'm not even sure what such an option would be called. I'd suggest creating an alias or a wrapper script for it.

> > Or at least, if this bug were fixed, having that gap would be impossible.
> 
> Wouldn't the `--heading` option enable this?

No? The issue here is precisely that the gap exists when `--heading` is set. And `--heading` is set by default when printing to a tty.

In fact, when doing a recursive search, both `--heading` and `--line-number` are _only_ set when printing to a tty. That's because ripgrep is assuming that when printing to a tty, you want to see "human friendly" output. But you can squash that pretty easily by piping the output of ripgrep into `cat`. For example, the following two commands are equivalent:

```
$ rg -IN --no-filename --color never host /etc
```

and

```
$ rg -I host /etc | cat
```

Because when you aren't printing to a tty, ripgrep simply doesn't enable `--heading`, `--line-number` or `--color auto`. This matches grep's behavior when not printing to a tty. The only difference between ripgrep and grep here is that grep doesn't take as many liberties with the output when printing to a tty.

Given that I still feel the same way that I did back when I first looked at this issue, I don't think I'm going to take any action here. So I'm going to close this.

---

_Closed by @BurntSushi on 2022-08-16 18:20_

---

_Closed by @BurntSushi on 2022-08-16 18:20_

---

_Comment by @BurntSushi on 2022-08-16 18:21_

Sorry, I forgot I marked this as a doc bug. So the action here is to document this somewhere in the man page and/or GUIDE. And that hasn't been done yet. But otherwise, I have no plans to add new flags or change how ripgrep behaves today.

---

_Reopened by @BurntSushi on 2022-08-16 18:21_

---

_Comment by @ltrzesniewski on 2022-08-16 18:38_

That makes sense, thanks for the explanation! 🙂 

---

_Label `rollup` added by @BurntSushi on 2023-11-25 19:02_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
