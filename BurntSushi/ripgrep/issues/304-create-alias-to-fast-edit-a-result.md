```yaml
number: 304
title: Create alias to fast-edit a result
type: issue
state: closed
author: Hywan
labels:
  - question
assignees: []
created_at: 2017-01-06T13:33:14Z
updated_at: 2017-12-19T08:21:38Z
url: https://github.com/BurntSushi/ripgrep/issues/304
synced_at: 2026-01-12T16:13:21Z
```

# Create alias to fast-edit a result

---

_@Hywan_

Hello and thanks for this incredible work!

I am wondering if this would be hard to implement something similar to https://github.com/aykamko/tag? I can try if you want. The principle is simple: Generate one alias per results in a temporary file (so `e1` will open an editor to the first result —file and line—, `e2` to the second result etc.; e.g. `alias e1="$EDITOR +line file"`), source the alias (done by the shell, not by the command itself), that's it.

---

_Comment by @BurntSushi on 2017-01-06 13:38_

I don't think mucking about with shell aliases is something that belongs in ripgrep. It sounds like a maintenance nightmare. Does it work on Windows? Which shells does it support?

I would, however, be amenable to making ripgrep's output format a bit more customizable. However, that will require some refactoring/design work that I'm not quite ready to tackle yet. That particular issue is tracked by #244.

---

_Label `question` added by @BurntSushi on 2017-01-06 13:38_

---

_Comment by @BurntSushi on 2017-01-06 13:39_

Note that it looks like ripgrep support will land in `tag` at some point: https://github.com/aykamko/tag/issues/27

---

_Comment by @lukaslueg on 2017-01-06 15:42_

You can do this using [peco](https://github.com/peco/peco) and a shell function.

Put this in your .zshrc / .bashrc / whatever:
```bash
rg2vim() { vim $(rg -n $1 | peco | awk -F\: '{print "+"$2,$1}') }
```

You can then just call `rg2vim foobar`. Peco will fire up and show you all files that contain `foobar` and the matching line obtained from rg. You can narrow it down within peco. Hit Enter and Vim will fire up at the exact matching line. Hold your breath in awe.

---

_Comment by @lukaslueg on 2017-01-06 17:38_

This has the virtue of opening multiple selected files in individual tabs and skipping peco if there is only one match reported by rg. It does a horrible (read:none) job of escaping special characters though. Maybe someone can come up with something better

```bash
rg2vim() { vim -c "$(rg -n $1 | peco --select-1 | awk -F\: 'BEGIN {ORS="|"}; {print (NR==1 ? "e " : "tabe ") $1"|:"$2}')" }
```

---

_Comment by @Hywan on 2017-01-06 17:39_

Will try to integrate it into emacs. Have been running Vim for 7 years, I did the switch recently (with evil-mode, miam!).

---

_Comment by @Hywan on 2017-01-06 19:54_

peco is a nicer alternative to tag. Thanks for the tips!

---

_Closed by @Hywan on 2017-01-06 19:54_

---

_Comment by @partounian on 2017-12-19 01:59_

EDIT: Moving the chat to Luke's dotfiles. https://github.com/lukaslueg/dotfiles/issues/1

Hey @lukaslueg it's been some time from this thread but I noticed this in your dotfiles
```zsh
rg2vim() {
	vim -c "$(
	FL=0
	while IFS=: read -r fn ln line; do
		if [ "$FL" -eq "0" ]; then
			printf "e %q|" "$fn"
			FL=1
		else
			printf "tabnew %q|" "$fn"
		fi
		printf ":%d|" "$ln"
	done < <(rg -n "$1" | peco --select-1)
	)"
}
```
Is this the latest and greatest solution?

EDIT: Also any way to catch a Ctrl-C and prevent vim from opening?

---

_Comment by @lukaslueg on 2017-12-19 08:21_

I never got around to such details. The "script" does not process Ctrl-C and just falls on it's nose if there is no `rg` or `peco` binary at all. All I do with it is fire `rg` and see if something useful comes out at all and then use `rg2vim !$` to redo the search and load the desired matches into vim.

---
