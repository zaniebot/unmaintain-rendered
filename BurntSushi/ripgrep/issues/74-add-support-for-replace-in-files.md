```yaml
number: 74
title: Add support for replace in files
type: issue
state: closed
author: kfir-drivenets
labels: []
assignees: []
created_at: 2016-09-25T06:30:32Z
updated_at: 2025-10-02T07:36:56Z
url: https://github.com/BurntSushi/ripgrep/issues/74
synced_at: 2026-01-12T16:13:21Z
```

# Add support for replace in files

---

_@kfir-drivenets_

One of the usecases that many people have is a simple search and replace in multiple files.
Usually I achieve this functionality with `xargs sed` for example with `ag -l 'hello' | xargs sed -i "s/hello/bye/g`. 
Since rg contains the replace flag it will be nice to add support for this feature by replacing in the files directly. 

This will be a nice addition after https://github.com/BurntSushi/ripgrep/issues/26

An example on how to do this with `sed` can be found in the comments here.
https://github.com/BurntSushi/ripgrep/issues/74#issuecomment-376659557


---

_Comment by @BurntSushi on 2016-09-25 13:21_

I don't think I'd be willing to do this. It would change rg from a tool that will never modify your files to one that will modify your files. It is also a very complicated feature to get right to make sure you don't really mess up files on disk.

Just use `sed`.


---

_Closed by @BurntSushi on 2016-09-25 13:21_

---

_Comment by @samuelcolvin on 2016-10-04 12:44_

Real search and replace would be an absolutely excellent feature.

But I can well believe it would be painful and complex to implement.

Perhaps there could be a reasonable middle way: if `rg` could output in `.patch` file based on the replace argument the user could then review the patch and apply it when they're happy.

Something like:

``` shell
rg '(foo) (bar)' --replace '$2 $1' --diff-output > rg.diff
cat rg.diff
...
patch < rg.diff
```

That has the advantage that the user couldn't easily mess up a directory by happening to add an argument and the business of modifying files is left to another command.


---

_Comment by @BurntSushi on 2016-10-04 12:51_

@samuelcolvin Why would someone do that over using `sed`?

I guess your suggestion does satisfy one of my primary concerns, but it doesn't seem like good UX to me.


---

_Comment by @samuelcolvin on 2016-10-04 13:01_

Because for me (and lots of other developers) `sed` is a complete anathema, it's not at all easy to get started with or reason with and it's documentation doesn't help much. I'm also not sure how well it deals with unicode.

Most people seem to open up sublime or atom or their IDE when they want to do a search and replace. For me that says a lot about the dearth of decent tools in the terminal for this.

I agree the UX isn't great but since some kind of review step is likely to be required anyway, it couldn't be that much more succinct. If you know what you want you could of course pipe the output of `rg` straight into patch.

(by the way, I'm a big fan of `rg`, sorry to sound like I'm demanding lots highly complex features. The current tool is already great.)


---

_Comment by @BurntSushi on 2016-10-04 13:36_

@samuelcolvin Thanks for explaining. I can appreciate that `sed` might be an opaque tool, however, its functionality is vast and I really don't want to start down the path of being a `sed` replacement. The patch idea is clever, but I don't think it's a particularly good fit for `ripgrep`.

With all that said, I am actively working towards moving more of `ripgrep` out into libraries (a lot of it already is), so if someone wanted to build a `sed` replacement down the line using the same components that make `ripgrep` work, I think that would actually be a feasible thing to do.


---

_Comment by @leeoniya on 2016-10-05 14:37_

Sorry if this is a stupid question, but what's the use case for `--replace` if it has no way to provide actionable output?


---

_Comment by @BurntSushi on 2016-10-05 14:51_

Why is the output of `ripgrep` not actionable? It could be used to create a new file, for example. It could also be used to pipe input into other line-oriented command tools.


---

_Comment by @leeoniya on 2016-10-05 15:28_

nvm, it looks good, now that i actually tried it.

sorry for the noise :)


---

_Comment by @despairblue on 2016-10-06 14:31_

How about an option to print out the whole file? Than I can simply pipe it over the file if I wish to.

That would make this easier: `rg -C 9999999 -N '(57ed11b0add205adee02a9bd)' --replace '\'$1\'' file.js | sponge file.js`

This can also work for multiple files:

``` fish
for f in (rg -l '(57ed11b0add205adee02a9bd)')
  rg -C 9999999 -N '(57ed11b0add205adee02a9bd)' --replace '\'$1\'' $f | sponge $f
end
```

