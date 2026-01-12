```yaml
number: 169
title: "Feature request: --directories"
type: issue
state: closed
author: HerrmannM
labels: []
assignees: []
created_at: 2016-10-11T22:00:10Z
updated_at: 2017-10-17T03:35:21Z
url: https://github.com/BurntSushi/ripgrep/issues/169
synced_at: 2026-01-12T16:13:21Z
```

# Feature request: --directories

---

_@HerrmannM_

Hi there,

I read the blog http://blog.burntsushi.net/ripgrep/#gathering-files-to-search and it seams that the ripgrep directory traversal is very efficient (http://burntsushi.net/rustdoc/walkdir/). This could be used for a feature like `--files`, but for directories (while coping with the ignore rules , e.g. r`g --hidden --directories`).

Thank you for that amazing tool,
Kind regards

EDIT: formatting 


---

_Comment by @BurntSushi on 2016-10-11 22:02_

What is the use case?

`--files` is a secondary feature of `ripgrep`, and its primary purpose is to tell you which files `ripgrep` will search.


---

_Comment by @HerrmannM on 2016-10-11 22:18_

It can be used in the same way as `--files`, i.e. by showing which directories are searched.

I also use the `--hidden --files` to create files listings as it is far more efficient than `ag -g ""`. The same could benefit directories listings, as I think we are stuck with `find -d`. And in combination with the `-g` option, it could be a `find -d` on steroids (I understand that this is not the primary purpose of `rg`, but it could be an other nice way to use that amazingly fast lib!). 


---

_Comment by @BurntSushi on 2016-10-11 22:20_

I'm kind of wishy washy on this. What I'm looking for is a use case. i.e., You run `rg --directories` and what do you actually do with the output? For example, would something like `rg --files | xargs dirname | sort -u` do what you need?


---

_Comment by @HerrmannM on 2016-10-11 22:42_

Indeed, `rg --hidden --files --null | xargs -0 dirname | sort -u` does the trick, but it is less efficient than `find . -type d` (according to a quick and dirty `time` test in my home, `find` is 5 times than the `rg-xargs-sort` trick,  which is in fact not a surprised ;-) ).

I generally feed such output to fuzzy searchers such as `fzf` or `ctrlp-p` (so speed is the key here) or I use them for creating folder listing for backuping tools.


---

_Comment by @BurntSushi on 2016-10-11 22:50_

Is `rg --files` all by itself faster than `find`? I wouldn't expect so. `rg` has to fundamentally do quite a bit more work (processing ignore files).


---

_Comment by @HerrmannM on 2016-10-11 23:09_

Indeed, it isn't. It is around 2.5 times slower than `find`.
For info, `find` finds 25000 directories in around 200ms, while `rg` finds 129000 files in around 500ms, so it is 5 more items for only 2.5 the time.
`find . -type f` finds 131000 files in around 300ms, which is similar to `rg -uuu --files`.
I guess that if one is interested in a raw listing of files, `find` and `rg` are equivalent, and it may be the same for the directories. However, filtered listings (especially with the ignore files) are much more interesting. But I recognize that a `find . -type d | grep ...` will probably do the trick quite efficiently.


---

_Comment by @BurntSushi on 2016-10-11 23:31_

> find and rg are equivalent

This is my expectation. I actually [wrote `walkdir`](https://github.com/BurntSushi/walkdir) and specifically benchmarked it against GNU find and `nftw`. It's of comparable speed. :-)

> However, filtered listings (especially with the ignore files) are much more interesting. But I recognize that a find . -type d | grep ... will probably do the trick quite efficiently.

Right, but the `dirname | sort -u` trick should also work. Perhaps the `sort` makes things a bit too slow on 100,000+ files though.


---

_Comment by @HerrmannM on 2016-10-12 08:50_

Well, `dirname | sort -u` does not give us a filtered listing, only the directories, so we have to filter it one more time through a `grep`, or `rg`.
For example, searching hidden folders is quiet efficient with `find . -type d | rg -N '^\./\.'` (in fact, same perf as `grep` according to my quick tests, which is your expectation ;-) ). However, we won't be able to deal easily with ignore files, which is a strength of `rg`.

EDIT :
`rg --hidden --files --null | xargs -0  dirname | uniq` is faster (1.4s to 0.5s)  than `rg --hidden --files --null | xargs -0  dirname | sort -u` (but still 2 times slower then `find . -d`)


---

_Comment by @BurntSushi on 2016-10-13 10:45_

If `rg --hidden --files --null | xargs -0 dirname | uniq` is only 2 times slower than `find . -d`, then I'm inclined to say that's good enough.

With that said, I am currently working on moving all of the ignore code out into its own library. When that's done, it would be relatively straight-forward to build your own tool that searches files/directories and respects ignore rules.

I think at the end of the day, the `--files` flag is a nice convenience, but it's a non-goal for `rg` to become a `find` replacement. If we add `--directories`, what's to stop us from adding everything else in GNU find as well? I don't want to do that.


---

