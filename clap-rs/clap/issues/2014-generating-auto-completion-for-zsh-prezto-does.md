```yaml
number: 2014
title: Generating auto completion for zsh-prezto does not work with Deno project
type: issue
state: closed
author: muuvmuuv
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2020-07-14T16:22:10Z
updated_at: 2020-08-14T09:25:51Z
url: https://github.com/clap-rs/clap/issues/2014
synced_at: 2026-01-12T16:14:12Z
```

# Generating auto completion for zsh-prezto does not work with Deno project

---

_@muuvmuuv_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

Ref: https://github.com/denoland/deno/issues/6309

```zsh
_deno() {
    typeset -A opt_args
    typeset -a _arguments_options
    local ret=1

    if is-at-least 5.2; then
        _arguments_options=(-s -S -C)
    else
        _arguments_options=(-s -C) # line 12
    fi

    local context curcontext="$curcontext" state line
    _arguments "${_arguments_options[@]}" \
#....
```

### Steps to reproduce the issue

1. Install deno
2. Run `deno completions zsh > /Users/marvinheilemann/.zprezto/modules/deno/init.zsh`
3. Print `_deno:12: command not found: _arguments`

### Version

* **Rust**: Output of `rustc -V`
* **Clap**: 2.33.1

### Actual Behavior Summary

I am getting an error

### Expected Behavior Summary

I think *this* should happen instead.

### Additional context

I am not familiar with Rust

### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

Paste Debug Output Here

</code>
</pre>
</details>


---

_Label `T: bug` added by @muuvmuuv on 2020-07-14 16:22_

---

_Comment by @pksunkara on 2020-07-14 18:01_

This is an issue with zsh version IIRC. Please paste your zsh version. Also can you please try with clap master branch?

---

_Label `C: generator` added by @pksunkara on 2020-07-14 18:01_

---

_Comment by @muuvmuuv on 2020-07-14 18:16_

I am running the default apple zsh: zsh 5.7.1 (x86_64-apple-darwin19.0)

Sorry but I am not familiar with Rust :/

---

_Comment by @pksunkara on 2020-08-11 01:09_

@intgr Do you think you can take a look at this please?

---

_Comment by @intgr on 2020-08-11 14:20_

Sure, will take a look later (feel free to ping me if I forget).

---

_Comment by @intgr on 2020-08-12 18:32_

The issue here is that zsh is very picky about how it discovers those completion files. You have to place it with file name `_deno` into some directory in the `$fpath` variable, usually `/usr/local/share/zsh/site-functions`. Unfortunately there are no user-local paths by default.

As a one-liner:
```sh
deno completions zsh |sudo tee /usr/local/share/zsh/site-functions/_deno
```
After that you can run `compinit` in your zsh shell and `deno <TAB>` should automagically work.


---

_Comment by @CreepySkeleton on 2020-08-12 20:49_

@intgr Thank you a lot!

@muuvmuuv Could you please check if the proper path and filename solves the problem?

---

_Comment by @muuvmuuv on 2020-08-13 15:16_

@intgr this does not work for me:

```
~/Downloads                                                                                                 at 17:12:43
❯ echo $fpath
/Users/marvinheilemann/.zprezto/modules/prompt/functions /Users/marvinheilemann/.zprezto/modules/git/functions /Users/marvinheilemann/.zprezto/modules/completion/external/src /Users/marvinheilemann/.zprezto/modules/helper/functions /Users/marvinheilemann/.zprezto/modules/utility/functions /Users/marvinheilemann/.zprezto/modules/node/functions /usr/local/share/zsh/site-functions /usr/share/zsh/site-functions /usr/share/zsh/5.7.1/functions

~/Downloads                                                                                                 at 17:12:45
❯ deno completions zsh |sudo tee /usr/local/share/zsh/site-functions/_deno
[1]  + 26538 done                    deno completions zsh |
       26539 suspended (tty output)  sudo tee /usr/local/share/zsh/site-functions/_deno

~/Downloads                                                                                      ✘ 0|TTOU  at 17:13:15
❯ compinit

~/Downloads                                                                                                at 17:13:20
❯ deno Make_Gale/
 -- file --
Make_Gale/         Make_Gale.zip      nested-RCE-Tree_/  test/
```

From my expectation it should anyway be added like any other zprezto module in `$fpath`. 

---

_Comment by @intgr on 2020-08-13 17:01_

> ```
> ❯ deno completions zsh |sudo tee /usr/local/share/zsh/site-functions/_deno
> [1]  + 26538 done                    deno completions zsh |
>        26539 suspended (tty output)  sudo tee /usr/local/share/zsh/site-functions/_deno
> ```

Did that command complete successfully? sudo should ask you for your password.

You should have the file `/usr/local/share/zsh/site-functions/_deno` in the end.

---

_Comment by @muuvmuuv on 2020-08-13 17:05_

Ahh. `|sudo` → `| sudo` :D It worked!

---

_Closed by @pksunkara on 2020-08-14 09:25_

---

_Referenced in [jonringer/nix-template#16](../../jonringer/nix-template/issues/16.md) on 2021-07-18 19:30_

---