(that's fish, not bash, but differences should be subtle)

---

by the way this was easier to come up with than googling and reading `sed` tutorials, I tried...


---

_Comment by @luisbelloch on 2016-10-07 09:11_

I like @samuelcolvin suggestion on producing diffs, looks like a good compromise.

The problem using `sed` is that we've to search inside the file again, so `rg` is only useful to scan the files _quickly_. For instance, redacting a bunch of IPs from files results in a quite cumbersome expression:

```
rg -e "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}" -c | \
  cut -d":" -f1 | \
   xargs sed -b -i "s/\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}/REDACTED/g"
```

In any case, knowing that `rg` has already opened the file and can do replacements with `--replace` I don't fully understand the negative of adding an option to make inline replaces (I might be omitting implementation reasons, probably).


---

_Comment by @BurntSushi on 2016-10-07 10:11_

Let me say this: IMO, the cumbersomeness of `sed` is not a reason for `rg` to support basic replacement. The cumbersomeness of `sed` is a reason to _go out and build a replacement for `sed` itself_. We've already covered the point that some people find `sed` hard to use. I understand and acknowledge that, but _I do not want to get into the business of replacing `sed` at the point in time_. Adding simple inline replacements is going to open the flood gates for features requests like, "`sed` supports doing X, please add it to `ripgrep`." I do not want to open those flood gates.

> In any case, knowing that rg has already opened the file and can do replacements with --replace I don't fully understand the negative of adding an option to make inline replaces (I might be omitting implementation reasons, probably).

`rg` is a search tool that will never touch your files. Inline replacements means it becomes a search tool that will sometimes mutate your files. This is a huge change because `rg` now has to be very careful when it starts mutating files. It's a _completely different implementation path_ than searching and requires being absolutely sure that you don't mess up the end user's data. (What happens if the end user Ctrl-C's in the middle of writing a file?)

I, as the maintainer of this project, do not want to go down this road. I admit that the patch idea is a clever compromise, but I find the the UX to be very bad personally. It also doesn't stop the floodgates from opening to start supporting every nook and cranny of `sed` itself.

I suggest we drop this and wait for the components of `ripgrep` to get put into a library so that someone can build a better `sed`.


---

_Comment by @luisbelloch on 2016-10-09 08:15_

I agree with your point, making a `sed` replacement is a different business. However, the `--replace` flag suggest a different thing, the purpose is a bit confusing.

Better to wait for `libripgrep`, is that happening in a branch or another repo? I'm interested to contribute to it.


---

_Comment by @OJFord on 2016-10-09 16:59_

I second the statement that, given this, `--replace` is confusing.

I found this thread trying to find out how to 'accept' the changes, after realising it output what I thought was a confirmation.

I'm sure it's possible to pipe line numbers and changes to some other utility to apply the replacement, but I agree that it's non-obvious, and it would be good to be able to say in `rg --help | rg -A2 replace` that this can be done "by piping to `command`".


---

_Comment by @BurntSushi on 2016-10-11 00:44_

> Better to wait for libripgrep, is that happening in a branch or another repo? I'm interested to contribute to it.

A lot of it is already done, but there is no canonical "libripgrep." I wrote up more details in #162.

I've updated the documentation of `--replace`.


---

_Comment by @ticki on 2016-10-14 17:40_

@BurntSushi Would you be open to PRs implementing this functionality?


---

_Comment by @BurntSushi on 2016-10-14 17:48_

@ticki Not really. I don't think any of my objections to this feature were related to my personal unwillingness to implement it initially.

I am making good progress on #162. I'd rather see this functionality built out in a separate tool. My hope is that #162 makes this actually feasible to do without becoming an expert in text search.


---

_Comment by @lydell on 2017-06-17 13:02_

I made a little [bash script](https://github.com/lydell/dotfiles/blob/d279dedf4904687bd537ee33a4146715abe0ba14/bin/pre) for using ripgrep as a command-line find-and-replace tool. It might be terrible. But it works great for my use cases so far. (Note: You need to replace `(e` with `(rg` on line 21.)

The idea is to first search and see what matches:

```
rg something
```

Then, press the Up arrow and edit in a replacement:

```
rg something --replace SOMETHING
```

If it looks good, press the Up arrow again and add a `w` at the beginning (w for write) (although my script is called `pre`, not `wrg` for historical reasons):

```
wrg something --replace SOMETHING
```

Voila! The changes should be written to disk.

(I'm not saying that ripgrep should add a feature for "writing the replacements" – I'm just sharing that a couple of lines of bash can be enough :) )

(Edit: macOS support thanks to the next comment.)

