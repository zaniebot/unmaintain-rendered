```yaml
number: 1068
title: Command line is too verbose
type: issue
state: closed
author: sabi0
labels:
  - question
assignees: []
created_at: 2018-09-26T12:46:26Z
updated_at: 2018-11-30T15:05:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1068
synced_at: 2026-01-12T16:13:22Z
```

# Command line is too verbose

---

_@sabi0_

This is a kind of meta-issue. And a subjective one. So feel free to close it.

I wanted to bring the attention to the problem of having to write too much in the command line.
For instance, the following ripgrep command:
`ripgrep "Failed\s+(.*)" -o --replace $1 --no-filename -g luu*.2018-09-2?`
is equivalent to this pcre2grep one:
`pcre2grep "Failed\s+(.*)" -o1 -h luu*.2018-09-2?`

I understand that ripgrep tries to keep command line compatibility with grep and other searchers as much as possible.
But we are all lazy :-) So if you could find some ways to implement the "shortcuts" to make the options easier to use (without breaking compatibility) that would be just awesome.

For instance, how about automatically considering the filename as a 'glob' in case 1) the 'verbatim' file does not exist and 2) the name contains "glob symbols": ? and / or *. This would essentially eliminate the need for `-g` option in most cases.

Another example: `-o --replace $1` could be effectively combined into something like `-o1` or `-o $1`.

---

_Comment by @BurntSushi on 2018-09-26 12:58_

Sorry, but I don't think you've actually written a minimal command here. However, I am not a frequent user of `pcre2grep`, so I am not certain. In particular, I imagine that this would work:

```
$ rg "Failed\s+(.*)" -or $1 --no-filename luu*.2018-09-2?
```

If you want something more succinct, then I don't understand why you are spelling out `--replace` instead of using its short option `-r`.

Adding an argument to `-o/--only-matching` doesn't make sense in my opinion. It is much better expressed through the orthogonal concept of `-r/--replace`.

I don't understand why you are OK with using standard shell globbing with `pcre2grep`, but not with ripgrep. ripgrep accepts a list of file paths just like any other grep-like tool.

You do not have single quotes around your `-g` argument, which means it is probably not being interpreted in a way that you expect.

---

_Label `question` added by @BurntSushi on 2018-09-26 12:58_

---

_Comment by @sabi0 on 2018-09-26 13:30_

You are right, the command examples I provided are not the most succinct.
And indeed, when abbreviated to `-or $1` it does not look that bad anymore.
I could also live with "orthogonal" `-o` and `-r` (and even use that to benefit in some cases).

I do miss the shorthand for `--no-filename` though as I use this option all the time.

As for the globs I should have mentioned that I am using ripgrep on Windows - there's no "shell globbing". All I get without `-g` is: "The filename, directory name, or volume label syntax is incorrect. (os error 123)".

---

_Comment by @BurntSushi on 2018-09-26 13:41_

> As for the globs I should have mentioned that I am using ripgrep on Windows - there's no "shell globbing". All I get without -g is: "The filename, directory name, or volume label syntax is incorrect. (os error 123)".

Yes. This is why the issue template asks you what OS you're on. :-) This is a known bug: #234. Your input on that issue (not this one) would be appreciated. For example, one thing I'm uncertain about is whether ripgrep should use the standard Windows globbing APIs, or whether it should just use normal Unix-style globbing.

> I do miss the shorthand for --no-filename though as I use this option all the time.

It's a niche option, so it's not going to get a short flag, sorry. It's niche because `--no-filename` is the default already in the most common scenario that you would want it: when searching a single file or a stream. But otherwise, you almost always want to see _which_ file matched when searching many of them. There are very few short flags left that are available, and I don't see myself using up one of those for `--no-filename`.

---

_Closed by @BurntSushi on 2018-09-26 13:41_

---

_Comment by @sabi0 on 2018-09-27 08:31_

I would argue that when `-r` is used the user is trying to "produce some output" and not "find" where the matches are. So the filenames should not be printed by default.

---

_Comment by @cebaa on 2018-11-28 20:13_

