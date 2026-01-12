```yaml
number: 6107
title: "Native completions have incorrect behavior when completing in the middle with `sudo`"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - E-hard
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2025-08-19T13:31:55Z
updated_at: 2025-09-08T23:07:23Z
url: https://github.com/clap-rs/clap/issues/6107
synced_at: 2026-01-12T16:14:17Z
```

# Native completions have incorrect behavior when completing in the middle with `sudo`

---

_@epage_

### Problem

`cargo` does not complete correctly when I try to complete *in the middle* with `sudo` using the `native-completions` feature.

### Steps

1. Run `source <(CARGO_COMPLETE=bash cargo +nightly)` to ensure that we're using `native-completions` feature
2. Create a cargo project with two examples, `apple` and `banana`
3. Invoke completion like this:
  ```bash
  cargo build --example ap<TAB> --example banana
  ```
4.  Notice that we've got the correct result:
  ```bash
  cargo build --example apple --example banana
  ```
5. Prepend with `sudo` and invoke completion again:
  ```bash
  sudo cargo build --example ap<TAB> --example banana
  ```
6. Notice that we've got the wrong result:
  ```bash
  sudo cargo build --example banana --example banana
  ```

### Possible Solution(s)

_No response_

### Notes

Completions by `rustup completions bash` are correct in both cases, and this bug only occurs when using completions by `CARGO_COMPLETE=bash cargo +nightly`.

### Version

```text
$ cargo +nightly version --verbose
cargo 1.91.0-nightly (71eb84f21 2025-08-17)
release: 1.91.0-nightly
commit-hash: 71eb84f21aef43c07580c6aed6f806a6299f5042
commit-date: 2025-08-17
host: x86_64-unknown-linux-gnu
libgit2: 1.9.1 (sys:0.20.2 vendored)
libcurl: 8.14.1-DEV (sys:0.4.82+curl-8.14.1 vendored ssl:OpenSSL/3.5.0)
ssl: OpenSSL 3.5.0 8 Apr 2025
os: Ubuntu 24.4.0 (noble) [64-bit]
```

---

_Label `E-hard` added by @epage on 2025-08-19 13:31_

---

_Label `A-completion` added by @epage on 2025-08-19 13:31_

---

_Label `E-help-wanted` added by @epage on 2025-08-19 13:31_

---