(Edit: Warning – there seems to be some bug that corrupts your file if `-U/--multiline` is enabled. Edit again: Thought it was an issue with my script, but it’s this issue: https://github.com/BurntSushi/ripgrep/issues/2095)

---

_Comment by @greglearns on 2017-12-25 15:18_

For those on MacOSX who try out [lydell's bash script solution](https://github.com/lydell/dotfiles/blob/330e9245ba8f820e2459a876d31cad78a49215b3/bin/pre), which does not support "head -n -1" (get all except the last line); you can replace "head -n -1" with " sed '$d' " (take the input and delete the last line).

Thank you BurntSushi for ripgrep, and lydell for the simple solution.

---

_Comment by @z2oh on 2018-03-27 20:14_

This is more easily accomplished using `sed` than I think people realize. `sed` is quite a beast and is not very user friendly, but simple regex find and replace is easier than one may think. To perform find and replace in a directory (using `ripgrep` to find files with matches) you can do something akin to the following:

(GNU sed)
```
rg 'foo' --files-with-matches | xargs sed -i 's/foo/bar/g'
```

(BSD sed) <-- this includes OSx
```
rg 'foo' --files-with-matches | xargs sed -i '' 's/foo/bar/g'
```

(keep in mind here that `foo` can be a regex, as you would expect for `ripgrep`)

I'll break down what is happening here:

`rg 'foo'` looks for matches with `foo`. By using the `--files-with-matches` flag, we only print out the filenames for files that have matches, but none of the actual matches themselves. We pipe this output (which is a list of matching filenames) into `xargs` which essentially splits our list of filenames from standard in into individual command line arguments for the next program we execute. This program happens to be `sed`. The `-i` flag to `sed` tells `sed` to edit files in place. On BSD sed, we need to use `-i ''` to specify that we do not want file backups (the empty string indicates a lack of backup extension i.e. no backups). Lastly `'s/foo/bar/g` indicates we want to perform a **s**ubstitution, replacing matches of the pattern `foo` with `bar`, and that we want to perform this substitution **g**lobally, i.e. for all matches in the file.

This method works very well for me, fits in the UNIX philosophy, and avoids using any sort of extra shell script. For people googling "ripgrep search and replace", hopefully this will help you use some basic `sed` to accomplish your goals!

---

_Comment by @BurntSushi on 2018-03-27 20:23_

@z2oh Thanks for that write up! I would be delighted if you'd be willing to contribute that to the FAQ. :-)

---

_Comment by @okdana on 2018-03-27 20:43_

If you don't like `sed` (because of the command syntax or its regex features) you can also use `perl -i`, which works quite similarly but is more powerful and probably more intuitive.

`sed` and `perl` do have issues that you need to be aware of, though. Despite `-i` being described as an 'in-place' modification, what it actually does is delete and replace the whole file. This means that it'll have a new inode, so any open handles to the old file will be out of date, and they will both destroy symlinks, hard links, file-system attributes and flags (`chattr`/`chflags`), extended attributes (including POSIX ACLs and Spotlight meta-data), and in some cases file permissions. No idea what kind of shenanigans you might have to deal with on Windows.

I'm guessing that the difficulty of handling cases like that in a robust manner is one of the reasons doing this in `rg` isn't an exciting prospect, and maybe also a reason there isn't already a dedicated recursive find/replace tool that everyone uses.

---

_Comment by @lydell on 2018-03-27 20:44_

`sed` sure is a competent tool. Just in case somebody gets confused what the differences between the `sed` way and my script are:

With `sed` you need to provide one regex to `rg` and one (probably the same) to `sed` (beware of regex differences between the two!) My script: Just one regex (passed to `rg`).

`sed` workflow:

1. `rg 'foo'`. Adjust the search regex until you find what you need.
2. `rg 'foo' --files-with-matches | xargs sed -i 's/foo/bar/g'`.
3. Hold your breath and hope you got that replacement right.

My script:

1. `rg 'foo'`. Adjust the search regex until you find what you need.
2. `rg 'foo' -r 'bar'`. See if the replacements look good.
3. `wrg 'foo' -r 'bar'`. Profit.

Not trying to promote my script or say that one is better than the other. But something to keep in mind :)

---

Edited `sed` workflow thanks to the tip in the next comment:

1. `rg 'foo'`. Adjust the search regex until you find what you need.
2. `rg 'foo' --files-with-matches | xargs sed 's/foo/bar/g'`. Try to spot if your replacements look good.
3. `rg 'foo' --files-with-matches | xargs sed -i 's/foo/bar/g'`. Profit. (Notice the added `-i` flag.)

The reason I say “_try_ to spot if your replacements look good” is because `sed` prints the _entire_ files, while `rg -r` only prints the relevant lines with the changes highlighted in color.

---

_Comment by @OJFord on 2018-08-21 14:14_

@lydell I do like your solution, but it's a little disingenuous to say:
> Hold your breath and hope you got that replacement right.

the equivalent `sed` steps would be 2. without `-i`, and then 3. with it, analogously to 2. `rg`; 3. `wrg`.

---

_Comment by @sergeevabc on 2019-03-16 11:47_

Anyone found a Windows solution without installing Unix utils?

Edit: there is [Goreplace][1] by @piranha, which helped me as follows ```gr "night is dark" -r "day is light"``` 

[1]: https://github.com/piranha/goreplace

---

_Comment by @frangio on 2019-04-17 21:19_

I would like a ripgrep-like sed replacement because I don't want to mentally switch between ripgrep-style and sed-style regex syntax.

I wrote a bash script that uses ripgrep to find the matching files, bash to loop over them, and ripgrep with the `--passthru` option to do the actual replacement it. It requires `sponge` from [`moreutils`](https://joeyh.name/code/moreutils/).

https://gist.github.com/frangio/462d5563d88a2982b6c23e6d2e72e93c

---

_Comment by @stefanvanburen on 2019-04-17 21:21_

@frangio how about https://github.com/chmln/sd?

---

_Comment by @hauntsaninja on 2019-08-08 01:04_

In case it's of use to anyone, here's a Python script I wrote that's a thin wrapper around `rg --replace`. You can accept or reject changes individually using the `--ask` flag. It also accepts all of ripgrep's flags.

https://github.com/hauntsaninja/rg-sed

---

_Comment by @CantGetRight82 on 2019-08-11 12:15_

I also wrote a wrapper in NodeJS that shows the ripgrep output and then asks to apply them all or separately:

https://github.com/CantGetRight82/rgw

---

_Comment by @ssbarnea on 2020-01-05 13:26_

Can we reopen this? sed workaround is cross-platform compatible due to gnu/bsd differences so it would be much better to have a solution that does not rely on external dependencies.

---

_Comment by @BurntSushi on 2020-01-05 14:09_

No. Feel free to go build one or use any of the numerous projects linked above or in the FAQ.

---

_Comment by @nevdelap on 2020-02-05 01:37_

`ned` is `grep` with replace/`sed` over multiple lines... https://github.com/nevdelap/ned#replace

I started it, for my own use, before `ripgrep` existed, and for search `ripgrep` is probably much better (I don't use it because `ned` does what I need), but the point of `ned` is to make it easy to do bulk edits.

I mention at the top of the TL;DR that if you are just doing searches to use `ripgrep`. https://github.com/nevdelap/ned#tldr

---

_Comment by @BatmanAoD on 2020-04-02 01:11_

I actually really like the `patch` format idea and was going to suggest it until I found this thread. I have been using something similar to the `sed`/`perl` solution above since before RipGrep was released (I used `ack` before I switched to RipGrep`).

I'd be happy to write a tool to convert RipGrep's output to the patch format, but I'm not sure that's possible as-is, since `patch` includes the original line as well as the replacement, but `rg --replace` doesn't print out the pre-replacement line. @BurntSushi would you be open to a PR with a flag for use in conjunction with `--replace` that would include the pre-replacement text as well?

The main reasons I'd prefer a patch file are because, as mentioned above, it would be nice to be able to review a large-scale change before applying it, and because patches are easily revertible, whereas reversing the search-and-replace is not (in case the replacement string already existed anywhere in the directory).

One other major benefit is that patchfiles can be used as-is with multiple other tools (such as Git). It's also possible that using multiple regex tools may eventually reveal incompatibilities between them, and that a patch-based approach would be faster than re-running a complex regex twice on every line of the files that will be modified.

---

_Comment by @BurntSushi on 2020-04-02 01:20_

@BatmanAoD I think it would be better to build a dedicated tool for such functionality. It sounds exactly like the kind of feature that will beget more features, and I'm not that interested in maintaining it.

---

_Comment by @jessekv on 2020-05-07 19:03_

@lydell Here is my interactive version of your script:

```bash
rgr () {
    if [ $# -lt 2 ]
    then
        echo "rg with interactive text replacement"
        echo "Usage: rgr text replacement-text"
        return
    fi
    vim --clean -c ":execute ':argdo %s%$1%$2%gc | update' | :q" -- $(rg $1 -l ${@:3})
}
```
You can also reuse your rg args, e.g:
```bash
$ rg foo ./my-dir/ -t py
$ rgr foo bar ./my-dir/ -t py
```

---

_Comment by @BatmanAoD on 2020-05-09 23:51_

It looks like [ruplacer](https://crates.io/crates/ruplacer) has output in patch format.

---

_Comment by @dreua on 2021-02-14 12:17_

@kfir-drivenets could you add links to some solutions to your top post? My favorite would be: https://github.com/BurntSushi/ripgrep/issues/74#issuecomment-376659557

---

_Comment by @donhector on 2021-12-10 00:38_

Quick and dirty shell function based on https://github.com/BurntSushi/ripgrep/issues/74#issuecomment-625440008

```shell
replace(){ rg "$1" --files-with-matches | xargs sed -i "s/$1/$2/g" }
```

Usage:

`replace 'foo' 'bar'`

---

_Comment by @ElectricRCAircraftGuy on 2022-01-04 07:28_

For anyone looking for this feature, as I have been:

1. For the simple solution/manual command approach, see this excellent 1-liner by @z2oh here: https://github.com/BurntSushi/ripgrep/issues/74#issuecomment-376659557
1. For those looking for a more-complete solution with printed output of the replacements, ability to specify paths, etc--ie: a full implementation within ripgrep itself, or wrapping ripgrep, it was not trivial, but I just wrote a wrapper around RipGrep to give it this feature. I call it `rgr` for RipGrep Replace, since it will do the find-and-replace feature on your disk. See installation instructions at the top of the file here: https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/useful_scripts/rg_replace.sh

Example usage from `rgr -h`:

```
EXAMPLE USAGES:

    rgr foo -r boo
        Do a *dry run* to replace all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo
        ACTUALLY REPLACE ON YOUR DISK all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo file1.c file2.c file3.c
        Same as above, but only in these 3 files.
    rgr foo -R boo -g '*.txt'
        Use a glob filter to replace on your disk all instances of 'foo' with 'boo' in .txt files
        ONLY, inside this folder and down. Learn more about RipGrep's glob feature here:
        https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs
    rgr foo -R boo --stats
        Replace on your disk all instances of 'foo' with 'boo', showing detailed statistics.
```


---

_Comment by @ElectricRCAircraftGuy on 2022-01-04 07:32_

Full `rgr -h` help menu:

```
rgr ('rgr') version 0.1.0

RipGrep Replace (rgr). This program is a wrapper around RipGrep ('rg') in order to allow some
extra features, such as find and replace in-place in files. It doesn't rely on 'sed'. It uses
RipGrep only. Since it's a wrapper around 'rg', it forwards all options to 'rg', so you can use it
as a permanent replacement for 'rg' if you like.

Currently, the only significant new option added is '-R' or '--Replace', which is the same as
RipGrep's '-r' except it MODIFIES THE FILES IN-PLACE ON YOUR FILESYSTEM! This is great! If you
think so too, go and star this project (link below).

USAGE (exact same as 'rg')
    rgr [options] <regex> [paths...]

OPTIONS
    ALL OPTIONS ACCEPTED BY RIPGREP ('rg') ARE ALSO ACCEPTED BY THIS PROGRAM. Here are just a few
    of the options I've amended and/or would like to highlight. Not all Ripgrep options have been
    tested, and not all of them ever will be by me at least.

    -h, -?, --help
        Print help menu
    -v, --version
        Print version information.
    --run_tests
        Run unit tests (none yet).
    -d, --debug
        Turn on debug prints. '-d' is not part of 'rg' but if you use either of these options here
        it will auto-forward '--debug' to 'rg' under-the-hood.
    -r <replacement_text>, --replace <replacement_text>
        Do a dry-run to replace all matches of regular expression 'regex' with 'replacement_text'.
        This only does the replacement in the stdout output; it does NOT modify your disk!
    -R <replacement_text>, --Replace <replacement_text>
        THIS IS THE ONE! Bingo! This is the sole purpose for the creation of this wrapper. This
        option will actually replace all matches of regular expression 'regex' with
        'replacement_text' ON YOUR DISK. It actually modifies your file system! This is great
        for large code-wide replacements when programming in large repos, for instance.
    --stats
        Show detailed statistics about the ripgrep search and replacements made.

    SEE ALSO 'rg -h' AND 'man rg' FOR THE FULL HELP MENU OF RIPGREP ITSELF, WHICH OPTIONS ARE ALSO
    ALLOWED AND PASSED THROUGH BY THIS 'rgr' WRAPPER PROGRAM.

EXAMPLE USAGES:

    rgr foo -r boo
        Do a *dry run* to replace all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo
        ACTUALLY REPLACE ON YOUR DISK all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo file1.c file2.c file3.c
        Same as above, but only in these 3 files.
    rgr foo -R boo -g '*.txt'
        Use a glob filter to replace on your disk all instances of 'foo' with 'boo' in .txt files
        ONLY, inside this folder and down. Learn more about RipGrep's glob feature here:
        https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs
    rgr foo -R boo --stats
        Replace on your disk all instances of 'foo' with 'boo', showing detailed statistics.


Note to self: the only free lowercase letters not yet used by 'rg' as of 3 Jan. 2021 are:
    -d, -k, -y

This program is part of eRCaGuy_dotfiles: https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles
by Gabriel Staples.

```

---

_Comment by @Spongman on 2022-02-05 23:12_

the whole "just use sed" thing would be fine if sed and rg had compatible syntax.

---

_Comment by @steabert on 2022-02-12 09:33_

> Real search and replace would be an absolutely excellent feature.
> 
> But I can well believe it would be painful and complex to implement.
> 
> Perhaps there could be a reasonable middle way: if `rg` could output in `.patch` file based on the replace argument the user could then review the patch and apply it with they're happy.
> 
> Something like:
> 
> ```shell
> rg '(foo) (bar)' --replace '$2 $1' --diff-output > rg.diff
> cat rg.diff
> ...
> patch < rg.diff
> ```
> 
> That has the advantage that the user couldn't easily mess up a directory by happening to add an argument and the business of modifying files is left to another command.

I've been using [sad](https://github.com/ms-jpq/sad) for a while, and in the README there I discovered that ripgrep actually supports `--replace`. That made me wonder about the same thing you proposed here, to have ripgrep generate diff output. The beauty of that solution is that it doesn't require ripgrep to modify any files, and at the same time it's very flexible as the output can be viewed with any kind of diff pager for easy previewing, and to eventually apply with e.g. `patch` or `git apply`.

My quick-and-dirty solution (for single-line changes) has been the following little snippet to produce a sad-person's diff format:

```
cat ./my-sad-script
#!/usr/bin/env bash
m=$(mktemp)
f=$(mktemp)
r=$(mktemp)
rg "$1" --vimgrep | sort >$m
cat $m | awk -F ':' '{print $1}' | uniq >$f
cat $f | xargs rg "$1" --replace "$2" --vimgrep | sort >$r
cat $m | awk 'match($0, /([^:]+):([0-9]+):[0-9]+:(.*)/, a) {print "--- " a[1] "\n" "@@ " "-" a[2] " +" a[2] " @@" "\n" "-" a[3]}' >$m.s
cat $r | awk 'match($0, /([^:]+):([0-9]+):[0-9]+:(.*)/, a) {print "+++ " a[1] "\n" "@@ " "-" a[2] " +" a[2] " @@" "\n" "+" a[3]}' >$r.s
paste -d"\n" $m.s $r.s | uniq
```

I then usually pipe the output to [delta](https://github.com/dandavison/delta) for previewing with:
```
./my-sad-script 'search-regex 'replace' | tee /tmp/patch | delta
```
and when it looks good, I just
```
git apply -p0 --unidiff-zero /tmp/patch
```

---

_Comment by @ElectricRCAircraftGuy on 2022-02-13 21:26_

@steabert , the limitation of using diff output is that is only works inside git repos. If you want to do find and replace outside a git repo you're out-of-luck. I recommend using my [`rgr` (Ripgrep replace) wrapper](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/useful_scripts/rg_replace.sh) instead. It's super easy to use, and since it wraps `rg`, it gives you access to _all_ of `rg` plus the `-R` option, which does an actual in-file replace.

From `rgr -h`:

```bash
    rgr foo -r boo
        Do a *dry run* to replace all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo
        ACTUALLY REPLACE ON YOUR DISK all instances of 'foo' with 'boo' in this folder and down.
```

---

_Comment by @BatmanAoD on 2022-02-13 23:47_

@ElectricRCAircraftGuy Patch files are *much* older than git, and you can apply them anywhere with the `patch` command (assuming you have standard Unix-y tools or a suitable substitute). 

---

_Comment by @abitrolly on 2022-06-18 12:01_

For those who is coming late to the party, **no, there is no support for replace in files**, here is **[the link to FAQ](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#search-and-replace)** and TL;DR to **[fastmod](https://github.com/facebookincubator/fastmod)**.

`rg` + `fastmod` = :heart:

---

_Comment by @Corallus-Caninus on 2022-07-21 02:55_

I don't want to be "that guy" but this should definitely be supported. While its important to be able to pipe together commands which rg does a great job of, there should at least be support for a "modification" feature flag. Theres something about piping rg with single-threaded, non-vectorized and largely scripted tools that curbs my enthusiasm.
This would be a great help to beginners of macro programming and consolidate CLI tools that should have been integrated decades ago (in my opinion).

---

_Comment by @ElectricRCAircraftGuy on 2022-07-21 03:29_

@Corallus-Caninus , meanwhile, have you seen my wrapper above? Use `rgr -R` to replace on the disk.

---

_Comment by @BurntSushi on 2022-07-21 10:48_

I am _never_ going to add this to ripgrep. It isn't happening.

Normally I would lock this issue. But folks are still posting helpful content about other approaches. So I'm going to leave this open, but I'm going to mark comments asking for this to be in ripgrep as off topic.

There is no hope. It will never happen. Final decision.

---

_Comment by @BatmanAoD on 2022-08-07 00:22_

Disclaimer: I think `fastmod` (also recommended above) is probably the best tool for anyone who wants a decently full-featured search-and-replace CLI tool. With that said...

Now that `ripgrep` has been refactored into libraries, and one of those libraries is a set of "printers", I went through the exercise of adding a "patch printer" that would print patch-output. I then extracted just the necessary patch-printer and arg-parsing bits from this fork of `ripgrep` to create a separate tool with the same functionality.

Here's the version that's a branch of `ripgrep`: https://github.com/BurntSushi/ripgrep/pull/2275

Here's the version that extracts the necessary bits: https://github.com/BatmanAoD/RipPatch

Note that it relies on a [a branch](https://github.com/BurntSushi/ripgrep/pull/2276) that publishes `ripgrep`'s actual text-replacement functionality, which currently isn't usable outside of RipGrep.

I wanted RipPatch to respect or ignore every `ripgrep` argument it could, so that working `ripgrep` searches could be trivially transformed into RipPatch commands. Between this and some other functionality that `ripgrep` doesn't expose for import and had to be copied over, there's _a lot_ of duplication of code from `ripgrep`, which doesn't thrill me. The fork-of-ripgrep approach seems ultimately simpler.

There's a third possible approach, which is to use either the `json` printer or the `--byte-offset` option to generate `ripgrep` output that can provide sufficient information for a separate tool to generate the patch itself. That seems like the least straightforward approach, though, especially since the simplest way to transfer the data from `ripgrep` to the new tool would be something platform-specific (or at least shell-specific) like a pipe.

---

_Comment by @ElectricRCAircraftGuy on 2022-08-07 04:20_

>  so that working ripgrep searches could be trivially transformed into RipPatch commands.

Or, just use regular ripgrep commands, plus `-R`.

@BatmanAoD , at the risk of me sounding redundant, or self-promoting, have you tried running this to do an on-disk replacement?: `rgr foo -R boo`. From the `rgr -h` help menu:

```bash
    rgr foo -r boo
        Do a *dry run* to replace all instances of 'foo' with 'boo' in this folder and down.
    rgr foo -R boo
        ACTUALLY REPLACE ON YOUR DISK all instances of 'foo' with 'boo' in this folder and down.
```

I feel like people either don't know about the wrapper I wrote, don't understand how to use it, or don't believe how easy it is to use. It is a wrapper around ripgrep and it supports **all** ripgrep commands, period, plus `-R` to do an actual on-disk replacement (ripgrep only supports `-r`).

It is here: https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/useful_scripts/rg_replace.sh

Installation instructions are at the top of the file. I plan on keeping it functional as long as `ripgrep` exists, and I am alive. Once you've installed it, run `rgr -h` for a full help menu. 

---

Can anyone offering an alternative please tell me what problem my wrapper has yet to solve? I don't understand. I feel like if you just knew it existed, you'd use it and be done asking for an on-disk ripgrep replace option, since this is the sole purpose of my wrapper and exactly what it does. And again, it supports **all** ripgrep commands, **exactly** as ripgrep does, as well.

That being said, if you're needing some sort of patch tool instead of an on-disk ripgrep-based search and replace tool, my wrapper may not be what you want (albeit, I don't know what a patch tool is or does or why someone would want one :slightly_smiling_face:).


---

_Comment by @abitrolly on 2022-08-07 05:14_

@ElectricRCAircraftGuy I can install `fastmod` with `brew install fastmod`. While downloading your script and tossing it into some PATH accessible dir is too old school for me. There is also no separate repo, so it is hard to track versions and see related bugs. It is also not clear if it support confirming changes or not. If interface gives previews with context of what is replaced. I could probably go and read the script to find out, but I didn't. But.. if there was a recorded `.gif` demo (which `fastmod` does not) I could at least watch it. Frankly, because of lack of demo, I would ignore `fastmod` too, but `facebook` name together with stars convinced my to give it a try and spend 15 minutes to try.

---

_Comment by @OJFord on 2022-08-07 18:20_

Yes, @ElectricRCAircraftGuy, I agree with both you and @abitrolly, 'people don't know about' your wrapper because discoverability is hard, it's not obviously supported, etc.

If you put it in its own repo it will no doubt collect 'stars', seem like a 'real' thing that's visibly maintained, more people will package it even if you don't provide first-party ones, etc.

---

_Comment by @BatmanAoD on 2022-08-07 23:00_

@ElectricRCAircraftGuy `patch` is a standard POSIX tool, and `git` implements some of its functionality. It takes a diff between files (in one of several formats) and applies it. [Here's the Linux man page.](https://man7.org/linux/man-pages/man1/patch.1.html)

Regarding making a version of `ripgrep` that can produce output consumable by `patch`: I just thought it was a cool idea, and despite being suggested 6 years ago, it appeared that no one else had actually implemented it, so I wanted to see what a proof-of-concept would look like. The benefits of creating a `patch`-readable diff are primarily in the fact that the diff itself is a separate artifact, so you can examine it before applying it, and it's trivial to reverse the change after applying it (both BSD and GNU `patch` support this using `--reverse`).

Regarding your script, I haven't actually tried it, but I did know about it prior to writing my code, and I actually read through the source before posting my comment.

To be honest, the _primary_ reason I don't use your script is simply that I already have a Bash function [in my personal config](https://github.com/BatmanAoD/public-config/blob/master/dotfiles/bash.d/functions#L356) that works about equally well for my purposes. It does suffer from some drawbacks that your script avoids, particularly the risk of `perl` and `rg` potentially not handling a complex regex in the same way.

The secondary reason is that it's nontrivial to install your script and make it work on all platforms. I always install `git` on Windows, and that comes with `git-bash`, but I don't necessarily have the `.sh` file extension set up to run with `git-bash`, so the most reliable way to run the script on Windows would be to create `rgr.bat` to invoke Git Bash with that script. That's trivial but (for me personally) not worth the effort.

The tertiary reason is simply that Bash scripts are notoriously flaky, and your script is a lot more complex than the function I wrote with `xargs perl`, so I feel that there's a low but not-insignificant chance that if I replaced my solution with yours, I'd end up running into a weird edge-case behavior. (Though to your credit, it looks quite clean as Bash scripts go.)

Note that the `patch` solution, your script, and my `xargs perl` solution are all probably slower than a single-executable solution would be when run on large directories with lots of matches; this is part of why `fastmod` is appealing (the other reasons being the interactive-confirmation UX and the ease of cross-platform installation). In the non-`fastmod` approaches, the matching files must be handled serially rather than in parallel, and their contents must be searched twice. Additionally, your script loads the entire contents of each file into memory, which could be a problem for very large files. With that said, it's purely conjecture on my part that `fastmod` performs better than your script; the observation is not very useful without some empirical data. If you're interested, I'd recommend comparing the performance of `fastmod --accept-all` against `rgr -R` on large directories.



---

_Comment by @ElectricRCAircraftGuy on 2022-09-19 06:24_

@abitrolly @OJFord @BatmanAoD , I appreciate the feedback. In an attempt to begin addressing your concerns, collect stars--as you said, have broader support and better tracking, etc., I've moved my `rgr` "ripgrep replace" script to its own repo:

https://github.com/ElectricRCAircraftGuy/ripgrep_replace

If anyone wants to make a gif of its usage, feel free, and I can add it to the readme via a pull request. Otherwise, I'll make an issue to do that.

Issue: https://github.com/ElectricRCAircraftGuy/ripgrep_replace/issues/1

---

_Comment by @aagit on 2025-10-02 07:36_

Hello,

I recently improved my code assistant workflow by having the LLM rewrite the ripgrep's output. This keeps the transformer's input/output razor-thin, boosting accuracy, increasing performance, while avoiding the complexity and overhead of full agentic AI if all you need is to grep and modify multiple files in a large codebase.

The problem then was to  apply the changes of the rewritten/refactored ripgrep back to the original files, to do that I had to write a new tool, not even wgrep would work while dealing with a full gptel-rewrite including all metadata.

However it should be useful for general regex-based edits and can be integrated with any editor or sed-like workflow.

https://gitlab.com/aarcange/ripgrep-edit/

---
