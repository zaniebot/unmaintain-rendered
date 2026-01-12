```yaml
number: 491
title: Should override with last given value when incompatible options are used
type: issue
state: closed
author: ericbn
labels:
  - question
assignees: []
created_at: 2017-05-25T22:46:08Z
updated_at: 2017-08-04T19:55:44Z
url: https://github.com/BurntSushi/ripgrep/issues/491
synced_at: 2026-01-12T16:13:22Z
```

# Should override with last given value when incompatible options are used

---

_@ericbn_

That I can think of, is does not make sense to have

* `--vimgrep` and `--count`
* `--vimgrep` and `--heading`
* `--vimgrep` and `--pretty`

both at the same time. As vimgrep is trying to ouput

    filename:linenumber:columnnumber:text

and the other options conflict with this. It would not make sense to have vimgrep with `--no-line-number` or `--no-filename` either, but these would just generate "useless" output...

@BurntSushi, do you agree rg should fail if conflicting arguments are passed?

---

_Comment by @ericbn on 2017-05-25 23:18_

Other conflicting options are

* (`--heading` or `--pretty`) and `--no-heading`
* (`--line-number` or `--column` or `--pretty`) and (`--no-line-number` or `--count`)
* `--no-filename` and `--with-filename`
* `--no-mmap` and `--mmap`
* `--ignore-case` and `--case-sensitive` and `--smart-case` (an "argument group" here)

---

_Comment by @BurntSushi on 2017-05-26 10:57_

We need to tread carefully here. We can't just say "these flags conflict and therefore produce an error if both are provided." A more measured strategy is required IMO. We might want to say that between `--foo` and `--bar` (for example), the one specified most recently overrides all previous uses of `--foo` and `--bar`. This is, for example, useful if someone is using an alias to setup a default set of flags. It will also become relevant if/when ripgrep gets a config file some day, since one might expect the config file to be a set of flags used by default by the user. For that to work well, flags need to be overridable.

I believe clap supports flags that mutually override each other, although I can't remember the precise semantics off the top of my head.

---

_Comment by @BurntSushi on 2017-05-26 11:00_

Your initial examples with `--vimgrep` might qualify as flags that really do conflict with each other since the mode that `--vimgrep` implies doesn't really make sense with uses of some flags. Other examples, though, like `--no-mmap` and `--mmap` seem more like "one overrides the other" rather than "one conflicts with the other."

With all that said, I'm somewhat skeptical of forcing a hard error on conflict, and instead try to define sensible semantics for each combination. The former gives the end user less flexibility, and in my experience, with the number of flags that ripgrep supports, end users can be *intensely* creative in the types of flag combinations they come up with. It would be a shame to start a crusade to decrease the ability to be creative.

Thoughts?

---

_Comment by @ericbn on 2017-05-26 14:52_

I haven't thought about overriding. Sure, is more flexible and gives a better user experience. This is from the [clap README](https://github.com/kbknapp/clap-rs/blob/master/README.md#features):

> **POSIX Compatible Conflicts/Overrides** - In POSIX args can be conflicting, but not fail parsing because whichever arg comes last "wins" so to speak. This allows things such as aliases (i.e. `alias ls='ls -l'` but then using `ls -C` in your terminal which ends up passing `ls -l -C` as the final arguments. Since `-l` and `-C` aren't compatible, this effectively runs `ls -C` in clap if you choose...clap also supports hard conflicts that fail parsing).

---

_Label `question` added by @BurntSushi on 2017-05-30 11:39_

---

_Renamed from "Should fail when incompatible options are used" to "Should override with last given value when incompatible options are used" by @ericbn on 2017-06-01 00:52_

---

_Comment by @ericbn on 2017-06-02 00:00_

We're not completely able to override last options with previous ones as clap does not allow that in some cases, as reported in kbknapp/clap-rs#970 and kbknapp/clap-rs#976. Some minimal changes in this direction, using what we can get from clap, are in #499.

I was experimenting with [rust-argparse](https://github.com/tailhook/rust-argparse) and it looks like a much simpler, yet more flexible solution. It's API is more similar to the [C POSIX implementation](http://pubs.opengroup.org/onlinepubs/9699919799/functions/getopt.html) in the sense that variables are set in a loop, so last assignments "win". Also, no bells and whistles like [color output](https://github.com/BurntSushi/ripgrep/issues/353). On the other hand: [choices](https://github.com/tailhook/rust-argparse/issues/42), and [required](https://github.com/tailhook/rust-argparse/issues/23) and conflicting arguments would need to be handled manually with rust-argparse.

---

_Comment by @BurntSushi on 2017-06-02 00:40_

@ericbn Thanks for looking into this. I've only briefly glanced at the clap issues. I will say this though: I really do not want to switch argv parsers.

Note that I don't particularly care that much about POSIX per se, but care more about just ironing out a decent user experience. So if there are corner cases that aren't strictly compatible with POSIX but don't have a big impact on actual uses, then I'm fine with that. (And of course, I understand that "POSIX compatibility" can be treated as an approximation to good UX because it is familiar.)

---

_Comment by @ericbn on 2017-06-02 01:19_

@BurntSushi, sounds good. I don't care about "POSIX compatibility" either, I just think the "last overrides previous" and other characteristics present these make sense for a good UX.

I'll close this and hope we can see improvements in clap and make use of them in the future...

---

_Closed by @ericbn on 2017-06-02 01:19_

---

_Comment by @ericbn on 2017-08-04 19:42_

Just to follow up on the general argv parser discussion, `exa` implemented their own parser instead of using clap-rs, as per https://github.com/ogham/exa/issues/152#issuecomment-320224452, because "clap-rs doesn't have any way to allow later arguments to override earlier ones"

---

_Comment by @BurntSushi on 2017-08-04 19:55_

@ericbn Thanks! That's a useful data point. ripgrep will definitely not grow its own argv parser though. :-)

---
