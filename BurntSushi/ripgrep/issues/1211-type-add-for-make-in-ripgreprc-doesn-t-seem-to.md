```yaml
number: 1211
title: "type-add for make in .ripgreprc doesn't seem to search added files"
type: issue
state: closed
author: mathomp4
labels:
  - invalid
assignees: []
created_at: 2019-03-07T12:35:30Z
updated_at: 2019-03-07T15:42:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1211
synced_at: 2026-01-12T16:13:23Z
```

# type-add for make in .ripgreprc doesn't seem to search added files

---

_@mathomp4_

#### What version of ripgrep are you using?

```
07:23 $ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.13.6 (High Sierra)

#### Describe your question, feature request, or bug.

A model I work on uses GNU Make as a build system, but often has "unique" names for the makefiles including `GNUmakefile_openmp` and `GNUmakefile_rnin`. I'd like to have `rg -t make` search these files. However, it seems like it does not. To wit, in my `.ripgreprc` file I have:
```
--type-add
make:GNUmakefile_openmp, GNUmakefile_r4r8, GNUmakefile_rnin
```
and `rg --type-list` seems to see it:
```
07:27 $ rg --type-list | rg make
amake: *.bp, *.mk
cmake: *.cmake, CMakeLists.txt
make: *.mak, *.mk, GNUmakefile, GNUmakefile_openmp, GNUmakefile_r4r8, GNUmakefile_rnin, Gnumakefile, Makefile, gnumakefile, makefile
qmake: *.prf, *.pri, *.pro
```
Now in a directory I have a good mix:
```
07:27 $ fd GNUmakefile
GNUmakefile
NCEP_bacio/GNUmakefile
NCEP_bacio/GNUmakefile_rnin
NCEP_sfcio/GNUmakefile
NCEP_sigio/GNUmakefile
NCEP_sp/GNUmakefile
NCEP_sp/GNUmakefile_rnin
NCEP_w3/GNUmakefile
NCEP_w3/GNUmakefile_rnin
```
and if I do a `grep -i ranlib` on this directory's `GNUmakefile*`:
```
07:27 $ fd GNUmakefile -X grep -i ranlib {}
NCEP_sigio/GNUmakefile:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
NCEP_sfcio/GNUmakefile:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
NCEP_bacio/GNUmakefile_rnin:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
NCEP_w3/GNUmakefile_rnin:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
NCEP_sp/GNUmakefile_rnin:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
```
However, if I do `rg` in the same place:
```
07:28 $ rg -i ranlib -t make
NCEP_sigio/GNUmakefile
80:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)

NCEP_sfcio/GNUmakefile
81:	$(RANLIB) $(RANLIB_FLAGS) $(LIB)
```

#### If this is a bug, what is the expected behavior?

As `--type-list` says `GNUmakefile_rnin` is a "make" type file, `rg` should have found matches for RANLIB in `NCEP_bacio/GNUmakefile_rnin`, `NCEP_w3/GNUmakefile_rnin`, and `NCEP_sp/GNUmakefile_rnin`.

That said, it's entirely possible I didn't "add" things right in `.ripgreprc`. My full rc is:
```
07:34 $ cat ~/.ripgreprc

# Don't let ripgrep vomit really long lines to my terminal.
--max-columns=150

--type-add
fortran:*.P90, *.cuf, *.CUF

--type-add
make:GNUmakefile_openmp, GNUmakefile_r4r8, GNUmakefile_rnin

# Because who cares about case!?
--smart-case

# Colorize
--colors=path:fg:green
#--colors=path:style:bold
--colors=line:fg:yellow
--colors=line:style:bold

#--line-number-width=5
```

---

_Comment by @BurntSushi on 2019-03-07 13:22_

Thanks for the careful bug report! However, this isn't a bug and this doesn't have anything to do with `.ripgreprc`. Namely, please read the [section in the docs on type filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types) a bit more carefully. Your syntax isn't correct. ripgrep is treating `GNUmakefile_openmp, GNUmakefile_r4r8, GNUmakefile_rnin` as one big glob. Here is the correct way to do it:

```
$ rg --files
GNUmakefile_r4r8
foo
Makefile
GNUmakefile_openmp

$ rg --files -tmake
Makefile

$ rg --files -tmake --type-add 'make:GNUmakefile_openmp'
Makefile
GNUmakefile_openmp

$ rg --files -tmake --type-add 'make:GNUmakefile_openmp' --type-add 'make:GNUmakefile_r4r8'
GNUmakefile_r4r8
Makefile
GNUmakefile_openmp

$ rg --files -tmake --type-add 'make:GNUmakefile_openmp, GNUmakefile_r4r8'
Makefile
```

---

_Closed by @BurntSushi on 2019-03-07 13:22_

---

_Label `invalid` added by @BurntSushi on 2019-03-07 13:22_

---

_Comment by @mathomp4 on 2019-03-07 15:33_

For those who find this in search, as @BurntSushi says, this is correct for `.ripgreprc`:
```
--type-add
make:GNUmakefile_openmp

--type-add
make:GNUmakefile_r4r8

--type-add
make:GNUmakefile_rnin
```
Not all one line.

Well, unless you have a file called `GNUmakefile_openmp, GNUmakefile_r4r8, GNUmakefile_rnin` :)

---

_Comment by @BurntSushi on 2019-03-07 15:42_

If you did want one line, then this should work too:

```
--type-add
make:{GNUmakefile_openmp,GNUmakefile_r4r8,GNUmakefile_rnin}
```

---
