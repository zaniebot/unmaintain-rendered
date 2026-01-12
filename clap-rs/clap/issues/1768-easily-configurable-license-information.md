```yaml
number: 1768
title: Easily configurable license information
type: issue
state: open
author: Dantali0n
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2020-03-27T15:01:10Z
updated_at: 2025-06-23T13:31:04Z
url: https://github.com/clap-rs/clap/issues/1768
synced_at: 2026-01-12T16:14:11Z
```

# Easily configurable license information

---

_@Dantali0n_

### Describe your use case

Some types of software licenses, in particular GPL and friends, require licensing information to be displayed by the program upon request. Naturally this can already be configured with clap by creating a flag similar to -h/--help, however, I think this can be improved

### Describe the solution you'd like

A more seamlessly integrated solution similar to `.author()` would improve this. The `license()` function could take sufficient arguments to automatically generate the licensing excerpt. for instance: `.license(gplv3, 'MyNiceCompany.inc')`.

Calling the program with _--license_ could generate the following:
```
Copyright (C) 2020 MyNiceCompany.inc
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```


---

_Label `T: new feature` added by @Dantali0n on 2020-03-27 15:01_

---

_Comment by @CreepySkeleton on 2020-03-27 15:06_

Sounds good to me. @pksunkara @Dylan-DPC @TeXitoi what do you think?

---

_Comment by @pksunkara on 2020-03-27 17:08_

This is more similar to `version` flag and setting.

---

_Comment by @Dylan-DPC-zz on 2020-04-01 22:10_

@Dantali0n thanks for the request. In this case it is better we make it a separate crate and see how it goes and then later merge it into clap if needed. Would you like to volunteer to write it? (we can mentor). Else will try myself or get someone to write it

---

_Comment by @Dantali0n on 2020-04-02 17:48_

> @Dantali0n thanks for the request. In this case it is better we make it a separate crate and see how it goes and then later merge it into clap if needed. Would you like to volunteer to write it? (we can mentor). Else will try myself or get someone to write it

Hi, In terms of experience I don't think I am there yet as I am written my very first rust program atm. Currently I am still struggling with a lot of construct in rust, in addition, I tend not to have a lot of free time. Therefor, I think it is better if someone else tries to implement this, assuming someone has time and feels motivated to do so ofcourse.

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:32_

---

_Label `C: other` added by @CreepySkeleton on 2020-06-30 10:08_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-06-30 10:08_

---

_Label `Z: good first issue` added by @CreepySkeleton on 2020-06-30 10:08_

---

_Label `Z: mentored` added by @CreepySkeleton on 2020-06-30 10:08_

---

