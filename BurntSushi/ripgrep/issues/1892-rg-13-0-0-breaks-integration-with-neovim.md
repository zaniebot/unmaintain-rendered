```yaml
number: 1892
title: rg 13.0.0 breaks integration with NeoVim
type: issue
state: closed
author: datanoise
labels: []
assignees: []
created_at: 2021-06-13T19:22:33Z
updated_at: 2021-08-30T21:41:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1892
synced_at: 2026-01-12T16:13:24Z
```

# rg 13.0.0 breaks integration with NeoVim

---

_@datanoise_

I am using vim-grepper NeoVim plugin that uses new jobstart api to start ripgrep as a background process and uses callback functions to read the produced output. With the new ripgrep version 13.0.0, this plugin stopped working.

I have found that commit 020c5453a588025 cause this problem. Just as the comment to this commit suggested, the ripgrep completely blocks forever without reporting any output. Reverting this change, fixes the problem. 

Is there a way to provide the command line option to disable checking for sockets in this method?


---

_Comment by @BurntSushi on 2021-06-13 19:32_

I'm not keen on adding flags to change stdin detection unless there is absolutely no other way.

I suppose the obvious question here is this: why is ripgrep being started with a socket attached to stdin that shouldn't be searched?

I tried to answer this question myself by looking at the [plugin's source](https://github.com/mhinz/vim-grepper/blob/master/plugin/grepper.vim), but couldn't really make sense of it.

---

_Comment by @datanoise on 2021-06-13 21:07_

I believe that is how background job processing is implemented in NeoVim. NeoVim uses libuv with the event loop processing, which AFAIK is socket based. 

---

_Comment by @BurntSushi on 2021-06-13 21:10_

Thanks for responding, but it doesn't really help my understanding here. Why is it attaching a socket to stdin in the first place?

I understand this is probably frustrating since ripgrep changed its behavior and caused a breakage here, but before I'm willing to implement a kludge or break somebody's else's patch, I need to understand whether the plugin itself is working correctly or not. It eould be very surprising to me if "attach a socket to stdin" was an unavoidable consequence of libuv, for example. Either something about my mental model is off, or something isn't quite right on the plugin side of things.

---

_Comment by @BurntSushi on 2021-06-13 21:11_

And note that a work around is very simple here. No flag is needed. Just pass `./` to ripgrep.

---

_Comment by @datanoise on 2021-06-13 21:33_

I've just checked and the plugin is working fine with the regular Vim. It must be something related to the way NeoVim implements the communication with the external processes. So I assumed that it is related to libuv, but I'm not 100% sure about it.

Passing `./` to ripgrep doesn't help with the problem.


---

_Comment by @BurntSushi on 2021-06-13 21:36_

> Passing `./` to ripgrep doesn't help with the problem.

That's quite interesting because that should disable stdin detection. Hmmm.

---

_Comment by @datanoise on 2021-06-13 21:38_

Maybe i'm using it incorrectly. Is this a correct way to run it?:

``
rg -H --no-heading --vimgrep is_socket ./
``

---

_Comment by @BurntSushi on 2021-06-13 21:45_

Yes. That should not try to "detect" whether stdin is readable or not. But maybe my understanding is wrong. I'll check the code again once I'm back on a keyboard.

---

_Comment by @kevinhwang91 on 2021-06-14 09:07_

```diff
diff --git a/plugin/grepper.vim b/plugin/grepper.vim
index 73e4e36..40c455f 100644
--- a/plugin/grepper.vim
+++ b/plugin/grepper.vim
@@ -935,6 +935,7 @@ function! s:run(flags)
           \ 'on_stdout': function('s:on_stdout_nvim'),
           \ 'on_stderr': function('s:on_stdout_nvim'),
           \ 'on_exit':   function('s:on_exit'),
+          \ 'pty': 1
           \ }
     if !a:flags.stop
       let opts.stdout_buffered = 1
```
should work.
using pty option will close stdin. I'm not instead in a PR for vim-grepper. Free feel to borrow the idea. Because the plugin is not specified to rg, I don't know whether this cause the side effective for other tools.

---

_Comment by @blueyed on 2021-06-14 13:28_

Note that this also affects other (Neovim) plugins, which used the workaround to append ".": https://github.com/nvim-telescope/telescope.nvim/pull/908

> And note that a work around is very simple here. No flag is needed. Just pass `./` to ripgrep.

Any specific reason to use `./`, or is `.` enough already / the same?

---

_Comment by @datanoise on 2021-06-14 13:43_

> ```diff
> ```diff
> diff --git a/plugin/grepper.vim b/plugin/grepper.vim
> index 73e4e36..40c455f 100644
> --- a/plugin/grepper.vim
> +++ b/plugin/grepper.vim
> @@ -935,6 +935,7 @@ function! s:run(flags)
>            \ 'on_stdout': function('s:on_stdout_nvim'),
>            \ 'on_stderr': function('s:on_stdout_nvim'),
>            \ 'on_exit':   function('s:on_exit'),
> +          \ 'pty': 1
>            \ }
>      if !a:flags.stop
>        let opts.stdout_buffered = 1
> ```
> 
> 
>     
>       
>     
> 
>       
>     
> 
>     
>   
> should work.
> using pty option will close stdin. I'm not instead in a PR for vim-grepper. Free feel to borrow the idea. Because the plugin is not specified to rg, I don't know whether this cause the side effective for other tools.
> ```

