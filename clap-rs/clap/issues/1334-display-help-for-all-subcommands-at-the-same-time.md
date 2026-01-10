---
number: 1334
title: Display help for all subcommands at the same time
type: issue
state: closed
author: mmstick
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2018-08-24T17:55:46Z
updated_at: 2023-11-10T22:21:54Z
url: https://github.com/clap-rs/clap/issues/1334
synced_at: 2026-01-10T01:26:49Z
---

# Display help for all subcommands at the same time

---

_Issue opened by @mmstick on 2018-08-24 17:55_

**Maintainer's note: Proposed solution**

Setting:
- `help_includes_subcommands` or `help_shows_subcommands`
  - trying to keep what the setting impacts first, otherwise I would have done `flatten_subcommands_in_help`
  - not starting with `subcommand` because this is set on the parent of the subcommands rather than a subcommand itself

Help output:
- USAGE includes regular usage and usage for each subcommand that is visible in short help
- Show current command's subcommand and arguments like normal
  - Even if parent has `help_includes_subcommands` set (this diverges from `git stash <command> -h` which shows `git stash -h`)
- After current command's arguments, for each subcommand visible in short help
  - Show a header for the subcommand
  - Show short-help arguments under the above heading (ie ignore help headings)
    - Ideally, hide all generated and global arguments
  - Do not list subcommands

Man page:
- Same as above but show long-help
- This will actually be split out into a separate issue

Open question:
- Do we recurse?
  - If so, we should elide intermediate subcommands that only have generated arguments

Implementation:
- Usage generation will need to be refactored so we can request the parts of it that we need

---
The API is very large and I've struggled to find any examples or hints pointing to this possibility. Is it possible for the help page for a program to list the help pages of all subcommands at the same time, rather than just the current scope? That way the user does not need to also request for the help info from each subcommand.

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `D: intermediate` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 06:33_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#99](../../epage/clapng/issues/99.md) on 2021-12-06 17:36_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:14_

---

_Comment by @epage on 2021-12-08 20:15_

Do you have examples of commands that do that today?  I'm having a hard time picturing this not being overwhelming.

---

_Comment by @mmstick on 2021-12-08 20:32_

I would have examples if clap supported it. Command line applications don't always have 100s of options. Underwhelming is how I'd describe the current usefulness of the help info generated right now.

---

_Comment by @mmstick on 2021-12-08 20:34_

```
# system76-power -h
system76-power 1.1.20
Utility for managing graphics and power profiles

USAGE:
    system76-power <SUBCOMMAND>

OPTIONS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    charge-thresholds    Set thresholds for battery charging
    daemon               Runs the program in daemon mode
    graphics             Query or set the graphics mode
    help                 Prints this message or the help of the given subcommand(s)
    profile              Query or set the power profile
```

```
# system76-power profile -h
system76-power-profile
Queries or sets the power profile.

 - If an argument is not provided, the power profile will be queried
 - Otherwise, that profile will be set, if it is a valid profile

USAGE:
    system76-power profile [profile]

OPTIONS:
    -h, --help    Prints help information

ARGS:
    <profile>    set the power profile [possible values: battery, balanced, performance]
```

Would be much more useful combined.

---

_Comment by @mkayaalp on 2021-12-08 20:40_