_Referenced in [clap-rs/clap#2144](../../clap-rs/clap/pulls/2144.md) on 2020-09-25 09:49_

---

_Referenced in [clap-rs/clap#3001](../../clap-rs/clap/issues/3001.md) on 2021-11-08 13:42_

---

_Comment by @TheAlgorythm on 2021-11-08 17:44_

I did a bit of research on the UX of displaying the license. There are mainly 2 options:

## A separate `version` subcommand

>- Most modern CLIs support a --version flag and a version command, where --version is a short, one-line machine-readable output and version is more detailed (and optionally machine readable).
>- The version command can include more detailed information, such as the backing API version or server version, git commit, build date/host, dependency versions, links to legal information, release notes, etc. It’s sort of like an “About” page on installed software.

[Opinion of Cody Ray](http://codyaray.com/2020/07/cli-design-best-practices)

> ```sh
>$ mycli version # multi only
>$ mycli --version
>$ mycli -V
>```
>Unless it’s a single-command CLI that also has a `-v`,`--verbose` flag, `$ mycli -v` should also just display the CLI version. It’s frustrating to run 3 different commands to get the version for a CLI until you find the right one.
The version command is a main place you’ll ask users for debugging information so it’s a good place to add any helpful extra information aside from just the version number that might help you diagnose issues.

[Jeff Dickey](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)

This is an elegant solution but has the downsides of possibly breaking compatibility and Imho the `version` subcommand isn't well known.

## Additional information in `version`-flag

>The standard --version option should direct the program to print information about its name, version, origin and legal status, all on standard output, and then exit successfully. [...]
>The first line is meant to be easy for a program to parse; the version number proper starts after the last space. [...]
>The following line, after the version number line or lines, should be a copyright notice. If more than one copyright notice is called for, put each on a separate line.
>Next should follow a line stating the license, preferably using one of abbreviations below, and a brief statement that the program is free software, and that users are free to copy and change it. Also mention that there is no warranty, to the extent permitted by law. See recommended wording below.
>It is ok to finish the output with a list of the major authors of the program, as a way of giving credit.
>Here’s an example of output that follows these rules:
>```
>GNU hello 2.3
>Copyright (C) 2007 Free Software Foundation, Inc.
>License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
>This is free software: you are free to change and redistribute it.
>There is NO WARRANTY, to the extent permitted by law.
>```
>You should adapt this to your program, of course, filling in the proper year, copyright holder, name of program, and the references to distribution terms, and changing the rest of the wording as necessary.
>This copyright notice only needs to mention the most recent year in which changes were made—there’s no need to list the years for previous versions’ changes. You don’t have to mention the name of the program in these notices, if that is inconvenient, since it appeared in the first line. (The rules are different for copyright notices in source files; see Copyright Notices in Information for GNU Maintainers.)
>Translations of the above lines must preserve the validity of the copyright notices (see Internationalization). If the translation’s character set supports it, the ‘(C)’ should be replaced with the copyright symbol, as follows: `©`
>Write the word “Copyright” exactly like that, in English. Do not translate it into another language. International treaties recognize the English word “Copyright”; translations into other languages do not have legal significance.

[The GNU Standard](https://www.gnu.org/prep/standards/html_node/_002d_002dversion.html#g_t_002d_002dversion) which could be a bit too GNU specific and hard to implement in an generic way.
But I think that should be the ultimate goal.

---

_Comment by @epage on 2021-11-08 17:52_

Do clap users need to list all of the legally relevant licenses and notices or just the license and attribution for their own program, leaving the rest to another file like with [cargo-about](https://github.com/EmbarkStudios/cargo-about)?

---

_Comment by @TheAlgorythm on 2021-11-08 19:13_

Idk, I'm not a lawyer.

---

_Referenced in [clap-rs/clap#3016](../../clap-rs/clap/pulls/3016.md) on 2021-11-12 16:06_

---

_Referenced in [epage/clapng#147](../../epage/clapng/issues/147.md) on 2021-12-06 19:16_

---

_Label `C: other` removed by @epage on 2021-12-08 20:19_

---

_Label `A-help` added by @epage on 2021-12-08 20:19_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:16_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:16_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 21:15_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 21:15_

---

_Label `E-medium` removed by @epage on 2021-12-09 21:15_

---

_Label `E-easy` removed by @epage on 2021-12-09 21:15_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 21:15_

---

_Comment by @dcampbell24 on 2025-06-22 18:11_

A license should also most definitely be displayed in the man page. I was thinking you should be able to display the contents of a file. Maybe you should be able to auto-generate a license excerpt from cargo's `license` attribute, but that seems more challenging.

For a file based approach I was thinking add the function `license_files_or(["LICENSE-APACHE", "LICENSE-MIT"])` to Command (maybe also allow using the cargo attribute `license-file`, but as is I use `license`, then also have `MIT-LICENSE`, not linked to, so `crates.io` works as expected, but also github works and I can link to the whole license).

Then (only bothering with the man page bit) add a `LICENSE` (or `LICENSES`) section to the man page and if, as is this example, two or more licenses are listed, put an `OR` between each license that is listed.

This is one of the features I would really like to see, because at the moment I am stuck duplicating my documentation and using `pandoc` instead of `clap_mangen`. Note, I am leaving the `copyright` as a separate issue. It would make it more challenging to combine the two.

---

_Comment by @epage on 2025-06-23 13:31_

Regarding `clap_mangen`, you can always mix your own content with what is generated and in fact I don't expect us to ever be able to fully generate man pages on our own.

We need to figure out the fundamentals about how licensing information should be included before we figure out how and where it should be rendered.

---

_Referenced in [rust-lang/cargo#15697](../../rust-lang/cargo/issues/15697.md) on 2025-06-23 13:33_

---

_Referenced in [clap-rs/clap#6041](../../clap-rs/clap/issues/6041.md) on 2025-06-23 18:05_

---