Thanks. That fixes the issue. It seems to work fine with other tools like `ag` and `grep`. 

---

_Closed by @datanoise on 2021-06-14 13:43_

---

_Comment by @BurntSushi on 2021-06-14 23:03_

OK, doing a bit of a code walkthrough here, this is the implementation of `is_stdin_readable`:

https://github.com/BurntSushi/ripgrep/blob/7ce66f73cf7e76e9f2557922ac8e650eb02cf4ed/crates/cli/src/lib.rs#L183-L213

On Unix, this is getting a handle to stdin's file descriptor, `stat`ing it and returning true only if its file type is a regular file, a fifo or a socket. Finally, it only returns true if it knows that stdin is _not_ a tty, which is done via `isatty`. This might actually be why the `pty` option works: AIUI, it causes `isatty(0)` to return `true` and thus `is_readable_stdin` would return `false` regardless of the file type detection.

So where is this used? It's used inside of ripgrep when determining which thing to search _when it is otherwise not given at least one thing to search_ as a positional parameter:

https://github.com/BurntSushi/ripgrep/blob/7ce66f73cf7e76e9f2557922ac8e650eb02cf4ed/crates/core/args.rs#L1325-L1342

Whether `search_cwd` is true or not, in this case, I believe, depends completely on what `is_readable_stdin` (as defined above) returns. Namely, none of `--file`, `--files`, `--type-list` or `--pcre2-version` are given to `rg` here. Thus, in this case, ripgrep will only search the CWD if it believes that stdin is _not_ readable. If the plugin runs ripgrep in a way that causes stdin to be open, _not_ a tty and report itself as a socket, then indeed, ripgrep will consider stdin readable and choose to search it instead of the CWD.

The part where I'm getting hung up is that the `path_default` routine is _only_ called when _no_ paths are given to ripgrep:

https://github.com/BurntSushi/ripgrep/blob/7ce66f73cf7e76e9f2557922ac8e650eb02cf4ed/crates/core/args.rs#L554-L559

So, if you provide `./` (or `.`, they are indeed the same), then all this business about stdin sniffing is totally skipped. So the fact that `./` didn't work is quite surprising to me, and if there is an easy way to reproduce that outside of vim in a more controlled experiment, I would consider that a bug. My guess though is that there is probably something else going on.

---

_Comment by @BurntSushi on 2021-07-10 10:24_

