```yaml
number: 564
title: "--clap-X arguments"
type: issue
state: closed
author: hgrecco
labels:
  - A-completion
assignees: []
created_at: 2016-07-03T01:52:33Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/564
synced_at: 2026-01-12T16:14:09Z
```

# --clap-X arguments

---

_@hgrecco_

This proposal is to introduce automagically a family of arguments to scripts using clap. They would be used to support some of the new features at run time (instead of compile time).

For example:

```
  $ myscript --clap-manpage
```

would output the manpage. An installation script could just run this to generate and install it where it is appropriate. 

In a similar way with TAB completion:

```
  $ myscript --clap-complete <text typed by the user before the tab was pressed>
```

would output a list of possible values separated by new lines. This would make the completion script much smaller and easier to port to other terminals as all logic is in clap and not in a shell script. 

This clap arguments will be hidden by default (i.e. will not show up in the help) 


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-03 15:54_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-03 15:54_

---

_Label `C: man page gen` added by @kbknapp on 2016-07-03 15:54_

---

_Label `C: completion gen` added by @kbknapp on 2016-07-03 15:54_

---

_Comment by @kbknapp on 2016-07-03 16:01_

I'm not a huge fan of adding flags/args to end user programs, hidden or not. But if enough people think it'd be useful, I'm not against adding a setting that builds these additional flags so long as it's not on by default.

My other thought is that this type of thing is doable in the end user code, so perhaps just some details examples or blog posts on how to do this would be more appropriate. Assuming we have all the infrastructure in place, a lo `App::gen_completions`, `App::gen_manpage` etc.


---

_Comment by @hgrecco on 2016-07-03 23:39_

Thanks for being open even if at first sight does not sound right to you. What drove me to this is the need to support multiple terminals for completion in simple way. I dislike complicated shell scripts. Using what I have proposed the **bash** completion script will be:

``` shell
_myscript_complete() {
  COMPREPLY=()
  local word="${COMP_WORDS[COMP_CWORD]}"
  local completions="$(myscript --clap-complete "$word")"
  COMPREPLY=( $(compgen -W "$completions" -- "$word") )
}

complete -f -F _myscript_complete myscript
```

Porting this to **zsh** is trivial:

``` shell
_myscript_complete() {
  local word completions
  word="$1"
  completions="$(myscript --clap-complete "${word}")"
  reply=( "${(ps:\n:)completions}" )
}

compctl -f -K _myscript_complete myscript
```

Supporting other terminals (maybe even Windows PowerShell) should as easy to do. The trick is that the logic is in `myscript` keeping the shell script very minimal.


---

_Comment by @joshtriplett on 2016-07-04 22:40_

@hgrecco I think you can simplify that even further for bash; `complete` supports a `-C` option to specify a command to run, which generates completions on stdout (one per line, with backslashes to escape newlines if necessary).


---

_Comment by @hgrecco on 2016-07-06 02:22_

@joshtriplett cool, thanks for the tip!


---

_Closed by @kbknapp on 2016-08-21 19:47_

---