_Referenced in [rust-lang/cargo#15862](../../rust-lang/cargo/issues/15862.md) on 2025-08-19 13:32_

---

_Label `C-enhancement` added by @epage on 2025-08-19 13:32_

---

_Renamed from "Incorrect behavior when completing in the middle with `sudo`" to "Incorrect behavior when completing in the middle with `sudo` (native competions)" by @epage on 2025-08-19 13:33_

---

_Renamed from "Incorrect behavior when completing in the middle with `sudo` (native competions)" to "Native completions have incorrect behavior when completing in the middle with `sudo`" by @epage on 2025-08-19 13:33_

---

_Comment by @epage on 2025-08-19 13:34_

In figuring this out, maybe we'd gain insights into how to resolve #5653

---

_Comment by @Unique-Usman on 2025-08-19 14:31_

Hi @epage, following up with our discussion, I will like to take on this. Thank you. 

---

_Comment by @Unique-Usman on 2025-08-20 11:45_

Hi @epage, I am already looking towards the issue, 

The main cause of the issue is mostly because, the Native was setup only for when the command starts with Cargo but, was not setup when the command start with sudo. I guess the solution will be to find where the completion is defined or implemented, and implement it for Sudo scenerio.

---

_Comment by @Unique-Usman on 2025-08-20 14:30_

Also @epage, I am guessing the reason why the issue is open against clap is because the cargo uses a library from the clap ? 

---

_Comment by @epage on 2025-08-20 15:50_

> The main cause of the issue is mostly because, the Native was setup only for when the command starts with Cargo but, was not setup when the command start with sudo. I guess the solution will be to find where the completion is defined or implemented, and implement it for Sudo scenerio.

Maybe I'm missing something but that seems to be re-framing the symptom while the root cause still needs to be determined.

It appears that `sudo` can tell bash to delegate completions to another program (like we want in #5653).  What does this look like to our completer?  How is this normally expected to work in completers?

> I am guessing the reason why the issue is open against clap is because the cargo uses a library from the clap ?

Yes, cargo is using `clap_complete::env` to handle completions.



---

_Comment by @Unique-Usman on 2025-08-21 18:46_

Hi @epage,

So, I have made some attempt in figuring out what the problem is with when `sudo` is placed at the front of the `cargo`,

Basically when someone run 

`source <(CARGO_COMPLETE=bash cargo +nightly)`

this part of the command is used 

https://github.com/rust-lang/cargo/blob/master/src/bin/cargo/main.rs#L43

```
    if nightly_features_allowed {
        let _span = tracing::span!(tracing::Level::TRACE, "completions").entered();
        let args = std::env::args_os();
        let current_dir = std::env::current_dir().ok();
        let completer = clap_complete::CompleteEnv::with_factory(|| {
            let mut gctx = GlobalContext::default().expect("already loaded without errors");
            cli::cli(&mut gctx)
        })
        .var("CARGO_COMPLETE");
        if completer
            .try_complete(args, current_dir.as_deref())

```

and the above basically make a call to clap

```
 pub fn try_complete(
        self,
        args: impl IntoIterator<Item = impl Into<OsString>>,
        current_dir: Option<&std::path::Path>,
    ) -> clap::error::Result<bool> {
        self.try_complete_(args.into_iter().map(|a| a.into()).collect(), current_dir)
    }

    fn try_complete_(
        self,
        mut args: Vec<OsString>,
        current_dir: Option<&std::path::Path>,
    ) -> clap::error::Result<bool> {
        let Some(name) = std::env::var_os(self.var) else {
            return Ok(false);
        };
        if name.is_empty() || name == "0" {
            return Ok(false);
        }

        // Ensure any child processes called for custom completers don't activate their own
        // completion logic.
        std::env::remove_var(self.var);

        let shell = self.shell(std::path::Path::new(&name))?;

        let mut cmd = (self.factory)();
        cmd.build();

        let completer = args.remove(0);
        let escape_index = args
            .iter()
            .position(|a| *a == "--")
            .map(|i| i + 1)
            .unwrap_or(args.len());
        args.drain(0..escape_index);
        if args.is_empty() {
            let mut buf = Vec::new();
            self.write_registration(&cmd, current_dir, shell, completer, &mut buf)?;
            std::io::stdout().write_all(&buf)?;
        } else {
            let mut buf = Vec::new();
            shell.write_complete(&mut cmd, args, current_dir, &mut buf)?;
            std::io::stdout().write_all(&buf)?;
        }

```

at this point, the `if args.is_empty()` will be true and `self.write_registration()` will be called and this will make a call to `shell.write_registration()`

https://github.com/clap-rs/clap/blob/master/clap_complete/src/env/mod.rs#L245

and `shell.write_registration()` basically makes a create a shell program

```
        let script = r#"
_clap_complete_NAME() {
    local IFS=$'\013'
    local _CLAP_COMPLETE_INDEX=${COMP_CWORD}
    local _CLAP_COMPLETE_COMP_TYPE=${COMP_TYPE}
    if compopt +o nospace 2> /dev/null; then
        local _CLAP_COMPLETE_SPACE=false
    else
        local _CLAP_COMPLETE_SPACE=true
    fi
    local words=("${COMP_WORDS[@]}")
    if [[ "${BASH_VERSINFO[0]}" -ge 4 ]]; then
        words[COMP_CWORD]="$2"
    fi
    COMPREPLY=( $( \
        _CLAP_IFS="$IFS" \
        _CLAP_COMPLETE_INDEX="$_CLAP_COMPLETE_INDEX" \
        _CLAP_COMPLETE_COMP_TYPE="$_CLAP_COMPLETE_COMP_TYPE" \
        _CLAP_COMPLETE_SPACE="$_CLAP_COMPLETE_SPACE" \
        VAR="bash" \
        "COMPLETER" -- "${words[@]}" \
    ) )
    if [[ $? != 0 ]]; then
        unset COMPREPLY
    elif [[ $_CLAP_COMPLETE_SPACE == false ]] && [[ "${COMPREPLY-}" =~ [=/:]$ ]]; then
        compopt -o nospace
    fi
}
if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -o nospace -o bashdefault -o nosort -F _clap_complete_NAME BIN
else
    complete -o nospace -o bashdefault -F _clap_complete_NAME BIN
fi
"#
        .replace("NAME", &escaped_name)
        .replace("BIN", bin)
        .replace("COMPLETER", &completer)
  ```

and this is what is sourced. 

https://github.com/clap-rs/clap/blob/master/clap_complete/src/env/shells.rs

---

_Comment by @Unique-Usman on 2025-08-21 18:47_

an example version created for me

```
_clap_complete_cargo() {
    local IFS=$'\013'
    local _CLAP_COMPLETE_INDEX=${COMP_CWORD}
    local _CLAP_COMPLETE_COMP_TYPE=${COMP_TYPE}
    if compopt +o nospace 2> /dev/null; then
        local _CLAP_COMPLETE_SPACE=false
    else
        local _CLAP_COMPLETE_SPACE=true
    fi
    local words=("${COMP_WORDS[@]}")
    if [[ "${BASH_VERSINFO[0]}" -ge 4 ]]; then
        words[COMP_CWORD]="$2"
    fi
    COMPREPLY=( $( \
        _CLAP_IFS="$IFS" \
        _CLAP_COMPLETE_INDEX="$_CLAP_COMPLETE_INDEX" \
        _CLAP_COMPLETE_COMP_TYPE="$_CLAP_COMPLETE_COMP_TYPE" \
        _CLAP_COMPLETE_SPACE="$_CLAP_COMPLETE_SPACE" \
        CARGO_COMPLETE="bash" \
        "/home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo" -- "${words[@]}" \
    ) )
    if [[ $? != 0 ]]; then
        unset COMPREPLY
    elif [[ $_CLAP_COMPLETE_SPACE == false ]] && [[ "${COMPREPLY-}" =~ [=/:]$ ]]; then
        compopt -o nospace
    fi
}
if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -o nospace -o bashdefault -o nosort -F _clap_complete_cargo cargo
else
    complete -o nospace -o bashdefault -F _clap_complete_cargo cargo
fi
```

---

_Comment by @Unique-Usman on 2025-08-21 18:57_

So any time a completion start with cargo it basically call the above shell function which set some variable and basically the cargo again and passed those variables to it.

The shell is able to know that it should called cargo for completion when the first part of the completion is cargo because of the below script.

```
if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -o nospace -o bashdefault -o nosort -F _clap_complete_cargo cargo
else
    complete -o nospace -o bashdefault -F _clap_complete_cargo cargo
fi
```

for completion instead of cargo to call `shell.write_registration()` it basically call `shell.write_completion `  which take care of the completion.

https://github.com/clap-rs/clap/blob/master/clap_complete/src/env/shells.rs#L72C5-L72C7

From what I understand the problem when the shell sees sudo cargo, it is calling completion for the sudo and not for the cargo, 

One solution I am thinking of is that,

We enhanced the bash script generated by `shell.write_completion` to sort of call completion for cargo when it see that the second argument after `sudo` is `cargo`

something like this will be the crux of the command

```
    if [[ "${words[0]}" == "sudo" && "${words[1]}" == "cargo" ]]; then

```

what do you think ? 

---

_Comment by @epage on 2025-08-21 19:09_

We can't be specializing a solution for `sudo` only.  This is likely a general feature of bash completions which is part of what I was hoping you'd dig into.  We need to understand the principles of how this works to determine what solution we should use.  We also need to look at how other shells handle this situation so we can determine if the solution needs to live in our shell-specific glue code or in our shell-agnostic Rust code.

---

_Comment by @Unique-Usman on 2025-08-22 22:29_

Hi @epage, I did some further research into how the shell completion works for each of the shell,

fish, zsh, bash and Elvish, 

And I tried to replicate the above bugs in the four of them.

Firstly, the bug is not present in fish at all and the above works perfectly but, it is present in bash, zsh and Elvish and this depends on how each of them implement their completion system.

So for Bash,

It uses the first word to know which completion function to call, so if you type something like sudo cargo or env cargo, 

the cargo completion function would not be called at all, it will call the completion for sudo or env or just use the bashdefault.

So, the propose solution here  will be done in shell-specific glue code to use the cargo completion when cargo completion when cargo follows commands like sudo or env. 

Something that was done by python argcomplete does is that they use  the -D flag [Python argcomplete](https://github.com/kislyuk/argcomplete/blob/main/argcomplete/bash_completion.d/_python-argcomplete#L246)  which route all completion which is not explicitely mentioned to a single function. So since sudo is not explicetely mentioned, the functiion is called and it explicitely stripped the sudo before passing it to their python shell-agnostic Python code for completion so, the only solution is in the shell-specific glue code for function detection.

For zsh, 

This automatically will call the correct function even if sudo is put at the front, it will still call cargo but, it would not remove sudo from whatever is  passed to our cargo completion function, so it basically remove sudo from knowing which completion to use but, does not remove sudo when it is passing as an argument to the cargo completion, so what we can do is either handle in the shell specific glue by stripping the sudo before passing it to the cargo completion rust function or handle it in the cargo completion rust function.

https://zsh.sourceforge.io/Doc/Release/Completion-System.html#Completion-of-commands - check for the behaviour in the zsh docs specifically (autoload and precommand) sudo, env and all this sort are handled. 

For Elvish, it also seems to be the same as bash.

But, fish handle evertything from stripping commands like sudo, env for knowing which completion function to call to also passing the args to our cargo function without the sudo or env.

I think one general thing we can do is to handle removing of sudo and env or other things from our shell-agnostic rust code and then in the shell for fish and bash, ensure they both know which completion function to call even if the command start with sudo or env or other but still passed all the args including sudo to the rust agnostic code(which we have wired to remove sudo or other things). 

What do you think ?  



---

_Comment by @epage on 2025-08-25 18:44_

> Firstly, the bug is not present in fish at all and the above works perfectly but, it is present in bash, zsh and Elvish and this depends on how each of them implement their completion system.

Did you verify if you were using built-in fish completions or our custom completions?



> So for Bash,
> 
> It uses the first word to know which completion function to call, so if you type something like sudo cargo or env cargo,
> 
> the cargo completion function would not be called at all, it will call the completion for sudo or env or just use the bashdefault.
> 
> So, the propose solution here will be done in shell-specific glue code to use the cargo completion when cargo completion when cargo follows commands like sudo or env.

When it calls our completions when nesting, what do the various pieces of information passed to us look like?

> Something that was done by python argcomplete does is that they use the -D flag [Python argcomplete](https://github.com/kislyuk/argcomplete/blob/main/argcomplete/bash_completion.d/_python-argcomplete#L246) which route all completion which is not explicitely mentioned to a single function. So since sudo is not explicetely mentioned, the functiion is called and it explicitely stripped the sudo before passing it to their python shell-agnostic Python code for completion so, the only solution is in the shell-specific glue code for function detection.

What was done to confirm it was the `-D` flag that is helping them?  Do you have a more complete description of what the `-D` flag affects?

From my reading of `complete --help` and https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion.html, it sounds like this would make the completions for any `clap_complete` command to be called for *any* command which seems a bit excessive.

> This automatically will call the correct function even if sudo is put at the front, it will still call cargo but, it would not remove sudo from whatever is passed to our cargo completion function, 

Sounds like `_normal` can help with this when used for zsh-only completions.  Might be worth looking at how thats implemented to see if there is logic we can use.

> I think one general thing we can do is to handle removing of sudo and env or other things from our shell-agnostic rust code and then in the shell for fish and bash, ensure they both know which completion function to call even if the command start with sudo or env or other but still passed all the args including sudo to the rust agnostic code(which we have wired to remove sudo or other things).

From what I gather, the only thing we need to do is handle removing the prefix commands and adjusting our indexing.  We shouldn't need to register to complete in more situations, that is what `sudo` and `env`s completers set up in a general way.

---

_Comment by @Unique-Usman on 2025-08-25 22:06_


> 
> Did you verify if you were using built-in fish completions or our custom completions?

Yeah, I did. 

> 
> When it calls our completions when nesting, what do the various pieces of information passed to us look like?

For the example above 

```console
$ cargo build --example ap<TAB> --example banana
```

### For bash 

This is the snippet for completion

```
    COMPREPLY=( $( \
        _CLAP_IFS="$IFS" \
        _CLAP_COMPLETE_INDEX="$_CLAP_COMPLETE_INDEX" \
        _CLAP_COMPLETE_COMP_TYPE="$_CLAP_COMPLETE_COMP_TYPE" \
        _CLAP_COMPLETE_SPACE="$_CLAP_COMPLETE_SPACE" \
        CARGO_COMPLETE="bash" \
        "/home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo" -- "${words[@]}" \
    ) )
```

where `${words[@]}"`  -> `cargo build --example ap --example banana` i.e the exact command we passed. 

and all these are environment variable
```
        _CLAP_IFS="$IFS" \
        _CLAP_COMPLETE_INDEX="$_CLAP_COMPLETE_INDEX" \
        _CLAP_COMPLETE_COMP_TYPE="$_CLAP_COMPLETE_COMP_TYPE" \
        _CLAP_COMPLETE_SPACE="$_CLAP_COMPLETE_SPACE" \
        CARGO_COMPLETE="bash" \
```

`_CLAP_IFS=$'\013'` → Field separator (vertical tab) that Clap uses when printing multiple completions, so Bash can safely split them even if suggestions contain spaces.

`_CLAP_COMPLETE_INDEX=3` → Index of the word being completed (matches COMP_CWORD), here word #3 = "ap".

`_CLAP_COMPLETE_COMP_TYPE=9` → Type of completion requested (how the user triggered completion, e.g. pressing <TAB>).

`_CLAP_COMPLETE_SPACE=false` → Whether Bash should insert a trailing space after a completion (false = don’t insert, e.g. for --flag= style options).

`CARGO_COMPLETE=bash` → Tells Cargo/Clap to run in bash completion mode instead of normal command execution

### For FIsh
```
complete --keep-order --exclusive --command cargo --arguments "(CARGO_COMPLETE=fish "'/home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo'" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```

simply this -> `CARGO_COMPLETE=fish cargo -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token)`

`argv = ["cargo", "build", "--example", "ap"] - `commandline --current-process --tokenize --cut-at-cursor`
`current_token = "ap"`  - `commandline --current-token`

Basically fish does not send all but cut it at the part we are trying to complete.


### For Zsh

Similar to bash

```
    local _CLAP_COMPLETE_INDEX=$(expr $CURRENT - 1)
    local _CLAP_IFS=$'\n'

    local completions=("${(@f)$( \
        _CLAP_IFS="$_CLAP_IFS" \
        _CLAP_COMPLETE_INDEX="$_CLAP_COMPLETE_INDEX" \
        CARGO_COMPLETE="zsh" \
        /home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo -- "${words[@]}" 2>/dev/null \
    )}")
```
```
_CLAP_IFS=$'\n' \
_CLAP_COMPLETE_INDEX=3 \
CARGO_COMPLETE=zsh \
/home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo -- \
    cargo build --example ap --example banana
```

### For Elvish

```
set edit:completion:arg-completer[cargo] = { |@words|
    var index = (count $words)
    set index = (- $index 1)

    put (env _CLAP_IFS="\n" _CLAP_COMPLETE_INDEX=(to-string $index) CARGO_COMPLETE="elvish" /home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo -- $@words) | to-lines
```

expands to something like this.
```
env _CLAP_IFS="\n" _CLAP_COMPLETE_INDEX="5" CARGO_COMPLETE="elvish" \
  /home/uniqueusman/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo \
  -- cargo build --example ap --example banana
```
I tried using elvish, the completion seems not work.

---

> What was done to confirm it was the `-D` flag that is helping them? Do you have a more complete description of what the `-D` flag affects?
> 
> From my reading of `complete --help` and https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion.html, it sounds like this would make the completions for any `clap_complete` command to be called for _any_ command which seems a bit excessive.

Yeah, I will look into this more, but, an approach is to capture sudo and maybe env or others and if cargo follow it, use cargo complete else go leave it to bash to pick the right completion. - This seems to be the only I have seen, I will still check around.

> 
> Sounds like `_normal` can help with this when used for zsh-only completions. Might be worth looking at how thats implemented to see if there is logic we can use.
> 

As I said initially, zsh already shipped with _sudo, _env and all which basically uses _normal, the issue with zsh is that it does not strip sudo or env when passing to our completion function. It strip it to detect which completion function to call. 


> 
> From what I gather, the only thing we need to do is handle removing the prefix commands and adjusting our indexing. We shouldn't need to register to complete in more situations, that is what `sudo` and `env`s completers set up in a general way.

Yeah, exactly, but this only works for all of it , if is done in shell code and not rust. If we do in rust, it would not work for bash and elvish, which does not even call our cargo completion code if the command is not starting with cargo.


---

_Comment by @epage on 2025-08-26 17:32_

> For the example above
> 
> ```console
> $ cargo build --example ap<TAB> --example banana
> ```

That doesn't seem to be a nesting situation.  If this was prefixed with `sudo` then that would work.




---

_Comment by @Unique-Usman on 2025-08-26 22:50_

> > For the example above
> > $ cargo build --example ap<TAB> --example banana
> 
> That doesn't seem to be a nesting situation. If this was prefixed with `sudo` then that would work.

Yeah, I did a little more research and I actually see that, there are actually already implemented solution for this for bash. 

https://github.com/scop/bash-completion

This can be installed on ubuntu and I was able to install it on my Arch Linux and the above bugs disappear after installing it.

It basically have a completion system for sudo, env and many others. So, the logic is something we have been discussing which is too basically check the next word after sudo and strip it when passing to cargo. It add Extend PATH to include /sbin etc., so privileged commands are found.

And I tested the above after installing, the bugs disappear.

---

_Comment by @epage on 2025-08-28 15:23_

Yeah, `sudo cargo <TAB>` works for me as well but the filer of rust-lang/cargo#15862 says its not working.

If the old completions work, it might still be worth finding a way to make this work for greatest compatibility.

---

_Comment by @epage on 2025-08-28 15:29_

Oh right, its not that we aren't completing but that we aren't completing correctly.  I'm able to reproduce the problem where, with `sudo`, arguments get swapped

---

_Comment by @Unique-Usman on 2025-08-28 16:58_

> If

By old and new completing, what do you mean by that  ?

This ->` cargo build --example ap<TAB> --example banana` and this `sudo cargo <TAB>`, works perfectly for me after getting the bash-completions

---

_Comment by @Unique-Usman on 2025-08-28 16:59_

> Oh right, its not that we aren't completing but that we aren't completing correctly. I'm able to reproduce the problem where, with `sudo`, arguments get swapped

This exact `cargo build --example ap<TAB> --example banana` ? How did you reproduce it ?

---

_Comment by @epage on 2025-08-28 17:36_

> This exact cargo build --example ap<TAB> --example banana ? How did you reproduce it ?

I followed the issues reproduction steps

---

_Comment by @Unique-Usman on 2025-08-28 17:48_

hmm, apparently, it works perfectly on my arch linux, I will try to get the same environment as the above.

---

_Comment by @Unique-Usman on 2025-08-28 18:13_

@epage, I just reproduce on Ubuntu, but, it works pefectly on arch linux. I will look into it more again.

---

_Comment by @Unique-Usman on 2025-08-28 19:10_

While looking at the source code for bash completion script in both arch linux and the ubuntu, they are different, the arch linux use the exact source code in the https://github.com/scop/bash-completion and it is different in ubuntu, I will guess the main problem comes from their, I will look into it more to verify

---

_Referenced in [RustBeginners/awesome-rust-mentors#206](../../RustBeginners/awesome-rust-mentors/issues/206.md) on 2025-08-29 05:09_

---

_Comment by @Unique-Usman on 2025-09-08 23:07_

@epage still working on this, go busy with school work recently. Will resume asap.

---
