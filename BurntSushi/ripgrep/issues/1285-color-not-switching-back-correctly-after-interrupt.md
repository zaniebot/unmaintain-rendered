```yaml
number: 1285
title: Color not switching back correctly after interrupt
type: issue
state: closed
author: sturdevant
labels:
  - duplicate
  - wontfix
assignees: []
created_at: 2019-05-25T05:10:07Z
updated_at: 2019-05-29T17:38:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1285
synced_at: 2026-01-12T16:13:23Z
```

# Color not switching back correctly after interrupt

---

_@sturdevant_

#### What version of ripgrep are you using?
ripgrep 11.0.1

#### How did you install ripgrep?
installed with Cargo install

#### What operating system are you using ripgrep on?
Ubuntu 19.04

#### Describe your question, feature request, or bug.
Minor cosmetic bug, when I Ctrl-C (or Ctrl-\\) in the middle of ripgrep while it's printing text in red (likely other colors too, but I've only been able to reliably reproduce it in red), the terminal that I ran ripgrep in will only print text in red from then on (unless I close the terminal or run `reset`). I suspect that before ripgrep outputs text in red, it's changing some environment variable to change the font color to red, then it prints everything that it would print in red, then it sets the color back, etc. It seems to handle things properly when the process is left to terminate on its own, but when it terminates prematurely, say from a SIGINT or SIGQUIT, it isn't being handled correctly, so there should be some cleanup code in the SIGINT and SIGQUIT handlers that resets the color back to what it originally was before running ripgrep.

#### If this is a bug, what are the steps to reproduce the behavior?
If you run something like:

`ripgrep -r "blah" .`

from a directory that has a lot of files / subdirectories with a lot of files that ripgrep won't skip over (in my case, from my home directory which is about 50GB in size when including size of all subdirectories, would likely work with a smaller size too, but it at least has to be big enough so ripgrep doesn't finish too fast), then while it's printing a bunch of red text, try to time the Ctrl-C to be sent in the middle of it printing a line in red (as in, if it were left to keep running w/o the SIGINT, it would have continued to print more in red). The timing can be tricky, since if it's not in the middle of it printing in red (as in the color has been reset after a line is printed), then the bug won't appear. I've found it's very easy to reproduce if you change "blah" to a veeeeerrryyy ridiculously long string, so feel free to try that if it helps. (somewhat amusing note, I found this bug by trying to use ripgrep like it was grep, but I didn't realize the '-r' option was for replacing the text of matches rather than recursive search, so at first I was very confused why I had **so** many files that seemed to match, and thought there was a more serious bug, but then I decided to actually run with '--help', and when I read the description for what '-r' actually does, everything made a *lot* more sense, lol, *everything* matched because ***I told it to***).

#### If this is a bug, what is the actual behavior?

Everything is in red text, I don't think the actual contents are so much important so kind of N/A, so I haven't included a full printout, but if you're curious, it's a lot of red blah's, then a red "^C", then red prompt, then red everything, there's just soooo much RED everywhere!!! I thought my terminal was bleeding.

#### If this is a bug, what is the expected behavior?
To have the terminal's text color reset to what it originally was before running ripgrep after ripgrep receives a signal.

What do you think ripgrep should have done?
Handle SIGINT (and SIGQUIT, SIGTERM, and SIGTSTP, might be others, but I didn't test any of the more obscure ones like SIGHUP or any ones that you obviously can't do anything about like SIGKILL) by resetting back to the terminal's default text color before exiting (in the case for my terminal, black text).

---

_Comment by @BurntSushi on 2019-05-25 08:08_

This is a duplicate. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#how-do-i-stop-ripgrep-from-messing-up-colors-when-i-kill-it

---

_Closed by @BurntSushi on 2019-05-25 08:08_

---

_Label `duplicate` added by @BurntSushi on 2019-05-29 17:38_

---

_Label `wontfix` added by @BurntSushi on 2019-05-29 17:38_

---