```
# opkg
opkg must have one sub-command argument
usage: opkg [options...] sub-command [arguments...]
where sub-command is one of:

Package Manipulation:
	update			Update list of available packages
	upgrade <pkgs>		Upgrade packages
	install <pkgs>		Install package(s)
	configure <pkgs>	Configure unpacked package(s)
	remove <pkgs|regexp>	Remove package(s)
	flag <flag> <pkgs>	Flag package(s)
	 <flag>=hold|noprune|user|ok|installed|unpacked (one per invocation)

Informational Commands:
	list			List available packages
	list-installed		List installed packages
	list-upgradable		List installed and upgradable packages
	list-changed-conffiles	List user modified configuration files
	files <pkg>		List files belonging to <pkg>
	search <file|regexp>	List package providing <file>
	find <regexp>		List packages whose name or description matches <regexp>
	info [pkg|regexp]	Display all info for <pkg>
	status [pkg|regexp]	Display all status for <pkg>
	download <pkg>		Download <pkg> to current directory
	compare-versions <v1> <op> <v2>
	                    compare versions using <= < > >= = << >>
	print-architecture	List installable package architectures
	depends [-A] [pkgname|pat]+
	whatdepends [-A] [pkgname|pat]+
	whatdependsrec [-A] [pkgname|pat]+
	whatrecommends[-A] [pkgname|pat]+
	whatsuggests[-A] [pkgname|pat]+
	whatprovides [-A] [pkgname|pat]+
	whatconflicts [-A] [pkgname|pat]+
	whatreplaces [-A] [pkgname|pat]+

Options:
	-A			Query all packages not just those installed
	-V[<level>]		Set verbosity level to <level>.
	--verbosity[=<level>]	Verbosity levels:
					0 errors only
					1 normal messages (default)
					2 informative messages
					3 debug
					4 debug level 2
	-f <conf_file>		Use <conf_file> as the opkg configuration file
	--conf <conf_file>
	--cache <directory>	Use a package cache
	-d <dest_name>		Use <dest_name> as the the root directory for
	--dest <dest_name>	package installation, removal, upgrading.
				<dest_name> should be a defined dest name from
				the configuration file, (but can also be a
				directory name in a pinch).
	-o <dir>		Use <dir> as the root directory for
	--offline-root <dir>	offline installation of packages.
	--verify-program <path>	Use the given program to verify usign signatures
	--add-arch <arch>:<prio>	Register architecture with given priority
	--add-dest <name>:<path>	Register destination with given path

Force Options:
	--force-depends		Install/remove despite failed dependencies
	--force-maintainer	Overwrite preexisting config files
	--force-reinstall	Reinstall package(s)
	--force-overwrite	Overwrite files from other package(s)
	--force-downgrade	Allow opkg to downgrade packages
	--force-space		Disable free space checks
	--force-postinstall	Run postinstall scripts even in offline mode
	--force-remove	Remove package even if prerm script fails
	--force-checksum	Don't fail on checksum mismatches
	--no-check-certificate Don't validate SSL certificates
	--noaction		No action -- test only
	--download-only	No action -- download only
	--nodeps		Do not follow dependencies
	--nocase		Perform case insensitive pattern matching
	--size			Print package size when listing available packages
	--strip-abi		Print package name without appended ABI version
	--force-removal-of-dependent-packages
				Remove package and all dependencies
	--autoremove		Remove packages that were installed
				automatically to satisfy dependencies
	-t			Specify tmp-dir.
	--tmp-dir		Specify tmp-dir.
	-l			Specify lists-dir.
	--lists-dir		Specify lists-dir.

 regexp could be something like 'pkgname*' '*file*' or similar
 e.g. opkg info 'libstd*' or opkg search '*libop*' or opkg remove 'libncur*'
```

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Comment by @epage on 2021-12-08 21:50_

> > Do you have examples of commands that do that today? I'm having a hard time picturing this not being overwhelming.

> I would have examples if clap supported it. Command line applications don't always have 100s of options. Underwhelming is how I'd describe the current usefulness of the help info generated right now.

I was hoping for precedent from another CLI to take inspiration from and to see how well it works in practice


@mkayaalp I see that opkg shows subcommand usage.  Is that your proposal for this?  Looks similar to https://github.com/clap-rs/clap/issues/962

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 18:37_

---

_Label `D: medium` removed by @epage on 2021-12-09 18:37_

---

_Label `E-help-wanted` removed by @epage on 2021-12-09 18:37_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 18:37_

---

