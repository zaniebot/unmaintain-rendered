```yaml
number: 2881
title: ripgrep not shows the unicode characters whereas grep does
type: issue
state: closed
author: ciscohack
labels:
  - invalid
assignees: []
created_at: 2024-09-05T15:32:58Z
updated_at: 2024-09-06T19:26:26Z
url: https://github.com/BurntSushi/ripgrep/issues/2881
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep not shows the unicode characters whereas grep does

---

_@ciscohack_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0


### How did you install ripgrep?

brew command 

### What operating system are you using ripgrep on?

macOS 14.6

### Describe your bug.

while doing ls | grep <filename> it shows the unicode symbol for the file but if i do ls | rg <filename> ripgrep failed and print like this 

<img width="207" alt="image" src="https://github.com/user-attachments/assets/84dc7d44-fe87-4240-b00c-2cff3f7c8d9f">

here is grep output 

<img width="129" alt="image" src="https://github.com/user-attachments/assets/8e34c958-22b6-4347-98f1-dccf012f1936">


it's happening with all filetype

### What are the steps to reproduce the behavior?

steps is simple ls | rg <filename> 

### What is the actual behavior?

actually behaviour is it should show file icon as grep is doing

### What is the expected behavior?

actually behaviour is it should show file icon as grep is doing

---

_Comment by @BurntSushi on 2024-09-05 16:05_

[Help me help you.](https://www.youtube.com/watch?v=l1B1_jQnlFk) Don't tell, [_show_](https://en.wikipedia.org/wiki/Minimal_reproducible_example). Here's [how to do it](https://stackoverflow.com/help/minimal-reproducible-example).

So for example, in this case, you should include commands to create a file with a name equivalent to what you're showing in the image.

You should also show the output of `alias ls`, `which ls`, `alias grep` and `which grep` to ensure you're running vanilla versions of these programs. As well as `grep -V` to show the version of grep you're using.

Finally, the commands you've provided are invalid. That is, neither `ls | grep` nor `ls | rg` are valid because both `grep` and `rg` require at least one argument. You should provide a complete command that someone else can run.

---

_Comment by @ciscohack on 2024-09-06 14:45_

@BurntSushi Thanks for the comment .. Here is the complete output for reference
<img width="399" alt="image" src="https://github.com/user-attachments/assets/a8a6aea3-0413-4010-9067-f55ec5a8dc31">


•❯•> which ls
/bin/ls


•❯•> which grep
/usr/bin/grep


•❯•> grep -V
grep (BSD grep, GNU compatible) 2.6.0-FreeBSD



---

_Comment by @BurntSushi on 2024-09-06 14:46_

I'm sorry, but you did not provide everything that I asked for.

---

_Comment by @ciscohack on 2024-09-06 15:02_

I don't know what else is missing except this 2 command

alias grep='grep -ia'

and 

# Colorls Dir explorer
if [ -x "$(command -v colorls)" ]; then
    alias ls=" colorls -rtG --report"
    alias la="colorls -altrG --report"
    alias ld="colorls -dtr --report"
    alias lf='colorls  -ftr --report'
    alias ll='colorls -lrtG --report'
fi




---

_Comment by @BurntSushi on 2024-09-06 15:22_

So you're using `colorls` and not `ls`. I don't know what that is or how to get it. You should also show the output of `alias rg`. And any config options you have set for ripgrep in `$RIPGREP_CONFIG_PATH`.

I really can't keep going back-and-forth with you about this. If you can't provide an MRE, then I can't help you. You might need to get someone else to give you a bit more help and guidance in how to [provide an MRE](https://stackoverflow.com/help/minimal-reproducible-example).

You need to be able to provide precise instructions for how someone else can see the same output that you are. Otherwise all I can do is guess. And I've already tried guessing and I can't reproduce your problem:

```
$ echo ' DnsServerSetup.zip' | grep DnsServerSetup.zip
 DnsServerSetup.zip
$ echo ' DnsServerSetup.zip' | rg DnsServerSetup.zip
 DnsServerSetup.zip
