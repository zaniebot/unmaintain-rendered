```yaml
number: 847
title: Respect TERM=dumb and emit no colour escape codes
type: issue
state: closed
author: andreastt
labels:
  - C-enhancement
assignees: []
created_at: 2017-02-09T16:55:44Z
updated_at: 2020-04-11T17:15:55Z
url: https://github.com/clap-rs/clap/issues/847
synced_at: 2026-01-12T16:14:10Z
```

# Respect TERM=dumb and emit no colour escape codes

---

_@andreastt_

Certain shells are not particularly happy with colour escape codes. To make them happy, clap should respect `TERM=dumb` and refrain from emitting colour escape codes.

### Rust Version

rustc 1.15.0 (10893a9a3 2017-01-19)

### Affected Version of clap

    % grep clap Cargo.toml
    clap = "2.19.0"

### Expected Behavior Summary

For a program using clap to respect `TERM=dumb` and emit no colour escape codes.

### Actual Behavior Summary

Program using clap emits colour escape codes as shown in https://github.com/BurntSushi/ripgrep/issues/353.

### Steps to Reproduce the issue

Compile any program using clap as a dependency and use an unrecognised flag:

```
% rg --color never -asd
�[1;31merror:�[0m Found argument '�[33m-d�[0m' which wasn't expected, or isn't valid in this context

USAGE:
    
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try �[32m--help�[0m
```


---

_Referenced in [BurntSushi/ripgrep#353](../../BurntSushi/ripgrep/issues/353.md) on 2017-02-09 16:55_

---

_Comment by @kbknapp on 2017-02-09 17:43_

Thanks! 

Relates to #836 

---

_Label `C: errors` added by @kbknapp on 2017-02-09 17:44_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-09 17:44_

---

_Label `P3: want to have` added by @kbknapp on 2017-02-09 17:44_

---

_Label `T: enhancement` added by @kbknapp on 2017-02-09 17:44_

---

_Label `T: refactor` added by @kbknapp on 2017-02-09 17:44_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-09 17:44_

---

_Added to milestone `2.21.0` by @kbknapp on 2017-02-12 16:27_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:58_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:58_

---

_Comment by @nateozem on 2017-05-15 05:21_

I wonder if someone can reference `TERM=dumb` is standardize across applications. Is there any documentation that explains how it should behave?

---

_Comment by @BurntSushi on 2017-05-15 10:49_

@nateozem This is the best I could find: https://linux.die.net/man/7/term --- TL;DR the `TERM=dumb` convention is quite old. The behavior today is to treat `TERM=dumb` as "don't emit escape sequences because the terminal being used won't know how to use them."

---

_Comment by @nateozem on 2017-05-16 02:22_

@BurntSushi, interesting.
Seem like the behavior would depend on each individual machine. In most probability, everyone has the same capability for `dumb`, but there can be an instance where someone can change how the terminal type behaves by using the `tic` command. 
On my system, I can do this to check the definition: `$ infocmp -L dumb`

	$ infocmp -L dumb
	#       Reconstructed via infocmp from file: /usr/share/terminfo/64/dumb
	dumb|80-column dumb tty,
			auto_right_margin,
			columns#80,
			bell=^G, carriage_return=^M, cursor_down=^J,
			scroll_forward=^J,

My expectation of moving forward is by calling "curses interfaces to terminfo database" (`$ man curs_terminfo(3X)`). What do you guys think?

When I discover anything more, I'll report back. 


---

_Comment by @BurntSushi on 2017-05-16 02:28_

@nateozem Consulting terminfo is certainly one path. It's a complex one, but it's valid. On the other hand, just respecting `TERM=dumb` will solve a good number of problems. Perhaps not all of them, but some of them. :-)

An example of a popular piece of software that generally gets colors correct is GNU grep, but they don't consult terminfo at all. This sacrifices compatibility on some very obscure terminal environments, but standard ANSI escapes pretty much work everywhere. (And GNU grep supports `TERM=dumb` explicitly.)

---

_Comment by @nateozem on 2017-05-16 02:43_

We could do almost the same thing as they have done

http://git.savannah.gnu.org/cgit/grep.git/tree/lib/colorize-posix.c:

	/* Return non-zero if we should highlight matches in output to file
	   descriptor FD.  */
	int
	should_colorize (void)
	{
	  char const *t = getenv ("TERM");
	  return t && strcmp (t, "dumb") != 0;
	}

It's very simple. Think this would work for us?


---

_Removed from milestone `2.21.0` by @kbknapp on 2018-02-02 01:57_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:57_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:28_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:28_

---

_Removed from milestone `v3-alpha.1` by @kbknapp on 2018-07-22 01:07_

---

_Added to milestone `v3-beta.1` by @kbknapp on 2018-07-22 01:07_

---

_Removed from milestone `v3-beta.1` by @pksunkara on 2020-02-01 07:44_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:44_

---

_Assigned to @pksunkara by @pksunkara on 2020-04-11 14:55_

---

_Comment by @pksunkara on 2020-04-11 17:15_

Fixed by #963 

---

_Closed by @pksunkara on 2020-04-11 17:15_

---
