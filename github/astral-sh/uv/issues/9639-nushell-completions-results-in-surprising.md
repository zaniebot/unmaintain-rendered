---
number: 9639
title: "Nushell: Completions results in surprising expansions of arguments passed to subcommands via `uv run`"
type: issue
state: open
author: jenshnielsen
labels:
  - external
assignees: []
created_at: 2024-12-04T13:42:17Z
updated_at: 2024-12-06T00:29:49Z
url: https://github.com/astral-sh/uv/issues/9639
synced_at: 2026-01-07T13:12:18-06:00
---

# Nushell: Completions results in surprising expansions of arguments passed to subcommands via `uv run`

---

_Issue opened by @jenshnielsen on 2024-12-04 13:42_

I was trying to run a command such as 

```
uv run pyright.cmd .\src\ -p pyright_ci_config.json
```
This fails with 

```
Unexpected option --python.
pyright --help for usage
```

adding verbose and I see

```
DEBUG Running `pyright.cmd --python .\pyright_ci_config.json`
```
It seems that since uv it self has a `-p/--python` arg the `-p` arg to powershell is expanded to `--python` 
A similar effect can be observed with `-m` which is expanded to `--module` if you run `uv run python -m build`

After some debugging, I found out that this only happens in nushell and only if I have source the uv generated completions.

```
uv generate-shell-completion nushell o> uv.nu
```

in config.uv

```
source uv.nu
```


uv 0.5.6 (b70c4f30e 2024-12-03)
nu 0.100.0
Windows 11
 

---

_Comment by @dedebenui on 2024-12-04 15:20_

I run `uv` 0.5.6 and Nushell 0.100.0 on Windows 10. I freshly generated the shell completion and ran
```nushell
uv run pyright.cmd .\src\ -p pyright_ci_config.json
```

Granted, I don't have Pyright in my path, nor is there a `pyright_ci_config.json` in my working directory, but in the logs I get:
```
DEBUG Running `pyright.cmd .\src\ -p pyright_ci_config.json`
```
as expected. Same if I run the command from PowerShell.

It's also funny that the `.\src\` disappears from your logs. Does any of this happen when you run Nushell without config (`nu -n`) ?


---

_Comment by @jenshnielsen on 2024-12-05 10:58_

@dedebenui Thanks for your comments. Yes it does also happen in a fresh shell (`nu -n`)

Specifically I do

```
source nu.env
```

and then

```
uv run --verbose python -m build 
```

or 

```
uv run python -m build 
```
fails with 

```
DEBUG Running `python --module build`
unknown option --module
```

However, running


```
uv --verbose run python -m build 
```
or 

```
uv run python `-m` build 
```
Runs as expected.

Furthermore, the syntax high lighting seems to indicate that the commands are interpreted differently.

Compare

![Image](https://github.com/user-attachments/assets/68cc5200-b3ec-4375-9c09-8a290877b591)

and 

![Image](https://github.com/user-attachments/assets/385cadf3-d306-4771-a431-9ca73a8b84c5)



With respect to `./src` that may have just been missed when I pasted.





---

_Comment by @dedebenui on 2024-12-05 14:35_

Thanks for the additional info. 

It seems that the completion file exports a `uv run` function with a `-m` option. Nushell supports having options provided after the argument (which it recognises as `python`) and so, it priositizes parsing the `-m` as an option of the command named `uv run`. When using `uv --verbose`, it thinks the command is just `uv`, which doesn't have a `-m` option and so everything is passed on without further parsing.

I would try opening an issue in the Nushell repo, as it looks to me that the `export extern` syntax lacks some way to:
1. pass options as-is, without expanding them into long-form options
2. tell it that options come only **before** arguments

I think any level of granularity can be achieved with [context-aware custom completions](https://www.nushell.sh/book/custom_completions.html#context-aware-custom-completions), but it's not easy to implement automatically like [`clap_complete_nushell`](https://github.com/clap-rs/clap/tree/master/clap_complete_nushell) does.

---

_Label `upstream` added by @charliermarsh on 2024-12-06 00:29_

---

_Referenced in [nushell/nushell#14574](../../nushell/nushell/issues/14574.md) on 2024-12-13 09:56_

---