@sabi0 To be frank, `grep` defaults to printing names as well when supplying multiple files (i.e. `grep needle haystack1.log haystack2.log` will print matches with `haystackN.log:` prefix by default).

@BurntSushi My 2c - though I agree on scarcity of short flags, I think `-h` being a shorthand for `--help` might be less useful than using it as a short for `--no-filename`, especially for `grep`-converts. Obviously your project => your choice and I respect that, but do you see perhaps doing that switch as a compromise?

This mostly on the grounds that invoking `rg` without any arguments prints:

```
For more information try --help
```

and doesn't print either of the following:

```
For more information try -h
For more information try -h / --help
```

I use `--no-filename` a lot, so it's not a niche option in general I guess.

---

_Comment by @BurntSushi on 2018-11-28 22:03_

@cebaa Thanks for the thoughtful response! One possible work-around to your problem is to setup an alias like `alias rgh="rg --no-filename"`.

> I use `--no-filename` a lot, so it's not a niche option in general I guess.

When I call a feature or idea "niche," that doesn't mean it isn't useful or is unimportant. It just means that, _in my judgment_, it doesn't have broad appeal. My judgment _could be_ stupendously wrong, but part of the input to that judgment is the fact that I've been on the Internet for a very long time, and people are most vocal when they disagree rather than agree. (There is a saying that the quickest way to find an answer to a question is to loudly proclaim an answer you know to be wrong; folks will jump over themselves to correct you.) Therefore, when I call something "niche," it has the interesting effect of folks announcing that they use that feature all the time and then concluding that, therefore, it is not niche. But, as I'm sure you can appreciate, that isn't necessarily true. The inputs that go into my judgment are incredibly biased and flawed in a lot of different ways, and this is why I've reversed several "decisions" I've made in the past. So far, so good.

At the end of the day, we have scarce resources and someone needs to make a judgment call about how to spend them. Your idea about re-purposing the `-h` flag is interesting, but I don't think I'm going to pursue it. In particular, ripgrep's `-h` and `--help` flags have intentionally different behavior. The former shows a condensed help output where as the latter is much more verbose. That both flags aren't advertised in the output of `rg` without any arguments is not necessarily an acknowledgment on my part that `-h` isn't helpful. It would actually be better if it mentioned both `-h` and `--help`. Even if they didn't have different behavior, I still would not like to re-purpose the `-h` flag because, in my opinion, that `-h` shows help output is a very strong convention in command line tools, and I'd rather not break it.

---

_Comment by @sabi0 on 2018-11-29 15:51_

FYI: I ended up creating `rgh.bat` in one of the folders on PATH:
```
@rg.exe --no-filename --no-line-number --crlf %*
```

It should be even easier on Linux with aliases.

---

_Comment by @cebaa on 2018-11-30 15:05_

@BurntSushi 

> Thanks for the thoughtful response! 

I should be thanking you for that! Aside from the great work on the software itself, you're awesome at communication, kudos for that and keep up the good work.

> One possible work-around to your problem is to setup an alias like alias rgh="rg --no-filename".

Yeah, I know, I just try not to do that, for the following reason: when I go to another machine and especially *another person's* machine, as soon as I start typing and being greeted with "command not found" they start wondering if calling me to help them troubleshoot things was really a wise idea :)

In other words, once you start customizing, you end up customizing everything and then you're an island on your own. The impedance mismatch comes with a price IMHO.

> In particular, ripgrep's -h and --help flags have intentionally different behavior.

Ah, I missed that part. Fair enough. Actually, this also looks like a standard behavior across other CLIs, so `rg` is on the right side there.

> That both flags aren't advertised in the output of rg without any arguments is not necessarily an acknowledgment on my part that -h isn't helpful.

Just to clarify, I meant that more as: `--help` is already advertised, so if someone gets stuck, they will be able to get unstuck by using `--help`, without needing to know about `-h`.

> -h shows help output is a very strong convention in command line tools, and I'd rather not break it.

Yeah, I guess `grep` is actually an offender here. Many other CLIs use `-h` for that purpose.

@sabi0 Yeah I know, see the above for the rationale against.

---
