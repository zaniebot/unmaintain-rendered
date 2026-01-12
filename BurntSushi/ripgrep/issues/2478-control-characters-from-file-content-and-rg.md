```yaml
number: 2478
title: Control characters from file content and rg (colors) get merged
type: issue
state: closed
author: Korkman
labels:
  - wontfix
assignees: []
created_at: 2023-03-26T13:42:13Z
updated_at: 2023-11-22T21:45:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2478
synced_at: 2026-01-12T16:13:24Z
```

# Control characters from file content and rg (colors) get merged

---

_@Korkman_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Binary download from releases.

#### What operating system are you using ripgrep on?

Debian Bullseye

#### Describe your bug.

Ripgrep does not sanitize file match content before showing it in annotated output (colorized lines and match, which is the default mode of operation). While I agree matched lines should be passed unmodified as-is when output is piped to a program, it is very unconvenient up to the point of being dangerous when passed to the terminal. The problem is that the output cannot be easily handled in a safe manner because it contains a mixture of control characters from the grepped file and ripgrep. I propose ripgrep should strip all control characters (and unprintable, while at it) from the match column before applying colors, whenever a terminal is attached to the output.

#### What are the steps to reproduce the behavior?

Attached is a file
[controlchars.txt](https://github.com/BurntSushi/ripgrep/files/11071721/controlchars.txt)
which will hide a line when grepped with

`rg "" controlchars.txt`

#### What is the actual behavior?

Sabotaged output:
```
1:one
2:three
```

#### What is the expected behavior?

Sanitized output:
```
1:one
2:two
3:three
```


---

_Comment by @BurntSushi on 2023-03-26 13:51_

I'm not sure how practical this is, and I doubt I'll work on it any time soon. I'm also skeptical of your characterization of it being dangerous. Is `cat file` dangerous? How many tools dump the contents of an arbitrary file without sanitizing it?

I'd be open to considering a PR fixing this, but I don't think it will be a simple change.

---

_Label `enhancement` added by @BurntSushi on 2023-03-26 13:51_

---

_Label `question` added by @BurntSushi on 2023-03-26 13:51_

---

_Comment by @Korkman on 2023-03-26 16:31_

Yes! `cat file` is indeed dangerous if the passed file is of untrusted origin. Not beyond a mild annoyance on its own, but combined with flaws in terminal emulators it [can lead to RCE](https://seclists.org/fulldisclosure/2021/May/33). I can think of many use-cases where rg is fed untrusted files, like logfiles or whole filesystems. The beauty of ripgrep is that it is insanely fast reading files. Absolutely feasible to just go to / and `rg --one-file-system`.

Currently, input files can be sanitized using a `--pre` processor, but that greatly impacts speed and distorts reported match locations. Also, it is unnecessary to sanitize the entire input. Output can be sanitized, too, with some `awk` magic, but not (easily) without losing colored matches (and at that point, there is no way to tell if colors came from ripgrep or the grepped file).

The perfect place to sanitize would be right between "found a matching line" and "output the match", and that's inside ripgrep ;-) I can totally understand this isn't a priority, though. Since you're open for a PR, I might give it a try, but not very soon either. I guess a new option `--safe-output` would be best, removing all non-printable characters from output?





---

_Comment by @misaki-web on 2023-03-29 20:03_

> The perfect place to sanitize would be right between "found a matching line" and "output the match", and that's inside ripgrep ;-) I can totally understand this isn't a priority, though. Since you're open for a PR, I might give it a try, but not very soon either. I guess a new option `--safe-output` would be best, removing all non-printable characters from output?

IMO, a flag `--safe-output` may cause some confusion. One could expect to get output with literal control chars/escape sequences, like this:

```bash
$ cat controlchars.txt | sed $'s/\e/\\\\E/g'
one
two
\E[1A\E[999D\E[2C\E[Kthree
```

Something like `--strip-xxxxx` may be clearer, but I'm not sure what would be the best expression (`xxxxx`) because we shouldn't strip all control characters.

> I propose ripgrep should strip all control characters (and unprintable, while at it) from the match column before applying colors, whenever a terminal is attached to the output.

It may be too much. See this example:

```bash
$ # By default:
$ echo -e "lorem\tipsum" | rg 'lorem'
lorem	ipsum
$ # Stripping all control characters:
$ echo -e "lorem\tipsum" | rg 'lorem' | tr -d '[:cntrl:]'
loremipsum
```


---

_Comment by @Korkman on 2023-04-06 21:27_

You're right, the full list of unprintables is too much of a filter. Also, terminal detection downpipe was a bad idea, strike that. If it's an option, it should always behave the same.

List of unprintable ASCII codes for reference:
```
dec hex  name
  0  00  NULL
  1  01  START OF HEADING
  2  02  START OF TEXT
  3  03  END OF TEXT
  4  04  END OF TRANSMISSION
  5  05  ENQUIRY
  6  06  ACKNOWLEDGE
  7  07  BELL (Beep)
  8  08  BACKSPACE
  9  09  HORIZONTAL TAB
 10  0A  LINE FEED
 11  0B  VERTICAL TAB
 12  0C  FORM FEED
 13  0D  CARRIAGE RETURN
 14  0E  SHIFT OUT
 15  0F  SHIFT IN
 16  10  DATA LINK ESCAPE
 17  11  DEVICE CONTROL 1 (XON)
 18  12  DEVICE CONTROL 2
 19  13  DEVICE CONTROL 3 (XOFF)
 20  14  DEVICE CONTROL 4
 21  15  NEGATIVE ACKNOWLEDGE
 22  16  SYNCHRONOUS IDLE
 23  17  END OF TRANSMISSION BLOCK
 24  18  CANCEL
 25  19  END OF MEDIUM
 26  1A  SUBSTITUTE
 27  1B  ESCAPE
 28  1C  FILE SEPARATOR
 29  1D  GROUP SEPARATOR
 30  1E  RECORD SEPARATOR
 31  1F  UNIT SEPARATOR
127  7F  RUBOUT (DELETE)
```

From those I can think of a whitelist of characters with known good effects:
* LINE FEED _if matched, it is probably intended to be output as well_
* CARRIAGE RETURN _keep windows linefeeds alive_
* HORIZONTAL TAB

And a blacklist of characters with likely unintended effects:
* BACKSPACE
* ESCAPE
* RUBOUT (DELETE) _I actually don't know if this one is interpreted by any terminal_
* BELL (Beep)
* NULL
_and from this [helpful comment](https://serverfault.com/a/189551/38826):_
* SHIFT OUT
* SHIFT IN
* DEVICE CONTROL 3 (XOFF)


The blacklist may be extended with all characters of no known good effects (not displayed = not useful, can only do harm).

It's quite difficult to make either a complete whitelist or blacklist fit for all purposes and use-cases.

After all, maybe a minimalist approach is best:

`--strip-escape`

Everything else can be filtered downpipe externally with other tools. But rg _adds_ escape sequences for colors and those use only ESCAPE afaik. So the one character which has to be stripped at minimum would be ESC?

In addition, a second option for convenience could be

`--strip-uncommon`

which would apply the above whitelist (easiest and safest route).

Also worth noting is that filtering any of these is not conflicting with UTF-8 because UTF-8 is 128+ for all its bytes. Other encodings, most notable UTF-16, have to be researched. How are they printed to the terminal? Are control characters passed through in other encodings at all? How to deal with them?


---

_Comment by @misaki-web on 2023-04-06 22:11_

> The perfect place to sanitize would be right between "found a matching line" and "output the match", and that's inside ripgrep

How do you see a new flag `--strip-xxxxx` interact with `--passthru`? Only stripping chars in matched lines or in the entire document (since `--passthru` returns all lines, matched or not)?

---

_Comment by @Korkman on 2023-04-06 23:49_

Since the core use-case here is "I want only trusted escape sequences downpipe" it should be applied to all content sourced from matching files.

(also: ```--context```)

---

_Comment by @MarSoft on 2023-10-15 13:55_

I think we should not *strip* escape sequences but *replace* them with a safe form, like `less` does by default:
```
# by default — some words will be colorized
$ echo -e 'lorem\tipsum \e[31mdolor \e[32msit amet\e[0m' | rg 'lorem'
lorem   ipsum dolor sit amet
# with escape characters guarding enable
$ echo -e 'lorem\tipsum \e[31mdolor \e[32msit amet\e[0m' | rg --guard-escapes 'lorem'
lorem   ipsum \x1B31mdolor \x1B32msit amet\x1B0m
```
We may also want to colorize those escape sequences (like `\x1B`), maybe just with "bold" effect.

---

_Comment by @BurntSushi on 2023-11-22 21:45_

I've given this some thought and I think this is beyond the scope of what ripgrep ought to do.

---

_Closed by @BurntSushi on 2023-11-22 21:45_

---

_Label `enhancement` removed by @BurntSushi on 2023-11-22 21:45_

---

_Label `question` removed by @BurntSushi on 2023-11-22 21:45_

---

_Label `wontfix` added by @BurntSushi on 2023-11-22 21:45_

---
