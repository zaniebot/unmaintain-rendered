```yaml
number: 1566
title: Color output character in VIM with --vimgrep --color never 
type: issue
state: closed
author: mangelozzi
labels: []
assignees: []
created_at: 2020-05-03T03:05:48Z
updated_at: 2020-05-03T16:41:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1566
synced_at: 2026-01-12T16:13:23Z
```

# Color output character in VIM with --vimgrep --color never 

---

_@mangelozzi_

I've have seen a few issues about color codes added in windows when they shouldnt be, but the issues have been closed because they have not been reproducible, hopefully this one is.

#### What version of ripgrep are you using?
```
ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?
`scoop`

#### What operating system are you using ripgrep on?
Windows 10

#### Describe your bug.
When calling rg from neovim, there are character which mess up return value, I suspect they are color codes. These occurs even with `--color never`

#### What are the steps to reproduce the behavior?
1. Add the below script to `init.vim`
2. Save the file
3. Then inspect `:messages`
```
function! myautoload#OnStdout(job_id, data, event)
    for line in a:data
        echom a:event.">>>".string(line)
    endfor
endfun
function! myautoload#OnStderr(job_id, data, event)
    echom "My STDERR, job_id:".a:job_id." event:".a:event." data:".string(a:data)
endfun
function! myautoload#OnExit(job_id, data, event)
    echom "My EXIT, job_id:".a:job_id." event:".a:event." data:".string(a:data)
endfun
function! myautoload#TestJobStart()
    let cmd = ["rg", "--vimgrep","--color", "never", "endfun"]
    let s:opts_dict= {
                \ 'on_exit'  : function('myautoload#OnExit'),
                \ 'on_stdout': function('myautoload#OnStdout'),
                \ 'on_stderr': function('myautoload#OnStderr'),
                \ 'pty' : v:true,
                \ 'stdout_buffered':1,
                \ 'rpc': v:false}
    let terminal_buf_nr = jobstart(cmd, s:opts_dict)
endfunction
call myautoload#TestJobStart()
```

#### What is the actual behavior?
Escape codes are added, I am guessing these are color codes?
e.g. `^[[0K^M'` or `^[[0K^[[?25l^M` or `^[[0K^[[?25h`

```
Trailing whitespace stripped & Auto Indented.
"init.vim" 481L, 19701C written
stdout>>>'^[[0m^[[0Kinit - Copy.vim:134:1:endfunction^[[0K^[[?25l^M'
stdout>>>'init - Copy.vim:363:1:endfunction^[[0K^M'
stdout>>>'init.vim:155:1:endfunction^[[0K^M'
stdout>>>'init.vim:386:1:endfunction^[[0K^M'
stdout>>>'init.vim:468:55:    let cmd = ["rg", "--vimgrep","--color", "never", "endfuncti^[[0Ko^M'
stdout>>>'n"]^[[0K^M'
stdout>>>'init.vim:477:1:endfunction^[[0K^M'
stdout>>>'temp.vim:11:1:endfunction^[[0K^M'
stdout>>>'autoload\myautoload.vim:67:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2410:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2414:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2468:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2486:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2518:1:endfunction^[[0K^M'
stdout>>>'autoload\plug.vim:2522:1:endfunction^[[0K^M'
stdout>>>'init\coc.vim:38:1:endfunction^[[0K^M'
stdout>>>'init\coc.vim:62:1:endfunction^[[0K^M'
stdout>>>'init\visual.vim:79:1:endfunction^[[0K^M'
stdout>>>'old\visual_dict.vim:97:1:endfunction^[[0K^M'
stdout>>>'old\text_object_entire_snippets.vim:372:1:endfunction^[[0K^M'
stdout>>>'^[[0K^[[?25h'
My EXIT, job_id:96 event:exit data:0
```

#### What is the expected behavior?
No color codes to be added.

#### What do you think ripgrep should have done?
Not add colour codes. 

---

_Comment by @BurntSushi on 2020-05-03 04:05_

Those don't appear to be added by ripgrep. They are probably coming from somewhere else, but I don't know where because I don't quite understand that vimscript at first glance.

---

_Comment by @BurntSushi on 2020-05-03 04:07_

What is ripgrep actually searching here?

When `--color never` is given, ripgrep will definitely not emit color codes. I'm pretty confident about that.

---

_Comment by @mangelozzi on 2020-05-03 08:54_

Thanks for the reply. With you certainity, and after much searching I came across this post:
[Inexplicable ANSI escape sequences in output of jobs with pty on win32](https://github.com/neovim/neovim/issues/11726)
Which explains its a neovim windows issue.

---

_Closed by @mangelozzi on 2020-05-03 08:54_

---

_Comment by @mangelozzi on 2020-05-03 08:55_

> What is ripgrep actually searching here?
> 
> When `--color never` is given, ripgrep will definitely not emit color codes. I'm pretty confident about that.

For interests sake, it was searching for the term `endfun`

---

_Comment by @BurntSushi on 2020-05-03 12:57_

Aye. I saw its query. But I can't tell what it is actually searching. That is, where is it getting its corpus? What is its input?

This might be relevant for another ticket you filed: https://github.com/neovim/neovim/issues/12239

Namely, if ripgrep thinks it is supposed to read stdin, then it will block forever. I don't actually know what your intent is here. Is ripgrep supposed to be searching stdin? Or the current working directory? Best to make it explicit. Either `rg pattern -` for searching stdin or `rg pattern ./` for searching the current working directory.

Or maybe you are getting the input from elsewhere. Not sure.

---

_Comment by @mangelozzi on 2020-05-03 16:36_

You were right `rg pattern ./` did the trick. its strange that without the `./` it worked in ubuntu, and windows CMD, and in windows vim with `:!rg pattern`, but not with jobstart(). That you so much!! I have spent many many hours trying to figure out the jobstart(), but that wasnt the case. Ps thanks for `--vimgrep`, such a useful flag!

---

_Reopened by @mangelozzi on 2020-05-03 16:36_

---

_Closed by @mangelozzi on 2020-05-03 16:36_

---

_Comment by @BurntSushi on 2020-05-03 16:41_

No problem. The issue is that tty detection is a platform specific operation, and given that you're dealing with weird pty things on Windows, it seemed like a likely source of problems. :-) In effect, without the `./`, ripgrep has to _guess_ what to search. This works in the vast majority of cases, but can fail badly in weird cases like this.

---