A clarification: folks seem to think that I am holding a line here because I'm insisting on being "technically correct" here. That's _not_ why I'm doing what I'm doing here. The [patch that broke neovim plugin integration](https://github.com/BurntSushi/ripgrep/commit/020c5453a5880257c87949030550d24c596f5c8d) was fixing another bug. Perhaps a less prominent one, but not insignificant. So the choice here isn't between "being pragmatic" and "being technically correct." The choice here is, "who do we break?"

(As far as I can tell anyway.)

---

_Comment by @blueyed on 2021-07-12 12:51_

@BurntSushi
What do you think of having an explicit flag (or env var) to control this?
The problem with the workaround in vim-grepper (Neovim) to append "." (automatically always) is bad, since it does not allow to search sub-directories then only (by specifying "pattern subdir"), since it would get turned into "rg … pattern subdir .".
(not appending "." in that case (of having specified (a) subdir(s)) is not trivial, given there is a single input line only etc (https://github.com/mhinz/vim-grepper/issues/162))

---

_Comment by @BurntSushi on 2021-07-12 13:08_

I'm not keen on adding flags for this. Especially since it looks like a temporary problem.

@blueyed Does the [`pty` trick above](https://github.com/BurntSushi/ripgrep/issues/1892#issuecomment-860695447) not work for you? (Or maybe that's only available in `vim` and not `neovim`?)

---

_Comment by @BurntSushi on 2021-07-12 13:09_

Crazy untested idea: is it possible to connect stdin to `/dev/null`? Does that work?

---

_Comment by @blueyed on 2021-07-12 13:19_

@BurntSushi 
I've tried to close stdin in Neovim, but it is likely too late then already (since it can only be done after the process was started; I've created https://github.com/neovim/neovim/issues/15068 to support this).

The pty-trick needs to be extended (https://github.com/mhinz/vim-grepper/issues/244#issuecomment-860755993), but might be what ends in vim-grepper if there is nothing better, like a fix/explicit flag in ripgrep.

---

_Comment by @wsdjeg on 2021-08-07 01:03_

Why this issue is closed? does this bug have been fixed. I still can reproduce it in my plugin. https://github.com/SpaceVim/SpaceVim/issues/4329

neovim version is: v0.5.0 release

---

_Comment by @BurntSushi on 2021-08-07 01:24_

The answer to your question is in the comments above. The problem isn't in ripgrep. Neovim is attaching a readable stdin file descriptor to every subprocess executed. There's even a PR against neovim to fix it. Work arounds have been documented above.

---

_Comment by @wsdjeg on 2021-08-07 01:51_

hmm. I do not know. But why old old version of rg works well for me. I need to lock my rg version to 0.10.0

---

_Comment by @BurntSushi on 2021-08-07 01:54_

It's explained above. See the link to the commit that introduced this change for example.

You don't need to use an old version of ripgrep. Certainly not one that is many years old. Just use `rg foo ./` instead of `rg foo`.

Again, please read the comments above. It should all be explained. If you have specific questions to things said, feel free to ask. 

---

_Comment by @ddeville on 2021-08-20 21:44_

To close the loop here (and for those still following this issue), https://github.com/neovim/neovim/pull/14812 landed in Neovim and provides a way for `stdin` to not be piped when calling `jobstart`. vim-grepper was updated so that it uses that option in https://github.com/mhinz/vim-grepper/pull/250. This should fix the issue where ripgrep doesn't return any results when used from vim-grepper.

Note that the Neovim fix hasn't been pushed in an official release yet (that is 0.5 doesn't have it) and thus it requires using the nightly 0.6 Neovim build.

Thanks for your help investigating this @BurntSushi!

---

_Comment by @wsdjeg on 2021-08-21 00:13_

@ddeville thanks, I am using 0.5.0 now.

---

_Comment by @bellini666 on 2021-08-30 21:41_

For anyone comming here using vim-grepper, I have a suggestion to help you guys use it and also apply the workaround suggested by this comment: https://github.com/BurntSushi/ripgrep/issues/1892#issuecomment-894588622

I have a custom function that gets called by some keymappings that I ported from this plugin a while ago: https://github.com/vim-scripts/grep.vim . The function is:

```vim
function! s:my_run_grep()
    let l:pattern = trim(input('Search for pattern: ', expand('<cword>')))
    if l:pattern == ''
      return
    endif
    let l:pattern = '"' . substitute(l:pattern, '"', '\"', "g") . '"'
    echo "\r"

    let l:dirs = trim(input('Limit for directory: ', './', 'dir'))
    if l:dirs != ''
      let l:dirs = '"' . l:dirs . '"'
    endif
    echo "\r"

    let l:files = trim(input('Limit for files pattern: ', '*'))
    if l:files == '*'
      let l:files = ''
    else
      let l:files = '-g "' . l:files . '"'
    endif
    echo "\r"

    :echo "GrepperRg " . l:pattern . " " . l:dirs . " " . l:files
    :execute "GrepperRg " . l:pattern . " " . l:dirs . " " . l:files
endfunction

nnoremap <silent> <Leader>gr :call <SID>my_run_grep()<CR>
```

So, when I press `<Leader>gr` (as in Grep Recursive) I get prompted for the pattern, which is autocompleted by the word in my cursor, the directory that I want to run (by default I set it to `./` so it works on current neovim version, but I can very easily type and autocomplete other directories) and later to limit for an optional filetype (e.g. `*.py`). Also, note that the pattern can have spaces in it because of the way that I execute the command

It speeds up a lot of things for me. If I want to grep for the word in my in my cursor without limiting the directories or the filetypes I can just `<Leader>gr` and then press `<Enter>` 3 times in a row.

---
