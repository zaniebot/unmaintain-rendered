```yaml
number: 101
title: Ability to simultaneously dump results with line numbers to a user-specified log file 
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-26T14:21:11Z
updated_at: 2016-12-01T21:02:22Z
url: https://github.com/BurntSushi/ripgrep/issues/101
synced_at: 2026-01-12T16:13:21Z
```

# Ability to simultaneously dump results with line numbers to a user-specified log file 

---

_@kaushalmodi_

Hello,

Here is my typical workflow when running any command line search tool.
1. Search for something
2. At times, open a file from the listed results at the exact line number in ${EDITOR}

To speed this up with `rg`, I tried the below but it is inefficient as I need to run `rg` twice:

```
alias _rg    '\rg --follow --no-ignore-vcs --smart-case'
alias rglog  '_rg --line-number \!* >! ${SEARCHLOG}'
alias rg     '_rg \!*; (rglog \!*&)'
```

_PS: While working with above, I realized that `--line-number` is off by default when piping the result to a file._

Using above, I would do

```
rg foo
```

I would see the results in color on stdout and also get the log file `${SEARCHLOG}` saved.

Then I use [`percol`](https://github.com/mooz/percol) to quickly jump to the result:

Here's the alias if interested:

```
alias pagl  'cat ${SEARCHLOG} | percol --query=\!* | \\
      (e `awk -F: '"'"'{ if ( $2 ) {print "+" $2 " " $1} else {print} }'"'"'` &)'
```

_`e` is my alias to open by default editor._

Instead of running `rg` twice as above, I even tried the below:

```
alias rg_bad '_rg --line-number \!* |& tee ${SEARCHLOG}'
```

But that shows the results in black&white on stdout!

**Questions:**
- Would you suggest a unixy way of doing the same (my `rg` alias implementation above) in a more efficient way so that `rg` does not need to be run twice?
- Would an option like `--tee-results-file ${SEARCHLOG}` be feasible to be added so that I see the results in color on stdout (_eat the cake__) **and also** get my `${SEARCHLOG}` (_have the cake_).


---

_Comment by @BurntSushi on 2016-09-26 17:12_

At a high level, I think the way people usually solve this problem is by running `rg` (or whatever search tool) in your editor. I don't find myself doing that often, so I don't have any concrete tips for you.

I'm unlikely to support a `--tee-results-file` option, not only because that would be a significant complication (because the flag opens the doors to "well have --foo for normal operation but --tee-foo for when the --tee-results-file flag is used), but because it's a significant conflation of concerns.

You might consider forcing colors to be shown (say, with the `--color` or `-p` flags) which should work with your `tee` command, and then try to extract the line number from that (but you'll need to deal with the fact that it contains ANSI escape codes).


---

_Comment by @kaushalmodi on 2016-09-26 18:22_

Thanks. The `-p` flag sort of helps..

Before I post process the search log, I get:

![image](https://cloud.githubusercontent.com/assets/3578197/18846241/fa8522e0-83f2-11e6-9679-b1dcc418d9f7.png)

After removing the ANSI color codes ([found this perl one-liner](http://unix.stackexchange.com/a/4529/57923)), I get:

![image](https://cloud.githubusercontent.com/assets/3578197/18846273/2583458a-83f3-11e6-8e98-06dbeed9f29a.png)

As we can see there are 2 issues here:
1. The perl one-liner which apparently works in general for removing ANSI color codes did not do a complete cleanup in this case.. In the second picture above, `^O` escape codes are still left out. Would you have an idea why that's the case? 
2. The `tee`'d search log should have been in this format (below produced using `rg .. >! foo.log`    
   
   ```
   25/org-plus-contrib-20160829/org-agenda.el:4174:(defvar org-arg-loc nil) ; local variable
   25/org-plus-contrib-20160829/org-agenda.el:4248:      (org-set-local 'org-arg-loc arg)
   general.el:103:(defconst modi/rg-arguments
   setup-files/setup-projectile.el:109:                         modi/rg-arguments
   elisp/org-mode/lisp/org-agenda.el:4166:(defvar org-arg-loc nil) ; local variable
   elisp/org-mode/lisp/org-agenda.el:4239:      (setq-local org-arg-loc arg)
   elisp/org-mode/lisp/org-agenda.el.1.bkp:4182:(defvar org-arg-loc nil) ; local variable
   elisp/org-mode/lisp/org-agenda.el.1.bkp:4256:      (setq-local org-arg-loc arg)
   ```
   
   The output needs to be in `FILE:LINE:STRING` format as above. But it's not in the picture 1 and 2 above. That's because `-p` sets `--heading` too. But looks like there is no other option. I either have output with headings in **both** stdout and tee'd file or no headings in both (just `--color=always`). 

> by running rg (or whatever search tool) in your **editor.**

I do that too. But I like have this terminal based flow too.

> I'm unlikely to support a --tee-results-file option, not only because that would be a significant complication

OK, that is understood. But I hope you can help me with point 1 above regarding the mysterious `^O` escape code.


---

_Comment by @kaushalmodi on 2016-09-26 18:50_

Turns out that the `rg` output has `"^O"` characters too, in addition to the standard ANSI color escape codes.

So using just [this](http://unix.stackexchange.com/a/4529/57923) or [this](http://www.commandlinefu.com/commands/view/3584/remove-color-codes-special-characters-with-sed) did not help.

On examining that `"^O"` character, I figured out that it is `0x0F`:

```
             position: 224 of 529 (42%), column: 5
            character: C-o (displayed as C-o) (codepoint 15, #o17, #xf)
    preferred charset: ascii (ASCII (ISO646 IRV))
code point in charset: 0x0F
               script: latin
               syntax: .    which means: punctuation
             to input: type "C-x 8 RET f" or "C-x 8 RET SHIFT IN"
          buffer code: #x0F
            file code: #x0F (encoded by coding system undecided-unix)
              display: no font available
       hardcoded face: escape-glyph

Character code properties: customize what to show
  old-name: SHIFT IN
  general-category: Cc (Other, Control)
```

So, eventually, this seems to do the trick of removing **all** the escape codes:

``` sh
alias rg2 "rg --line-number -p \!* |& tee /tmp/${USER}_rg_temp.txt; \\
           \sed -r -e 's/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g' \\
                   -e 's/\x0F//g' /tmp/${USER}_rg_temp.txt \\
                >! ${SEARCHLOG}; \\
           \rm -f /tmp/${USER}_rg_temp.txt"
```

You can close this bug report if that `"^O"` character was expected there.

Thanks.


---

_Comment by @kaushalmodi on 2016-09-26 18:54_

Now I just need a script to convert from

```
25/org-plus-contrib-20160829/org-agenda.el
4174:(defvar org-arg-loc nil) ; local variable
4248:      (org-set-local 'org-arg-loc arg)
```

to

```
25/org-plus-contrib-20160829/org-agenda.el:4174:(defvar org-arg-loc nil) ; local variable
25/org-plus-contrib-20160829/org-agenda.el:4248:      (org-set-local 'org-arg-loc arg)
```


---

_Comment by @BurntSushi on 2016-09-26 20:58_

That `^0` might be a bug in the term coloring. I wonder if it's a dupe of #37. What is the output of `echo $TERM` in the same environment in which you're running `rg`?


---

_Comment by @kaushalmodi on 2016-09-27 01:02_

> I wonder if it's a dupe of #37. 

It certainly looks like that.

> What is the output of echo $TERM in the same environment in which you're running rg?

It's `screen` for me (too?). I need to set `$TERM` to `screen` for [`tmux`](https://github.com/tmux/tmux).


---

_Comment by @BurntSushi on 2016-09-27 01:07_

If you do `TERM=xterm rg ...`, does that fix things?


---

_Comment by @kaushalmodi on 2016-09-27 01:15_

I tried that, but now I get a different extra escape code `^[(B` (after removing ANSI color codes using [this sed script](http://www.commandlinefu.com/commands/view/3584/remove-color-codes-special-characters-with-sed)).

![image](https://cloud.githubusercontent.com/assets/3578197/18857012/5478c3fc-842e-11e6-9383-9e14922bdffe.png)


---

_Comment by @kaushalmodi on 2016-09-27 01:21_

Here's a screencap for the same output _before_ removing any ANSI color escapes.

![image](https://cloud.githubusercontent.com/assets/3578197/18857124/53584460-842f-11e6-9692-874c7e9edbdb.png)

It looks like in both cases `^O` or `^[(B`, those extra cases are appearing where the colored text substrings end (see below):

![image](https://cloud.githubusercontent.com/assets/3578197/18857114/2ec7438a-842f-11e6-9e9a-9dfcfaf60783.png)


---

_Comment by @BurntSushi on 2016-09-29 01:13_

I'm going to close this with the hope that #37 is a dupe. If after fixing #37 this is still a problem, we can revisit.


---

_Closed by @BurntSushi on 2016-09-29 01:13_

---

_Comment by @kaushalmodi on 2016-12-01 17:04_

For the sake of completeness, I now 'sort of solve' the original issue in the first post as follows

rg still needs to be run twice in some cases. But atleast now I can run something like below

> rg_wrapper.sh --help | rg_wrapper.sh 'SOMETHING' | cat -

with results from that first rg_wrapper.sh saved to a `${SEARCHLOG}`.

```bash
#!/usr/bin/env bash
# Time-stamp: <2016-12-01 11:47:54 kmodi>

set -euo pipefail # http://redsymbol.net/articles/unofficial-bash-strict-mode

# /home/kmodi/scripts/bash/rg_wrapper.sh: line 81: /home/kmodi/usr_local/6/bin/rg
# --follow --no-ignore-vcs --smart-case --ignore-file /home/kmodi/.ignore: No such
# file or directory
# Uncommenting below gives the above error
# IFS=$'\n\t'

# Initialize variables
debug=0
user_args=''
piped_data=''

input_from_pipe_flag=0
output_to_pipe_flag=0

rg_bin="rg"
if ! hash ${rg_bin} 2>/dev/null # Quit if rg is not found in PATH
then
    echo "ERROR: The ripgrep binary (rg) is not installed."
    echo "Install from https://github.com/BurntSushi/ripgrep/releases ."
    exit 1
fi

# Parse the arguments, mainly to catch the debug flag for this script.
# Rest of the arguments are passed to rg.
while [[ $# -gt 0 ]]
do
    case "$1" in
        "-D"|"--debug" ) debug=1;;
        * ) user_args="${user_args} $1";;
    esac
    shift # expose next argument
done

if [[ ${debug} -eq 1 ]]
then
    echo "* Debug Mode *"
fi

set +e # Ignore if the below which or grep results in error
rg_in_home=$(which -a ${rg_bin} | grep -E '^/home' 2>/dev/null)
set -e
if [[ ${rg_in_home} != "" ]] # If rg binary exists in a dir in user's home
then
    rg_bin="${rg_in_home}"
fi
if [[ ${debug} -eq 1 ]]
then
    echo "rg_in_home: ${rg_in_home}"
    echo "rg_bin    : ${rg_bin}"
fi

rg_cmd="${rg_bin} \
--follow \
--no-ignore-vcs \
--smart-case \
--type-add ss:sim.setup* \
--ignore-file /home/${USER}/.ignore"
if [[ ${debug} -eq 1 ]]
then
    echo "rg base command: ${rg_cmd}"
fi

# https://gist.github.com/davejamesmiller/1966557
if [[ -t 0 ]] # Script is called normally - Terminal input (keyboard) - interactive
then
    # rg_wrapper.sh foo
    # rg_wrapper.sh foo | cat -
    if [[ ${debug} -eq 1 ]]
    then
        echo "-- Input from terminal --"
    fi
    input_from_pipe_flag=0
else # Script is getting input from pipe or file - non-interactive
    # echo bar | rg_wrapper.sh foo
    # echo bar | rg_wrapper.sh foo | cat -
    piped_data="$(cat)"
    if [[ ${debug} -eq 1 ]]
    then
        echo "-- Input from pipe/file --"
    fi
    input_from_pipe_flag=1
fi

if [[ ${debug} -eq 1 ]]
then
    echo "User Args: ${user_args}"
    echo "Pipe Args: ${piped_data}"
fi

# http://stackoverflow.com/a/911213/1219634
if [[ -t 1 ]] # Output is going to the terminal
then
    # rg_wrapper.sh foo
    # echo bar | rg_wrapper.sh foo
    if [[ ${debug} -eq 1 ]]
    then
        echo "-- Output to terminal --"
    fi
    output_to_pipe_flag=0
else # Output is going to a pipe, file?
    # rg_wrapper.sh foo | cat -
    # echo bar | rg_wrapper.sh foo | cat -
    if [[ ${debug} -eq 1 ]]
    then
        echo "-- Output to a pipe --"
    fi
    output_to_pipe_flag=1
fi

# Update the ${SEARCHLOG} using a sub-process, when input is not from a pipe
if [[ -z ${SEARCHLOG+x} ]]
then
    export SEARCHLOG="/tmp/${USER}_search.log"
fi
(
    if [[ ${input_from_pipe_flag} -eq 0 ]]
    then
        ${rg_cmd} --line-number ${user_args} > ${SEARCHLOG}
    fi
)

# Below statements can be optimize to fewer if cases, but they are kept for
# learning purpose.
if [[ ${input_from_pipe_flag} -eq 0 && ${output_to_pipe_flag} -eq 0 ]]
then
    # rg_wrapper.sh foo
    ${rg_cmd} ${user_args}

elif [[ ${input_from_pipe_flag} -eq 0 && ${output_to_pipe_flag} -eq 1 ]]
then
    # rg_wrapper.sh foo | cat -
    ${rg_cmd} ${user_args}

elif [[ ${input_from_pipe_flag} -eq 1 && ${output_to_pipe_flag} -eq 0 ]]
then
    # echo foo | rg_wrapper.sh
    echo "${piped_data}" | ${rg_cmd} ${user_args}

elif [[ ${input_from_pipe_flag} -eq 1 && ${output_to_pipe_flag} -eq 1 ]]
then
    # echo foo | rg_wrapper.sh | cat -
    echo "${piped_data}" | ${rg_cmd} ${user_args}

fi
```

---

_Comment by @BurntSushi on 2016-12-01 17:18_

@kaushalmodi Wow that's insane! I must confess that I don't have time to grok all of that but I'm glad you found something that sort-of works. :-)

---

_Comment by @kaushalmodi on 2016-12-01 21:00_

The "sort of works" is just because I still need to run `rg` twice; once for the stdout and second time for saving to the SEARCHLOG file.

That said, it now works 100% as I expected.

Earlier I had:

---
**BAD CODE**
```
alias _rg    '\rg --follow --no-ignore-vcs --smart-case'
alias rglog  '_rg --line-number \!* >! ${SEARCHLOG}'
alias rg     '_rg \!*; (rglog \!*&)'
```
---

The issue was that it did not work if I did `rg | rg 'something'`. The above wrapper script is verbose because I deal with all 4 use cases: Data Read From Pipe (yes/no) x Data Output To Pipe (yes/no), while also output search results to stdout and SEARCHLOG.

```
rg_wrapper.sh foo
rg_wrapper.sh foo | cat -
echo foo | rg_wrapper.sh
echo foo | rg_wrapper.sh | cat -
```

---
