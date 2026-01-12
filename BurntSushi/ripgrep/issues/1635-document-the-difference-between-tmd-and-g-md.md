```yaml
number: 1635
title: "document the difference between -tmd and -g*.md"
type: issue
state: closed
author: lamyergeier
labels:
  - doc
  - rollup
assignees: []
created_at: 2020-07-04T18:51:42Z
updated_at: 2023-11-25T20:04:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1635
synced_at: 2026-01-12T16:13:23Z
```

# document the difference between -tmd and -g*.md

---

_@lamyergeier_

#### What version of ripgrep are you using?

ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

linuxbrew

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

rg does not respects `.gitignore`. See example below:

#### What is the actual behavior?

```bash
Test-Glob() {
	mkdir -p Temp
	cd Temp || exit
	git init > /dev/null
	cp "../Data/.gitignore" .
	mkdir -p a/.Commit && touch a/.Commit/a.md
	touch a/a.md
	echo "PWD= ${PWD}"
	echo "${Blue:=}------------Cat gitignore--------------------${Normal:=}"
	cat .gitignore
	echo "${Blue:=}------------List files not ignored--------------------${Normal:=}"
	git check-ignore -v ./**/*
	echo "${Blue:=}------------Find--------------------${Normal:=}"
	find . -type f -name "*.md"
	echo "${Blue:=}------------Ripgrep Glob--------------------${Normal:=}"
	rg --glob "*.md" --files
	echo "${Blue:=}------------Ripgrep--------------------${Normal:=}"
	rg --files
	cd ..
	#Trash Temp > /dev/null
}
```

```
PWD= /home/nikhil/Documents/Git/Cs/Architecture/ArchitectureSrc/Hardware/MemoryUnit/Harddisk/FileSystem/File/ListSearch/AckAgRipgrep/Src/Temp
------------Cat gitignore--------------------
# Ignore everything
*

# Unignore all dirs
!*/

# Unignore
!*.md

# Ignore directory
/**/.Commit/**
------------List files not ignored--------------------
.gitignore:8:!*.md      ./a/a.md
------------Find--------------------
./a/a.md
./a/.Commit/a.md
------------Ripgrep Glob--------------------
a/a.md
a/.Commit/a.md
------------Ripgrep--------------------
a/a.md
```

#### What is the expected behavior?

rg should ignore `./a/.Commit/a.md`


---

_Renamed from "rg does not respects `.gitignore`" to "`rg --glob` does not respects `.gitignore`" by @lamyergeier on 2020-07-04 18:57_

---

_Comment by @BurntSushi on 2020-07-05 12:52_

Thank you for the detailed bug report, but this is correct behavior. Glob overrides always have the highest precedent. So if you use `-g '*.md'` and ripgrep would otherwise descend into a directory containing a `.md` file (i.e., the directory itself wasn't ignored), then ripgrep will search ever `.md` file in that directory.

Interestingly, the same is _not_ true for file types. If you use `-tmd` instead for example, then `a/.Commit/a.md` is ignored.

This does seem like something that the docs could be more explicit about, so I'll mark this as a doc bug.

---

_Label `bug` added by @BurntSushi on 2020-07-05 12:52_

---

_Label `doc` added by @BurntSushi on 2020-07-05 12:52_

---

_Label `bug` removed by @BurntSushi on 2020-07-05 12:52_

---

_Renamed from "`rg --glob` does not respects `.gitignore`" to "document the difference between -tmd and -g*.md" by @BurntSushi on 2020-07-05 12:52_

---

_Comment by @lamyergeier on 2020-07-05 14:24_

Hi I knew about `-tmd`, but was using `glob` only because I wanted to search for random extensions (non-standard). 

Not complaining, but I thought rg followed the behavior of `.gitignore`, as you mentioned it here: https://github.com/BurntSushi/ripgrep/issues/689#issuecomment-347173094

> You said: However, if you had read the README or the blog post introducing ripgrep, then it should have been very clear that respecting gitignore by default is a fundamental design decision.


If I am mistaken, I request you to reconsider this as by this we could support random extensions and  also respect `.gitigore`

---

Also please see the following, when I modified .gitignore (Is this consistent with your comment? I am not sure.):

```bash
Test-Glob() {
	mkdir -p Temp
	cd Temp || exit
	git init > /dev/null
	echo '/**/.Commit/**' > .gitignore
	mkdir -p a/.Commit && touch a/.Commit/a.md
	touch a/a.md
	echo "PWD= ${PWD}"
	echo "${Blue:=}------------Cat gitignore--------------------${Normal:=}"
	cat .gitignore
	echo "${Blue:=}------------List files not ignored--------------------${Normal:=}"
	git check-ignore -v ./**/*
	echo "${Blue:=}------------Find--------------------${Normal:=}"
	find . -type f -name "*.md"
	echo "${Blue:=}------------Ripgrep Glob--------------------${Normal:=}"
	rg --glob "*.md" --files
	echo "${Blue:=}------------Ripgrep--------------------${Normal:=}"
	rg --files
	cd ..
}
```

```
------------Cat gitignore--------------------
/**/.Commit/**
------------List files not ignored--------------------
------------Find--------------------
./a/a.md
./a/.Commit/a.md
------------Ripgrep Glob--------------------
a/a.md
------------Ripgrep--------------------
a/a.md
```


---

_Comment by @BurntSushi on 2020-07-05 14:49_

> but I thought rg followed the behavior of `.gitignore`

It does. The fact that rules in a `.ignore` override `.gitignore` does not mean ripgrep doesn't follow the behavior of gitignore. And similarly for the `--glob` flag. ripgrep does _more_ than just gitignore anyway. It also filters out hidden and binary files by default.

> I request you to reconsider this

No, the behavior isn't changing. It would be a breaking change that feels big enough to me that I'm not comfortable with it, and while I see that there are problems with the current behavior, there would also be problems with your proposed behavior. Ultimately, I judged that the status quo was the least bad option and went with that.

> Also please see the following, when I modified .gitignore (Is this consistent with your comment? I am not sure.):

Yes, because `/**/.Commit/**` ends up ignoring `.Commit` itself, which means ripgrep doesn't descend into that directory. Use the `--debug` flag to see how filtering rules are applied:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./a/.Commit: Ignore(IgnoreMatch(Hidden))
```

---

_Comment by @lamyergeier on 2020-07-05 16:52_

Sorry for the confusion: I am really not sure why  `/**/.Commit/**` is ignored in second case but not in the first case?

> For the first case you said:

> Glob overrides always have the highest precedent. So if you use -g '*.md' and ripgrep would otherwise descend into a directory containing a .md file (i.e., the directory itself wasn't ignored), then ripgrep will search ever .md file in that directory.

What i understood (from your comments): If glob finds `*.md` even in a gitignored directory, it will still print it.

Going by this explanation, if glob overrides always has the highest precedent,  then rg should also print `a/.Commit/a.md` in the second case. Isn't it?

---

_Comment by @BurntSushi on 2020-07-05 17:08_

It has to descend into the directory in the first place. Look at the `--debug` output. With your second gitignore file, ripgrep doesn't descend into `a/.Commit` at all. So there is no opportunity for `*.md` to match it.

This is why I wrote, "and ripgrep would otherwise descend into the directory."

---

_Label `rollup` added by @BurntSushi on 2023-11-25 19:12_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