_Closed by @BurntSushi on 2016-10-13 10:45_

---

_Comment by @dieggsy on 2017-03-03 03:12_

I know this is rather old, but I just wanted to comment that the ripgrep file search _and_ the directory workaround search are both faster than find for me, on `macOS 10.12.13` with `ripgrep 0.4.0`.

---

_Comment by @BurntSushi on 2017-03-03 11:55_

@therockmandolinist Yeah, since my earlier comments the directory iterator in ripgrep became parallelized, so it is indeed possible that it is faster than `find`. (Although in my own experiments, it is only nominally faster.) The important benefit of parallelization is that processing the gitignores is parallelized, which is actually quite compute intensive.

---

_Comment by @braham-snyder on 2017-07-01 19:20_

A note for posterity using OS X: if you get `usage: dirname path` with the commands above, try using the GNU coreutils version of `dirname` (installable with Homebrew -- then available by default at `gdirname`)

---

_Comment by @chrisjohnson on 2017-09-29 20:23_

Thanks @braham-snyder for the tip, that helped. I also found that uniq requires duplicates to be adjacent to one another and I was getting many duplicates with this call. You can solve that with `|sort |uniq` but then it won't stream lines as it discovers them (and also takes 4x as long).

So I found an awk snippet which does uniq's functionality for non-adjacent matches and has a happy benefit of being even faster than `|uniq`: `| awk '!h[$0]++'`

So combining the two tips above I can now use rg to get dir globbing that respects my `.ignore` for fzf with the following .fzf.zsh:

```
if [[ $(uname) == "Darwin" ]]; then
	# dirname on OS X behaves funky, get gdirname via
	# brew install coreutils
	export dirname_command="gdirname"
else
	export dirname_command="dirname"
fi
_fzf_compgen_dir() {
	rg --hidden --files --null "$1" 2>/dev/null | xargs -0 "$dirname_command" | awk '!h[$0]++'
}
```

---

_Comment by @okdana on 2017-09-29 20:55_

This is more idiomatic, more portable, and possibly faster:

```
_fzf_compgen_dir() {
  print -rl -- ${(u)${(f)"$( rg --hidden --files $1 2> /dev/null )"}:h}
}
```

zsh only, obv

edit: Actually, i'm sorry, i over-looked that you wanted the 'streaming' behaviour — in that respect it may not meet your needs.

---

_Comment by @chrisjohnson on 2017-09-29 21:02_

@okdana While it does run slightly faster, it has to wait until the full list is generated to return, so it suffers the same downside as `|sort|uniq`. Given that my intention is to use this for FZF which allows you to start filtering straight away that makes it not a viable option, unfortunately. Thanks though!

*Edit* It also uses a huge amount of memory

---

_Comment by @okdana on 2017-09-29 21:03_

Yeah, i caught that right after i submitted it. Sorry, wasn't paying enough attention.

---

_Comment by @braham-snyder on 2017-09-30 00:42_

@chrisjohnson `sort -u` does not block for me, but I too see the speed-up when using that `awk` snippet (given enough files/dirs) -- thanks!

---

_Comment by @partounian on 2017-10-17 02:48_

Sorry if this is a dumb question, but seeing as some of you use FZF, what is the exact function/command alias you guys use for fzf + rg directory traversal?

My current fzf aliases are (hoping this might help others):
```zsh
export FZF_DEFAULT_COMMAND='rg -L --files --hidden'

# fe [FUZZY PATTERN] - Open the selected file with the default editor
#   - Bypass fuzzy finder if there's only one match (--select-1)
#   - Exit if there's no match (--exit-0)
fe() {
  local files
  IFS=$'\n' files=($(fzf-tmux --query="$1" --multi --select-1 --exit-0))
  [[ -n "$files" ]] && ${EDITOR:-vim} "${files[@]}"
}

# fd [FUZZY PATTERN] - Open the selected folder
#   - Bypass fuzzy finder if there's only one match (--select-1)
#   - Exit if there's no match (--exit-0)
fd() {
  local file
  local dir
  file=$(fzf +m -q "$1") && dir=$(dirname "$file") && cd "$dir"
}

if [[ $(uname) == "Darwin" ]]; then
	# dirname on OS X behaves funky, get gdirname via
	# brew install coreutils
	export dirname_command="gdirname"
else
	export dirname_command="dirname"
fi

# Faster compgen
_fzf_compgen_dir() {
	rg --hidden --files --null "$1" 2 > /dev/null | xargs -0 "$dirname_command" | awk '!h[$0]++'
}
```

I would love for "fd" to be like fe except only show dir names, instead of dir and file names.

---

_Comment by @chrisjohnson on 2017-10-17 03:34_

https://github.com/junegunn/fzf#settings

Just worked with the author of fzf to finalize this suggested fix. If you look at my PR [here](https://github.com/junegunn/fzf/pull/1083) you can see a non-ruby version but honestly the ruby version is much better because it walks up the dir chain for every file to ensure the path is added all the way up, plus it's faster than the awk approach.

---
