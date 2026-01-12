```yaml
number: 103
title: Add filename-only option
type: issue
state: closed
author: ddrcoder
labels: []
assignees: []
created_at: 2016-09-26T16:04:03Z
updated_at: 2022-11-19T21:52:47Z
url: https://github.com/BurntSushi/ripgrep/issues/103
synced_at: 2026-01-12T16:13:21Z
```

# Add filename-only option

---

_@ddrcoder_

I quite frequently use `grep -l` to simply identify files which contained any match of the given pattern.  Since only existence checking is required, you could terminate at the first match. The list of files can then be fed to `xargs` or the like. This would go hand-in-hand with the `-0, --null` option.


---

_Comment by @BurntSushi on 2016-09-26 16:20_

This is already done. It's in `0.2.0`.


---

_Closed by @BurntSushi on 2016-09-26 16:20_

---

_Comment by @Flaque on 2018-08-17 17:41_

To be clear for folks who stumbled upon this like I did, the way to only show filenames in `rg` is the same as in `grep`:

```
$ rg -l 
```

---

_Comment by @watsoncj on 2020-05-28 04:41_

Here's the relevant help section from ripgrep 12.1.0:
```
    -l, --files-with-matches
            Only print the paths with at least one match.

            This overrides --files-without-match.
```

---

_Comment by @iburunat on 2020-05-28 05:29_

Maybe helpful to know that adding the flag `-c/--counter` will show the number of lines matching the searched term. The flag `--count-matches` will count the number of matches instead (see [man page](https://www.mankier.com/1/rg#--count)).

---

_Comment by @NetMage on 2021-01-19 19:42_

I think the help could be a little clearer - it doesn't clearly imply that the matches won't be printed.


---

_Comment by @SoftwareApe on 2021-03-17 18:44_

I agree with @NetMage I looked through all the file related options in the help and this one didn't seem to apply.

>Only print the paths with at least one match.

To me this says that it will only print those files that have at least one match. Which sounds like the default behavior. It doesn't say that it *doesn't* print the match, which is what most people are likely looking for.

I think it would be more clear if the help read

>Only print the path to the files with matches, not the matches.

---

_Comment by @NightMachinery on 2021-03-21 11:53_

> To be clear for folks who stumbled upon this like I did, the way to only show filenames in `rg` is the same as in `grep`:
> 
> ```
> $ rg -l 
> ```

`rg -l .`, to just list everything.

---

_Comment by @RichardBronosky on 2022-11-19 21:52_

Thank you for this! I used it to make `vg`, the perfect companion to `rg`.

```sh
function vg() { $EDITOR $(rg -l "$@"); }
```
Then I can follow up "find all files containing *pattern*" with "edit them".

---
