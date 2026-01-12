```yaml
number: 273
title: "Implement such `--files-from` option"
type: issue
state: open
author: ngirard
labels:
  - enhancement
  - question
assignees: []
created_at: 2016-12-07T16:25:59Z
updated_at: 2023-08-25T17:35:24Z
url: https://github.com/BurntSushi/ripgrep/issues/273
synced_at: 2026-01-12T16:13:21Z
```

# Implement such `--files-from` option

---

_@ngirard_

A recurring workflow of mine is to search within an existing list of files. 

Currently I'm living by
`$(generate list of files) | while read f; do rg pattern "$f"; done`
which is both inconvenient and inefficient.

Ack [does provide](http://beyondgrep.com/ack-2.0/) a `--files-from` option. Implementing it would allow me to type
`rg pattern --list-from <(gen list)`
to fulfill my needs.

---

_Comment by @BurntSushi on 2016-12-07 16:34_

It seems like there are a few ways to do this without building it into ripgrep. Here are a couple:

```
[andrew@Cheetah 273] echo test > foo
[andrew@Cheetah 273] echo test > bar
[andrew@Cheetah 273] echo test > baz
[andrew@Cheetah 273] cat > file-list <<EOF
> foo
> bar
> baz
> EOF
[andrew@Cheetah 273] xargs rg test < file-list
foo
1:test

bar
1:test

baz
1:test
```

```
[andrew@Cheetah 273] rg test $(cat file-list)
bar
1:test

foo
1:test

baz
1:test
```

I don't think either of these approaches is less efficient than what ripgrep would do if it were built-in. The only caveat here is that if your file list is big enough, you'll need to use `xargs`, which will split up the argument list correctly.

Could you explain in more detail why these approaches don't work for you?

---

_Label `question` added by @BurntSushi on 2016-12-07 16:35_

---

_Comment by @ngirard on 2016-12-07 16:48_

Sure. None of your proposed alternatives work with filenames containing spaces.
Try e.g.
```
echo test > "foo a"
echo "foo a" > file-list
xargs rg test < file-list
foo: No such file or directory (os error 2)
a: No such file or directory (os error 2)
```

---

_Comment by @BurntSushi on 2016-12-07 17:02_

I guess the standard solution to that is to delimit your file paths will a NUL terminator (e.g., `find ./ -print0`) and then tell `xargs` to read them using `xargs -0`.

If you aren't generating files with `find` (or some other tool that can be made to emit NUL terminators), then it seems like you should be able to use `xargs -d'\n' rg test < file-list`?

---

_Comment by @ngirard on 2016-12-07 17:57_

Fair enough. So, for the record, instead of the syntax I proposed
`rg pattern --list-from <(gen list)`

I can achieve the same results using
`xargs -d'\n' -a <(gen list) rg pattern`

Not as convenient, but I can very well live with that.
Thanks!

---

_Comment by @BurntSushi on 2016-12-07 18:23_

Yes, I think I'd prefer that at this point.

Popping up a level, do also note that ripgrep provides the `-g/--glob` flag, which allows you to apply ad hoc filters on which files/directories are searched. This obviously only works for simplistic cases where your rules are simple, but it does cover a lot of the simpler uses of `find ./ ... | xargs grep ...`.

---

_Closed by @BurntSushi on 2016-12-07 18:23_

---

_Comment by @kshenoy on 2018-12-14 16:12_

@BurntSushi My use-case is similar to his where I compile a list of files I'm interested in and search only those files instead of letting ripgrep loose on my entire project which would take a lot longer. Like you suggested, I've been using `xargs -d '\n' rg PATTERN < FILELIST`.

Next, I wanted to search only some specific filetypes (say C++ source files) within FILELIST so I tried to add a `-tcsrc` (csrc is a type I created which is defined in ~/.ripgreprc config file) but that doesn't work as ripgrep seems to ignore any glob/type arguments if provided with an explicit list of files to search from. So I ended up doing `xargs -d '\n' rg PATTERN < <(rg '\.(cc|cpp)$' FILELIST)` to pre-process the FILELIST before running.

This is kinda bad as I've defined the csrc type elsewhere but I'm not able to use it in this context. Is there a better way to go about this? It'd be nice if ripgrep filters the list of files provided using the type/glob argument if one is provided eg. `xargs < FILELIST rg -tcsrc PATTERN`

---

_Comment by @BurntSushi on 2018-12-14 16:27_

@kshenoy ripgrep has, and probably always will, explicitly ignore any filtering for file paths that are explicitly given on the command line. I realize that for your particular niche case, this isn't what you want, but to do otherwise would grossly complicate the already complex filtering logic that ripgrep performs. e.g., running `rg foo blah.py` and getting nothing back even if there was a match because `blah.py` is in your `.gitignore` would be quite an egregious UX fail. You might instead argue that file type filtering is different because it's explicitly provided on the command line, but it's still something that violates what is now a pretty iron clad rule: "If you give a file path to ripgrep, it will search it."

> My use-case is similar to his where I compile a list of files I'm interested in and search only those files instead of letting ripgrep loose on my entire project which would take a lot longer.

You might instead consider using a `.ignore` or `.rgignore` file to dictate which files should be skipped when searching your project.

---

_Comment by @kshenoy on 2018-12-14 18:20_

> "If you give a file path to ripgrep, it will search it."

That's a reasonable rule to follow. I agree that doing anything else would involve prioritizing between different ways to include/exclude files. Thanks for the clarification.

> You might instead consider using a .ignore or .rgignore file to dictate which files should be skipped when searching your project.

I did consider doing that. However, we use Perforce at work and it's easier to compile a list to search through using `p4 have ...` than to compile a list to ignore. I opted to create a [wrapper around rg](https://gist.github.com/kshenoy/d2d1c7b19eafb043d36239264e758b62) which adds the --files-from option similar to ack. It may be a little over-engineered :) but it seems to work. Any suggestions for improvement are welcome.

---

_Comment by @fent on 2019-09-27 01:27_

it would be nice to be able to pipe a list of files to ripgrep. right now, I searched for a second pattern in files matching a first pattern with

```bash
rg "pattern2" --files-without-match $(rg "pattern1" --files-with-matches)
```

when it would be nice to do the following because I usually think of the first pattern first
```bash
rg "pattern1" --files-with-matches | rg "pattern2" --files-without-matches
```

although, this use case is unique since I'm using `--files-without-matches`, which doesn't work with xargs since xargs calls a different ripgrep process for each file, and so ripgrep will end up printing a bunch more files than I intended it to

---

_Comment by @habamax on 2020-05-25 09:23_

> 
> Could you explain in more detail why these approaches don't work for you?

What about windows?

Main issue that there is no xargs.

And if you try to add all files to command line:

    rg "pattern" C:/longpath1/file1 C:/longpath2/file2 ... C:/longpath200/file200

then it exceeds maximum command length and doesn't work.

I want to search all vimhelp files provided in vim runtime path and there are a lot of files (including various plugin documentation).


---

_Reopened by @BurntSushi on 2020-11-09 12:27_

---

_Comment by @BurntSushi on 2020-11-09 12:27_

I'm re-opening this because it seems impossible or difficult to work around this when xargs is not present.

---

_Comment by @BurntSushi on 2020-11-09 12:28_

What should the flag name for this be? `--files-from` has been proposed. Is that the best name?

Also, should files specified via this method be subject to smart filtering or globs? Files specified on the command line are not, so I would think these shouldn't be either. That is, files to be searched via this method should act as if they were given on the command line.

---

_Label `enhancement` added by @BurntSushi on 2020-11-09 12:28_

---

_Comment by @ngirard on 2020-11-09 13:32_

> I'm re-opening this because it seems impossible or difficult to work around this when xargs is not present.

That's a great news !

> What should the flag name for this be? `--files-from` has been proposed. Is that the best name?

I proposed `files-from` after Tar:
```sh
man tar|rg -s -A8 -- '-T, --files-from'

       -T, --files-from=FILE
              Get names to extract or create from FILE.

              Unless specified otherwise, the FILE must contain a list of names separated by ASCII LF (i.e. one name per line).  The names read are handled the same way as command line arguments.  They  undergo  quote  removal  and
              word splitting, and any string that starts with a - is handled as tar command line option.

              If this behavior is undesirable, it can be turned off using the --verbatim-files-from option.

              The --null option instructs tar that the names in FILE are separated by ASCII NUL character, instead of LF.  It is useful if the list is generated by find(1) -print0 predicate.
```


> Files specified on the command line are not, so I would think these shouldn't be either.

Seconded.

---

_Comment by @okdana on 2020-11-09 15:31_

`file`, `rsync`, and (as mentioned in the OP) `ack` also use `--files-from`.

`file` basically treats the paths as if they were given on the command line. `rsync` treats them kind of like includes relative to the source directory, but it also applies include/exclude patterns to them. `ack` treats them like how `rg` would treat paths given on the command line — glob patterns and type filters don't apply to them. I guess that's a strong precedent

---

_Comment by @BurntSushi on 2020-11-09 15:36_

@okdana Aye. I also think that if the list of files is given explicitly like this, then users can use other mechanisms of filtering very easily before passing the file to ripgrep. For example, you might use `git ls-files` to get a list of files tracked by git instead of needing to rely on ripgrep's smart filtering.

And also, come to think of it, if we did allow gitignore or other filters to apply to the list of files given, that would probably prevent this feature from being implemented in any reasonable time frame. gitignore matching, for example, is pretty heavily coupled to directory traversal. Applying `-g/--glob` rules would probably be easy though.

---

_Comment by @timotheecour on 2020-11-09 16:52_

* I don't care much about the name of the flag so long these work:
```
find / | rg pattern --file-from
rg --file-from=FILE
```

* `find / | rg pattern --file-from` should start processing right away in a streaming fashion, and not wait for stdin to be closed (likewise with `rg --file-from=FILE`)


---

_Comment by @BurntSushi on 2020-11-09 16:56_

@timotheecour I would expect you to have to write `find / | rg pattern --files-from -`, where the `-` is an idiom for opening stdin.

Executing the search before stdin is closed is interesting. That will require some re-factoring inside ripgrep, since right now, it stores the complete set of paths to search in memory. (Because it was always in memory via CLI arguments.) I agree that streaming is probably the right option, although that may be an enhancement that comes after the initial feature lands, depending on how difficult that refactoring is.

---

_Comment by @timotheecour on 2020-11-09 17:20_

> enhancement that comes after the initial feature lands

totally fine, then let's keep this issue open till then :-)

---

_Comment by @BurntSushi on 2020-11-09 17:31_

No, if that happens, then I'll close this issue and open a new one.

---

_Comment by @vovcacik on 2021-10-29 20:50_

Using xargs adds few seconds to the execution time when the file list contains 20 000 paths. `xargs.exe -a "%tmp%\filelist.txt" -d '\n' rg foobar`. Its fine, but ripgrep itself is way faster on the same directory tree that filelist.txt was generated from. I am using `fzf` to select the files to search in for ripgrep.

---

_Comment by @dgutov on 2022-04-17 02:59_

Another possible motivation is that using `xargs` leads to non-optimal performance. E.g. inside a checkout of [gecko-dev](https://github.com/mozilla/gecko-dev/),

```sh
$ time git ls-files -z | xargs -0 rg symlinks >/dev/null
_______________________________________________________
Executed in    1,99 secs    fish           external
   usr time    2,64 secs  434,00 micros    2,64 secs
   sys time    2,67 secs   88,00 micros    2,67 secs
```

but if I just add `-P4` to `xargs`'s arguments, the performance improves dramatically:

```sh
$ time git ls-files -z | xargs -0 -P4 rg symlinks >/dev/null
________________________________________________________
Executed in  811,66 millis    fish           external
   usr time    3,41 secs      0,00 millis    3,41 secs
   sys time    3,26 secs      1,77 millis    3,25 secs
```

That's a more than 2x improvement.

IIUC the `-P` option is unsafe to use (can lead to broken mixing of outputs), but it should demonstrate that somewhere inside the processing gets parallelized poorly. And it's not like there can be a lot of lock contention during output: the result is only 500 lines for this example.

---

_Comment by @safinaskar on 2023-02-14 14:26_

@BurntSushi , let me share big feedback on using `rg` (especially `xargs rg`). I hope this post will be useful for you and for other people using `xargs rg`. My rg version is 13.0.0.

Several years ago I started writing script called `host-find` for searching my home directory. My task was very special: this script should skip all Chrome profiles (Chrome profile is any directory, which has `NativeMessagingHosts` subdirectory) and skip all git submodules (git submodule is any directory, which has `.git` regular file as opposed to `.git` directory). (Of course, this is very special task, so I'm not asking for adding this to rg.)

So I wrote C++ program called `my-find` which prepares file list I need. And then I wrote bash script `host-find`, which essentially does `my-find ... | xargs ... grep --color=always ... | less -R` (actual command line is bigger, of course).

And this `host-find` did its functions well for some years. But yesterday I decided to speed up it, so I decided to replace grep with rg.

First of all I needed to know whether `rg` can read file list from stdin (so that I can remove `xargs`). I found this bug report (i. e. https://github.com/BurntSushi/ripgrep/issues/273 ), so it seems it cannot. So I decided to use `xargs rg`.

Then I needed to know what rg options make rg fully compatible with grep (so that I can simply replace `grep` with `rg` in my command line). I have read README, FAQ and `rg --help`. All this texts (at first sight) say that `rg -uuu` is similar to `grep -r`. But I kept reading and doing experiments and found that this is lie! It turned out that `grep` always keeps order of files supplied as arguments intact. But `rg` can reorder them. (And I need predictable order!). So:

**Bug # 1**. Docs may be interpreted as saying that `rg -uuu` is equivalent to `grep -r`, but this is not true.

It would be great if `rg` docs will present some `rg` command, which will be fully compatible with `grep`.

Moreover:

**Bug # 2**. I was unable to find a way to keep argument order intact. `rg -j 1` seems to work, but this is not documented.

Fortunately, `my-find` outputs files in sorted order, so I simply added `--sort=path` to `rg` invocation. But here lies another problem: what exactly "sorted" means? `my-find` employs very particular sorting order: is splits path to parts and then sorts resulting word arrays lexicographically. In other words, `my-find` sorts path in this order:

```text
a/a
a+
a0
```

What order `rg --sort=path` uses? (Remember that I use `xargs rg`, so `rg` may be invoked many times!) If it uses different order, this will mean that files will be sorted using one method inside one `rg` invocation, but sorted using different method between `rg` invocations (inside one `xargs` invocation).

So I did experiments and fortunately I found that `rg --sort=path` sorting is compatible with `my-find` sorting. But this is not documented and I reported this separately. So:

**Bug # 3**. I reported it here: https://github.com/BurntSushi/ripgrep/issues/2418

---

When `rg` output is redirected and you use `-B` and `-A` options (it is redirected to `less` in my case and I actually use `-B` and `-A`), `rg` inserts `--` between found matches. But `--` is not inserted between different invocations of `rg` inside one `xargs` invocation. But I found solution: just add `--heading`. So:

**Advice for xargs users**. Add `--heading`.

But what if particular `rg` invocation will get exactly one file? Then heading will not be displayed. So:

**Advice for xargs users**. Add `/dev/null`. I. e.: `xargs ... rg "$REGEX" /dev/null`. Heading will be always displayed.

---

Some my files have actual `--` in them. So it is impossible to distinguish real "--" with rg-generated one (when using `-B`, `-A` and `--heading`). Because rg-generated `--` is colorless even when I pass `--color=always`. So:

**Bug # 4**. `--` is colorless even in color mode

I found a workaround:

**General advice**. Pass `--line-number` when using `-B` (or `-A`) and `--heading`

---

I have read rg changelog and found that rg sometimes does breaking changes. So I added to my script this:
```bash
if [ "$(rg --version | head -n 1)" != "ripgrep 13.0.0" ]; then
  echo "${0##*/}: rg problems" >&2
  exit 1
fi
```

---

Actual `rg` invocation in my `host-find` script looks similar to this:
```bash
my-find ... | { xargs -d '\n' --no-run-if-empty -- rg -uuu --heading --color=always --no-messages -B 10 -A 10 --no-config --sort=path --line-number -- "$REGEX" /dev/null || :; } | less -R
```

Notes:
- `-d '\n'` mean that `xargs` should not apply special processing to its input. `xargs` should simply split lines and do nothing else
- `--no-run-if-empty` - it seems this option is not needed, because I already passed `/dev/null` to `rg`. But I pass `--no-run-if-empty` just in case
- It seems `-uuu` is not needed, because I pass files themselves to `rg`. But, again, I pass `-uuu` just in case
- I passed `--no-messages`, because sometimes I have no read permission
- I pass `--` before `"$REGEX"`, because regex may begin with `-`
- If any `rg` invocation doesn't find matches, `rg` will return 1. Thus `xargs` will return non-zero. And my script will fail, because it has `set -e`. So I added `|| :`

---

So I eventually overcame all `xargs rg` quirks. I actually got speed up. :) New version of `host-find` works for 10.48 s on my data. Previous (grep-based) version worked 60.53 s on same data. (`rg` is single threaded because of `--sort=path`, so multi threaded version will be even faster.)

All these bugs are subjective, so I didn't report most of them as separate reports. But if you ( @BurntSushi ) want, I will do this

---

_Comment by @BurntSushi on 2023-02-14 14:44_

Thanks for the feedback but in the future, please just file a new issue instead of attaching to an existing one. The vast majority of your comment is irrelevant to his issue.

Also, most of the problems you ran into are also quirks of grep. The `--` issue for example and the heading issue. You could just do `--no-heading` and get grep output format, for example.

To comment specifically on a couple things...

> **Bug # 1**. Docs may be interpreted as saying that `rg -uuu` is equivalent to `grep -r`, but this is not true.

The README says, "Automatic filtering can be disabled with rg -uuu." And that's not a lie. And the docs for the `-u` flag say:

```
    -u, --unrestricted
            Reduce the level of "smart" searching. A single -u won't respect .gitignore
            (etc.) files (--no-ignore). Two -u flags will additionally search hidden files
            and directories (-./--hidden). Three -u flags will additionally search binary
            files (--binary).

            'rg -uuu' is roughly equivalent to 'grep -r'.
```

And in context, that is absolutely correct. It obviously doesn't mean that `rg -uuu` is [precisely equivalent to `grep -r` in literally every possible way](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever). It means that ripgrep will search the same set of files.

> It would be great if `rg` docs will present some `rg` command, which will be fully compatible with `grep`.

It never will because ripgrep never was, is or will be fully compatible with grep. Once again: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever

> **Bug # 2**. I was unable to find a way to keep argument order intact. `rg -j 1` seems to work, but this is not documented.

It's not documented because it's not guaranteed. [No such documentation exists for grep either.](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html)



---

_Comment by @eyalk11 on 2023-08-25 17:35_

Another motivation is that using it inside vim with git ls files is really cumbersome and not os dependent. 

---
