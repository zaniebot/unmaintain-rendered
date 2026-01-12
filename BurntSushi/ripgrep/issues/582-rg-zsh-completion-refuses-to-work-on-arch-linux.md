```yaml
number: 582
title: _rg zsh completion refuses to work on Arch Linux
type: issue
state: closed
author: ishitatsuyuki
labels: []
assignees: []
created_at: 2017-08-24T09:49:45Z
updated_at: 2017-08-24T10:27:19Z
url: https://github.com/BurntSushi/ripgrep/issues/582
synced_at: 2026-01-12T16:13:22Z
```

# _rg zsh completion refuses to work on Arch Linux

---

_@ishitatsuyuki_

```
zsh 5.4.1 (x86_64-unknown-linux-gnu)
```

```
$ rg <tab>
_rg: parse error near `)'
_rg: parse error near `)'
```

Side note: I have [zim](https://github.com/Eriner/zim) installed.

---

_Comment by @okdana on 2017-08-24 10:06_

I don't have Arch, but i can't seem to replicate with zsh 5.4.1 on macOS. Never used ZIM before, but i tried sourcing its template files as mentioned in the `README` and i don't seem to have any troubles there either.

Do you know if you're using the completion function included with `rg` 0.6.0 (released last night), or is it the older one?

Can you do the following please?

1. `autoload -Uz +X _rg`
2. `typeset -f _rg` -> paste the output into a Gist and link here
3. `typeset -ft _rg`
4. `rg <tab>` -> paste the output into another Gist and link here


---

_Comment by @ishitatsuyuki on 2017-08-24 10:09_

```
 ishitatsuyuki@X1Yoga  ~  autoload -Uz +X _rg
zsh: parse error near `)'
 ✘ ishitatsuyuki@X1Yoga  ~  typeset -f _rg
_rg () {
	# undefined
	builtin autoload -XUz
}
 ishitatsuyuki@X1Yoga  ~  typeset -ft _rg
 ishitatsuyuki@X1Yoga  ~  rg
(no error, silence)
```

I have just updated to the latest Arch package and did a full reload of zsh with `exec zsh`.
The full list of files:
```
ripgrep /usr/
ripgrep /usr/bin/
ripgrep /usr/bin/rg
ripgrep /usr/share/
ripgrep /usr/share/bash-completion/
ripgrep /usr/share/bash-completion/completions/
ripgrep /usr/share/bash-completion/completions/rg
ripgrep /usr/share/doc/
ripgrep /usr/share/doc/ripgrep/
ripgrep /usr/share/doc/ripgrep/README.md
ripgrep /usr/share/fish/
ripgrep /usr/share/fish/completions/
ripgrep /usr/share/fish/completions/rg.fish
ripgrep /usr/share/licenses/
ripgrep /usr/share/licenses/ripgrep/
ripgrep /usr/share/licenses/ripgrep/COPYING
ripgrep /usr/share/licenses/ripgrep/LICENSE-MIT
ripgrep /usr/share/licenses/ripgrep/UNLICENSE
ripgrep /usr/share/man/
ripgrep /usr/share/man/man1/
ripgrep /usr/share/man/man1/rg.1.gz
ripgrep /usr/share/zsh/
ripgrep /usr/share/zsh/site-functions/
ripgrep /usr/share/zsh/site-functions/_rg
```

Also worth to mention that zim enables `bashcompinit` as well.

---

_Comment by @okdana on 2017-08-24 10:19_

Hm, i guess that makes sense if the file itself is broken.

`print -l ${^fpath}/**/_rg(#qN)` should tell you where the file is; can you paste the command's output as well as the contents of the file(s) it lists?

---

_Comment by @ishitatsuyuki on 2017-08-24 10:22_

Yeah, you were correct. The packager missed something:
```
install -Dm644 "target/release/build/ripgrep-"*/out/_rg.ps1 "$pkgdir/usr/share/zsh/site-functions/_rg"
```

Uh, so I will overwrite with the version in the repository for the meanwhile. ~I cannot find the ps1 file inside repo either, is that removed or where is that generated from?~ Figured out, it's clap's autogenerate.

---

_Comment by @ishitatsuyuki on 2017-08-24 10:24_

Anyway, closing.

---

_Closed by @ishitatsuyuki on 2017-08-24 10:24_

---

_Comment by @okdana on 2017-08-24 10:27_

Oh, i don't know what the deal with that is. The `.ps1` file is a completion function for PowerShell, it has nothing to do with zsh. It's auto-generated during the build by `clap`. 🤷‍♀️ 

---