```

It works just fine.

I otherwise don't understand what would lead one command to result in emitting `` as-is, while another command would emit `<U+F410>`. Like, `` _is_ `U+F410`, so it's not like the codepoint isn't being detected in some form. I've seen _similar_ but not _identical_ problems happen because an ANSI color escape code gets inserted in the wrong place.

I want to be clear that this is my last response until you can provide a real MRE. If you aren't sure how, please seek out a generic beginners forum and ask them for help.

---

_Comment by @ciscohack on 2024-09-06 15:28_

here is rg detail

# Ripgrep Pager support
rg() {
    # Check if the standard output is connected to a terminal
    if [ -t 1 ]; then
        # If connected to a terminal, use ripgrep with bat for highlighting
        command rg --heading --color always -ia -p "$@" 2>/dev/null | bat -p
    else
        # If not connected to a terminal, use ripgrep without bat
        command rg --heading --color never -ia -p "$@" 2>/dev/null
    fi
}


here is colorls detail what i using it 
https://github.com/athityakumar/colorls

and here is my ripgreprc

# Don't let ripgrep vomit really long lines to my terminal, and show a preview.
# https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file
# https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md

# --max-columns=1000
# --max-columns=150
# --max-columns-preview

# Add my 'web' type.
--type-add
web:*.{html,css,js}*

# Using glob patterns to include/exclude files or folders
--glob=!git/*

# or
--glob
!git/*

# Because who cares about case!?
#--smart-case
--ignore-case

#  pretty output
--pretty
# Specify which regular expression engine to use
--engine=auto

# Configure color settings and styles. (https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file)
# --colors=path:bg:0x3b,0x3b,0x3b
# --colors=path:fg:white
# --colors=line:fg:0xf2,0xc2,0x60
# --colors=match:bg:0x2b,0x83,0xa6
# --colors=match:fg:0xff,0xff,0xff
# --colors=match:style:bold
# --colors=match:style:underline
# --colors=match:style:nobold

# Search hidden files and directories.
--hidden

# make the output look like The Silver Searcher's output
# https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#silver-searcher-output
# --colors=line:fg:yellow
# --colors=line:style:bold
# --colors=path:fg:green
# --colors=path:style:bold
# --colors=match:fg:black
# --colors=match:bg:yellow
# --colors=match:style:intense
# --colors=match:style:underline

# Show context by default
# --context=5

# Count the number of matches
# --count

# suppress line number
--no-line-number

#depth
--maxdepth=3

# https://github.com/BurntSushi/dotfiles/blob/master/.ripgreprc
#black, blue, green, red, cyan, magenta, yellow, white
# --colors=match:bg:0xff,0x7f,0x00
# --colors=match:fg:white
# --colors=match:style:nobold
# --colors=line:none
# --colors=line:fg:magenta
# --colors=path:fg:green
--colors=match:style:bold


--glob=!*.map
# --colors=match:style:underline
# --colors=line:fg:red
# --colors=match:bg:0,128,255
# --colors=match:bg:yellow
# --colors=match:bg:magenta
# --colors=match:fg:0,5,0
# --colors=line:fg:red
# --colors=match:style:underline
# --colors=match:bg:220,240,245
# --colors=match:fg:0,89,240
--colors=match:fg:red
--colors=match:style:bold
#--colors=match:bg:0,96,245
#--colors=line:bg:red
--colors=path:fg:magenta






---

_Comment by @ciscohack on 2024-09-06 16:01_

•❯•> $RIPGREP_CONFIG_PATH
/Users/praverai/.ripgreprc: line 10: --type-add: command not found
/Users/praverai/.ripgreprc: line 11: web:*.html*: command not found
/Users/praverai/.ripgreprc: line 14: --glob=!git/*: No such file or directory
/Users/praverai/.ripgreprc: line 17: --glob: command not found
/Users/praverai/.ripgreprc: line 18: !git/*: No such file or directory
/Users/praverai/.ripgreprc: line 22: --ignore-case: command not found
/Users/praverai/.ripgreprc: line 25: --pretty: command not found
/Users/praverai/.ripgreprc: line 27: --engine=auto: command not found
/Users/praverai/.ripgreprc: line 40: --hidden: command not found
/Users/praverai/.ripgreprc: line 60: --no-line-number: command not found
/Users/praverai/.ripgreprc: line 63: --maxdepth=3: command not found
/Users/praverai/.ripgreprc: line 73: --colors=match:style:bold: command not found
/Users/praverai/.ripgreprc: line 76: --glob=!*.map: command not found
/Users/praverai/.ripgreprc: line 87: --colors=match:fg:red: command not found
/Users/praverai/.ripgreprc: line 88: --colors=match:style:bold: command not found
/Users/praverai/.ripgreprc: line 91: --colors=path:fg:magenta: command not found


---

_Comment by @ciscohack on 2024-09-06 17:50_

@BurntSushi  I found the issue and exact repro step. this is not ripgrep issue infact something with BAT is causing problem.

so here is config snippet I have for rg which causing problem basically problem is not below function in fact simple repro 
step is do **ls | bat** and this will not print the unicode character. so basically when we pipe the ls output with bat. Bat unable to print the character .. can you please now suggest how to fix this bat issue as moar pager can able to print character with pipe

```
rg() {
    # Check if the standard output is connected to a terminal
    if [ -t 1 ]; then
        # If connected to a terminal, use ripgrep with bat for highlighting
        command rg --heading --no-line-number --color always -ia -p "$@" 2>/dev/null | bat -p
    else
        # If not connected to a terminal, use ripgrep without bat
        command rg --heading --no-line-number --color never -ia -p "$@" 2>/dev/null
    fi
}
```

Here is example
<img width="660" alt="image" src="https://github.com/user-attachments/assets/88e500ff-b8ff-47de-b0a1-4543086c251d">


Here is moar output when doing ls | moar

<img width="355" alt="image" src="https://github.com/user-attachments/assets/33ec34d4-e8ec-4e16-9b71-7b02792af3f1">


---

_Comment by @BurntSushi on 2024-09-06 18:01_

OK, thanks. In the future, here is what a good MRE would look like:

```
$ echo ' DnsServerSetup.zip' | bat -p
<U+F410> DnsServerSetup.zip
```

Once you get it down to this point, the next thing you should do is search. This is clearly nothing to do with ripgrep, so try searching the bat issue tracker.

If you search, you should find this, which exactly describes the bug you're seeing: https://github.com/sharkdp/bat/issues/2578

And if you follow that issue, it should become clear that this isn't even a `bat` problem, but a problem with `less`:

```
$ echo ' DnsServerSetup.zip' | less -FX
<U+F410> DnsServerSetup.zip
```

And issue even has a work-around!

```
$ echo ' DnsServerSetup.zip' | LESSUTFCHARDEF=E000-F8FF:p,F0000-FFFFD:p,100000-10FFFD:p less -FX
 DnsServerSetup.zip
