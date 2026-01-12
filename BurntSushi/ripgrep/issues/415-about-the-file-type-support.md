```yaml
number: 415
title: about the file type support. 
type: issue
state: closed
author: finviman
labels: []
assignees: []
created_at: 2017-03-21T07:25:46Z
updated_at: 2021-05-21T14:32:11Z
url: https://github.com/BurntSushi/ripgrep/issues/415
synced_at: 2026-01-12T16:13:21Z
```

# about the file type support. 

---

_@finviman_

Hi, maybe I misunderstand the option , that if the file type is in the list of `rg --type-list`, for example, *.vim, can I use `rg 'xxx' -tvim` to search text in all *.vim?  But now it output **unrecognized file type: vim**.
I'm using the latest version in 2017-03-21

---

_Comment by @BurntSushi on 2017-03-21 10:55_

@FinallyFinancialFreedom Could you please provide a minimal reproducible example? Please show the exact commands you're running, the actual output of each command and the expected output.

ripgrep does not provide a `vim` file type by default. If you're adding the `vim` file type manually, then please consider reading the docs carefully:

>  --type-add <TYPE>...
Add a new glob for a particular file type. Only one glob can be added at a time.
Multiple --type-add flags can be provided. Unless --type-clear is used, globs are added
to any existing globs defined inside of ripgrep.
>
> **Note that this MUST be passed to every invocation of ripgrep. Type settings are NOT
persisted.**


---

_Comment by @finviman on 2017-03-21 11:38_

Maybe I did not make it clearly.
I assumed that all filetypes in the output of `rg --type-list` command can be used after -t parameter, is it right?
`vimscript: *.vim` is in the list, and when i `rg something -tvim` in vim plugins folder,  i got **unrecognized file type: vim.**. There are fzf.vim and vim.vim file under this folder.

---

_Comment by @BurntSushi on 2017-03-21 11:43_

Oh, I see. The type name is on the left hand side. The right hand side are the glob rules. So you would need to use `-tvimscript`. I would be OK with a PR that add a `vim` type as well, since `vimscript` seems annoying to type.

---

_Comment by @finviman on 2017-03-21 11:54_

ok, -tvimscript works fine, `vimscript` is not so intuitive. Whatever, rg is blazing fast. It helps me a lot, thank you for the wonderful job.

---

_Closed by @BurntSushi on 2017-03-21 11:57_

---

_Comment by @BurntSushi on 2017-03-21 11:58_

@FinallyFinancialFreedom Thanks! I added the `vim` file type, so the next release you should be able to do `-tvim`. :-)

---

_Comment by @adam505hq on 2021-05-21 13:12_

@BurntSushi Why does it have to be added everytime ? Is there something more to it than just searching like `*extension`? Perhaps some noise reduction for each extension or something ?

I have quite exotic file types in my project and I would want to use them :)

---

_Comment by @BurntSushi on 2021-05-21 13:16_

> I have quite exotic file types in my project and I would want to use them :)

I don't understand what is preventing you from doing so. Could you please be more specific?

---

_Comment by @adam505hq on 2021-05-21 14:07_

Sorry maybe I am misunderstanding how to use this. 

Is `rg --tpy foo` suppose to be same as `grep -r foo --include=\*.py` ?

If so then, given `.snake` extension, why `rg --tsnake foo` fails with `unrecognized file type: snake` but `grep -r foo --include=\*.snake` doesn't ?


---

_Comment by @adam505hq on 2021-05-21 14:09_

Nevermind I just noticed there is `--type-add` flag :facepalm: Sorry for confusion.

I just thought this would be handled automatically like with `--include` flag in `grep`

---

_Comment by @BurntSushi on 2021-05-21 14:32_

Yes, the type system is meant to group related globs together. For example, if you want to search README files, you might want `{README,readme,README.txt,README.md}`.

ripgrep's approximate equivalent of grep's `--include` flag is the `-g/--glob` flag.

The GUIDE should help here: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs

---