_Referenced in [clap-rs/clap#1535](../../clap-rs/clap/issues/1535.md) on 2021-12-09 20:05_

---

_Comment by @dpc on 2021-12-09 20:27_

> I was hoping for precedent from another CLI to take inspiration from and to see how well it works in practice

`cargo-crev` has many, often multiple level commands, like `cargo crev repo fetch all --someoptions`. There's currently no way for the users to get all the possible commands in one go, which hurts discoverability.

---

_Comment by @epage on 2021-12-09 20:38_

@dpc mind hacking up example output of what your ideal would look like with `cargo-crev`?

---

_Comment by @dpc on 2021-12-09 20:56_

No strong opinions, but seems to me like just expanding every subcommand under `SUBCOMMANDS` and displaying instead what `<command> <that-subcommand> -h` would.

---

_Comment by @epage on 2021-12-10 16:22_

With clap, we are trying to balance ease of use, flexibility, and compile-time / binary size.

We have plans for [decoupling help generation](https://github.com/clap-rs/clap/issues/2913), allowing [custom formatting](https://github.com/clap-rs/clap/issues/2914).  I have a feeling that this might be the best way forward for this.  We wouldn't have a built-in "show all subcommands" flag but give you the tools to auto-generate the help output to look like you want it to.

I'm going to mark this issue as blocked on https://github.com/clap-rs/clap/issues/2914 to make sure we track the requirements needed for this and I'm assuming we will close this out when https://github.com/clap-rs/clap/issues/2914 is resolved.

If there are concerns about this plan, let us know!

---

_Label `S-waiting-on-design` removed by @epage on 2021-12-10 16:22_

---

_Label `S-blocked` added by @epage on 2021-12-10 16:22_

---

_Referenced in [clap-rs/clap#3174](../../clap-rs/clap/pulls/3174.md) on 2021-12-14 18:02_

---

_Label `S-blocked` removed by @epage on 2021-12-18 00:06_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-18 00:06_

---

_Comment by @epage on 2021-12-18 00:07_

Been thinking on this as we've been iterating on #3174.  I realized there is a good template for us to use for this, `git stash -h` / `git stash --help`.

---

_Comment by @epage on 2022-01-04 14:02_

One idea I was toying with

Setting:
- `help_includes_subcommands` or `help_shows_subcommands`
  - trying to keep what the setting impacts first, otherwise I would have done `flatten_subcommands_in_help`
  - not starting with `subcommand` because this is set on the parent of the subcommands rather than a subcommand itself

Help output:
- USAGE includes regular usage and usage for each subcommand that is visible in short help
- Show current command's subcommand and arguments like normal
  - Even if parent has `help_includes_subcommands` set (this diverges from `git stash <command> -h` which shows `git stash -h`)
- After current command's arguments, for each subcommand visible in short help
  - Show a header for the subcommand
  - Show short-help arguments under the above heading (ie ignore help headings)
    - Ideally, hide all generated and global arguments
  - Do not list subcommands

Man page:
- Same as above but show long-help

Open question:
- Do we recurse?
  - If so, we should elide intermediate subcommands that only have generated arguments

---

_Label `S-waiting-on-design` removed by @epage on 2022-01-04 14:02_

---

_Label `S-waiting-on-decision` added by @epage on 2022-01-04 14:02_

---

_Referenced in [clap-rs/clap#1431](../../clap-rs/clap/issues/1431.md) on 2022-01-04 14:08_

---

_Comment by @epage on 2022-01-10 15:41_

Anyone have thoughts on how to refer to / what to call the difference in subcommands between `git` and `git stash`?  `git`s subcommands act like external subcommands though whether all of them are is an implementation detail.  Since external subcommands are about the implementation, it'd be hard to reuse that name.

---

_Referenced in [clap-rs/clap#2513](../../clap-rs/clap/issues/2513.md) on 2022-01-10 15:43_

---

_Referenced in [clap-rs/clap#3365](../../clap-rs/clap/issues/3365.md) on 2022-01-28 21:57_

---

_Comment by @lu-zero on 2022-01-31 09:10_

decoupled/coupled subcommand help maybe? Assuming you want to mention all the subcommand in the same `--help` for the latter and show it for `--all-help` for the former.

---

_Referenced in [clap-rs/clap#1382](../../clap-rs/clap/issues/1382.md) on 2022-02-03 14:52_

---

_Referenced in [clap-rs/clap#3418](../../clap-rs/clap/issues/3418.md) on 2022-02-08 15:14_

---

_Referenced in [clap-rs/clap#962](../../clap-rs/clap/issues/962.md) on 2022-02-09 15:31_

---

_Renamed from "Q: Display help for all subcommands at the same time" to "Display help for all subcommands at the same time" by @epage on 2022-11-08 04:34_

---

_Referenced in [clap-rs/clap#1761](../../clap-rs/clap/issues/1761.md) on 2022-11-08 04:44_

---

_Added to milestone `4.x` by @epage on 2022-11-18 15:25_

---

_Referenced in [cargo-public-api/cargo-public-api#238](../../cargo-public-api/cargo-public-api/issues/238.md) on 2022-12-11 08:22_

---

_Referenced in [clap-rs/clap#4813](../../clap-rs/clap/issues/4813.md) on 2023-03-30 23:47_

---

_Referenced in [rust-lang/cargo#11879](../../rust-lang/cargo/pulls/11879.md) on 2023-04-03 13:49_

---

_Referenced in [clap-rs/clap#4818](../../clap-rs/clap/issues/4818.md) on 2023-04-03 20:18_

---

_Referenced in [clap-rs/clap#4897](../../clap-rs/clap/issues/4897.md) on 2023-05-09 19:28_

---

_Referenced in [clap-rs/clap#5042](../../clap-rs/clap/issues/5042.md) on 2023-07-24 15:06_

---

_Referenced in [clap-rs/clap#5204](../../clap-rs/clap/pulls/5204.md) on 2023-11-09 18:47_

---

_Referenced in [clap-rs/clap#5206](../../clap-rs/clap/pulls/5206.md) on 2023-11-09 21:24_

---

_Comment by @epage on 2023-11-09 21:24_

Would love feedback on #5206

---

_Closed by @epage on 2023-11-10 22:21_

---