```

---

_Closed by @BurntSushi on 2024-09-06 18:01_

---

_Label `invalid` added by @BurntSushi on 2024-09-06 18:01_

---

_Comment by @ciscohack on 2024-09-06 18:37_

@BurntSushi Thanks, if you don't mind one more question why ripgreprc showing below error when we have all this config in man page ... my environment variable is not working due to this error

•❯•> $RIPGREP_CONFIG_PATH
/Users/praverai/.ripgreprc: line 8: --max-columns=150: command not found
/Users/praverai/.ripgreprc: line 9: --max-columns-preview: command not found
/Users/praverai/.ripgreprc: line 12: --hidden: command not found
/Users/praverai/.ripgreprc: line 15: --colors=line:none: command not found
/Users/praverai/.ripgreprc: line 16: --colors=line:style:bold: command not found
/Users/praverai/.ripgreprc: line 19: --smart-case: command not found
/Users/praverai/.ripgreprc: line 23: --heading: command not found
/Users/praverai/.ripgreprc: line 27: --pretty: command not found
/Users/praverai/.ripgreprc: line 31: --binary: command not found
/Users/praverai/.ripgreprc: line 35: --follow: command not found
/Users/praverai/.ripgreprc: line 39: --ignore-case: command not found
/Users/praverai/.ripgreprc: line 43: --no-line-number: command not found
/Users/praverai/.ripgreprc: line 47: --max-depth=10: command not found
/Users/praverai/.ripgreprc: line 51: --context-separator=---: command not found
/Users/praverai/.ripgreprc: line 67: --text: command not found
/Users/praverai/.ripgreprc: line 71: --count: command not found
/Users/praverai/.ripgreprc: line 75: --context-line=match: command not found
/Users/praverai/.ripgreprc: line 78: --engine=auto: command not found
/Users/praverai/.ripgreprc: line 83: --color=auto: command not found
/Users/praverai/.ripgreprc: line 84: --colors=match:fg:green: command not found
/Users/praverai/.ripgreprc: line 85: --colors=line:fg:yellow: command not found


---

_Comment by @BurntSushi on 2024-09-06 19:26_

`$RIPGREP_CONFIG_PATH` probably expands to a file path. So what you have is equivalent to just typing `some/path/to/file` and hitting enter. The output suggests that your shell then tried to _run_ the file as if it were a shell program. This is not usually what happens unless the file is itself executable, and your ripgrep config file _shouldn't_ be executable. But who knows. Maybe it is, or maybe you're running a weird shell that tries to execute things even if they don't have the executable permission bit set. I don't know.

The correct command here would be `cat $RIPGREP_CONFIG_PATH`.

If this doesn't help, I really do suggest that you go find a beginner's forum. Or if you're in school, a teacher or peer who can help walk you through things.

---
