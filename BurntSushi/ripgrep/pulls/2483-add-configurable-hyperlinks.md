```yaml
number: 2483
title: Add configurable hyperlinks
type: pull_request
state: closed
author: ltrzesniewski
labels: []
assignees: []
base: master
head: hyperlinks-cfg
created_at: 2023-04-02T17:02:57Z
updated_at: 2024-07-12T16:40:52Z
url: https://github.com/BurntSushi/ripgrep/pull/2483
synced_at: 2026-01-12T18:23:14Z
```

# Add configurable hyperlinks

---

_@ltrzesniewski_

This PR adds hyperlinks to search results in terminals which support them:

![image](https://user-images.githubusercontent.com/7913492/229365208-8e89dbb0-53d1-4e82-b6ed-3d27004dd830.png)

Compared to the previous PR (#2322), it adds a `--hyperlink-format PATTERN` command line option which lets you configure the output.

`PATTERN` can be:

- An URL pattern with the following placeholders:
	- `{file}` for the absolute file name. This one is required.
		- Without the leading `/` on Unix (as this makes the patterns more intuitive and multi-platform)
		- With `\` replaced by `/` on Windows
    - `{line}` for the line number of the result.
    - `{column}` for the column number of the result. `{line}` is required if this is used.
    - `{host}` for the hostname. This becomes `wsl$/{distro}` on WSL.
- An alias to a well-known pattern, such as `vscode`. Only a few are currently defined but hopefully more could be added over time, on the same principle as file types (`default_types.rs`). A few special aliases are included:
    - `file` for the default `file://` scheme
    - `none` to disable hyperlinks (it aliases to the empty string)

The default patterns are the ones which are the most widely supported:

- Unix: `file://{host}/{file}`
- Windows: `file:///{file}`
- WSL: `file://wsl$/{distro}/{file}`

This makes ripgrep only output hyperlinks on the file headings by default, like in the screenshot above.

If a patten includes `{line}` however, each line prelude becomes a link. Here's an example with `--hyperlink-format vscode`:

![image](https://user-images.githubusercontent.com/7913492/229365867-aa1c8387-61f0-4f1b-9b2d-bb7ed06748af.png)

The idea is for a user to add `--hyperlink-format` to their configuration file to enable more features.

This PR depends on https://github.com/BurntSushi/termcolor/pull/65 to write hyperlink control codes to the terminal. I had to make changes to the CI in order to build ripgrep without that PR having shipped. I'll revert them once the updated termcolor crate is published. Of course, if any change is requested there, I'll update this PR accordingly.

I tried my best to write high-quality code, I hope it's good enough, but I'd appreciate any feedback on how I could have done things better, since Rust isn't my day-to-day language. 🙂

Please ignore the long commit history, I'll either clean it up or just squash it once the changes are approved.

Closes #665 
Supersedes #2322

> **Note**
> - Preview optimized builds can be found [here](https://github.com/ltrzesniewski/ripgrep/actions/workflows/preview.yml?query=is%3Asuccess), for those interested. Just pick the newest build, and you'll find artifacts for Linux/Windows/macOS.
> - For Windows Terminal users, [this change](https://github.com/microsoft/terminal/pull/14993) needs to ship before any scheme other than the default can be used.


---

_Comment by @fabian-thomas on 2023-04-02 17:24_

Works well for me. One minor thing I noticed, is that the last colon ist embedded in the link label. Visually it would be more pleasant to exclude that, but opinions on that might differ. Thanks for implementing this.

---

_Comment by @fabian-thomas on 2023-04-02 17:31_

And one more minor thing. The column seems to be off by one. I would assume it to be zero-based.

---

_Comment by @ltrzesniewski on 2023-04-02 19:23_

Thanks for your feedback! 🙂 

> One minor thing I noticed, is that the last colon ist embedded in the link label. Visually it would be more pleasant to exclude that, but opinions on that might differ.

Yes, I thought about it, and I think you're right - it would probably look better.

But I didn't want to change the existing code too much: currently there is one function per field (file/line/column/byte offset), which writes the field value followed by its separator (it may be different for the file). I'd have to break that design in order to end the hyperlink right before the last separator.

I'll see if I can come up with something clean though.



> The column seems to be off by one. I would assume it to be zero-based.

It works fine for me, at least in VSCode. Maybe your editor uses zero-based columns? In that case, I may have to add a `{column0}` placeholder (please tell me if you can come up with a better name, naming is hard).



---

_Comment by @fabian-thomas on 2023-04-02 19:36_

> Yes, I thought about it, and I think you're right - it would probably look better.
> 
> But I didn't want to change the existing code too much: currently there is one function per field (file/line/column/byte offset), which writes the field value followed by its separator (it may be different for the file). I'd have to break that design in order to end the hyperlink right before the last separator.
> 
> I'll see if I can come up with something clean though.
>

If it would require major modifications, I think it can just stay like that. It's really minor for me.

> It works fine for me, at least in VSCode. Maybe your editor uses zero-based columns? In that case, I may have to add a `{column0}` placeholder (please tell me if you can come up with a better name, naming is hard).

I'm using emacs. I'm not sure if there is support for custom URIs like with vscode. I use my own scripts so it's not that important for me. I can just substract one from each passed column. I guess I was wrong here. I just found it odd that something is 1-based, but lines are 1-based too. So I would wait for other people to report issues with that before you implement a switch for that. 🙂 


---

_Comment by @misaki-web on 2023-04-09 14:46_

> * An alias to a well-known pattern, such as `vscode`. Only a few are currently defined but hopefully more could be added over time

Hyperlinks in `ripgrep` results would be a very useful feature! By the way, some time ago, I wrote [`grep+`, a script improving grep results display](https://github.com/misaki-web/grepp) (hyperlinks, alignment and syntax highlighting). Example:

![grep+ default dark theme](https://raw.githubusercontent.com/misa-ki/grepp/main/assets/default-dark.png)

In order to open results to a specific file and line when clicking on hyperlinks, I created a custom scheme (`grep+:///path/to/file:line_number`, with fallback to the scheme `file:///path/to/file` if needed) supporting about 20 editors:

```
atom {file}:{line}
code -g {file}:{line}
eclipse {file}:{line}
emacsclient +{line} {file}
geany +{line} {file}
gedit {file} +{line}
gvim {file} +{line}
jedit {file} +line:{line}
kate -l {line} {file}
kile --line {line} {file}
kwrite -l {line} {file}
micro -parsecursor true {file}:{line}
nano +{line} {file}
notepadqq -l {line} {file}
pluma {file} +{line}
qtcreator {file} +{line}
scite {file} -goto:{line}
vim {file} +{line}
vi {file} +{line}
```

It allows to open files to a specific line (or column, but I've not added column support in `grep+`) with any editor, no matter if there's a scheme like `vscode://` or not. It may not be in the scope of `ripgrep` to support any editor, but I mention it here for brainstorming.

---

_Comment by @ltrzesniewski on 2023-04-09 15:47_

@misaki-web Thanks, I added the `grep+` scheme to the list in this PR. 🙂 

@fabian-thomas I made the hyperlink end before the last field separator, and it looks better indeed:

![image](https://user-images.githubusercontent.com/7913492/230782664-7b4117c0-9f9e-49fc-bf09-0b3bcbe1456d.png)


---

_Comment by @ltrzesniewski on 2023-06-05 11:23_

@BurntSushi since I know you're careful about the dependency tree of ripgrep, and this PR currently adds a dependency to `once_cell`, would it be better to raise the MSRV to 1.70 in order to replace that crate with the implementation from `std`?

(note that this PR also adds a dependency to `gethostname` to handle the `{host}` placeholder, I hope that's ok)

---

_Comment by @BurntSushi on 2023-06-05 11:43_

I haven't reviewed this PR yet. There is *a lot* of code here. It may be warranted, but I really just haven't sat down to explore the design space myself yet.

`once_cell` is a fine dependency, but yes, raising the MSRV to the latest Rust stable release is perfectly okay for ripgrep. If given the choice, I would indeed rather use `std` instead of adding a dependency.

---

_Comment by @ltrzesniewski on 2023-06-05 11:50_

No worries, I understand it's a large change and I don't want to rush you. 🙂

I'll make the switch to `std`.

---

_Comment by @rmartine-ias on 2023-06-14 19:56_

Thanks for making `PATTERN` a configurable string, this feature will be very useful to me. I use a pile of horrible hacks that requires file path URLs to always have a `#` fragment, even if there is no line number. (So I'd be setting `'file://{host}/{file}#'`)

(iTerm2 can be set, in the fragment case (but not for all links, regardless of fragment status), to use Semantic History rules for hyperlinks, which lets you run a coprocess when they're clicked on instead of passing to the OS, which lets me stack several more hacks to get the link to open in a vim pane in the current tmux window.)

---

_Comment by @ltrzesniewski on 2023-06-15 12:56_

I'm glad you find it useful! 😄

May I ask you a little question, since you seem to be a proficient macOS user? Are you aware of any popular macOS apps which provide custom URL handlers that users may want to use to open ripgrep links?

I could add them to the [aliases list](https://github.com/ltrzesniewski/ripgrep/blob/hyperlinks-cfg/crates/printer/src/hyperlink_aliases.rs) in this PR, as it's currently a little short.


---

_Comment by @rmartine-ias on 2023-06-16 18:57_

I think there are probably quite a few, which is why the configurability is good. Here are the editors iTerm2 has for opening files with:

<img width="195" alt="Screenshot 2023-06-16 at 12 22 02 PM" src="https://github.com/BurntSushi/ripgrep/assets/107639398/ea079cae-aee1-48db-8c9f-c42794b0bfff">

I'm not sure how many of them have their own path handlers. I know a few people who use MacVim, [which has one](https://macvim.org/docs/gui_mac.txt.html#mvim%3A%2F%2F). I think Atom has fallen out of favor, while Sublime is reasonably popular. If you're adding built-in support for vscode, might as well add vscodium as well.

If you're really trying to pack features in, you could add a git remote URL scheme that hyperlinks to `https://your.git.host/current-repo/tree/current-branch/{path}` or something but I think that's a little out of scope for an MVP ;p

---

_Comment by @ltrzesniewski on 2023-06-16 20:16_

Great, thanks for this list! I've added a few aliases.

I'm not really trying to pack features in, but I thought having a built-in list of common URL patterns would be nice to have. 🙂


---

_Comment by @BurntSushi on 2023-07-07 21:48_

It's bad juju to merge master back into a feature branch.

There's 35 commits here. I'm presuming this is an artifact of development. If so, please rebase this onto master and squash it down to one commit.

If that is too much work, then I would say skip it because I haven't reviewed this PR yet and I dunno if I will accept it. A 1K line diff is a red flag to me.

---

_Comment by @mitchcapper on 2023-07-07 22:23_

Well they are writing a new feature in and tried to do it in a very dynamic/generic way rather than just hardcoding a single hyperlink format. I am also guessing they expected the squash+merge but it shouldn't be an issue to rebase onto master with a single commit.  POC at: https://github.com/mitchcapper/ripgrep/compare/master...pr_rebased

As for the number of lines most is the new hyperlink class (almost 700 of the 1000) with the largest number of changes in the standard printer where 130 of the 200 new lines are all together for the new structured writer.   It is a good bit of code but at least not massive changes to a lot of existing code.   Full stats for the PR are:
```
  1  0  .gitignore
  1  0  complete/_rg
  1  0  tests/regression.rs
  1  1  README.md
  2  0  crates/printer/Cargo.toml
  
  4  1  Cargo.toml
  6  0  crates/printer/src/lib.rs
  9  1  crates/printer/src/counter.rs  
  17 2  crates/core/args.rs  
  21 0  crates/cli/src/wtr.rs
  21 0  crates/core/app.rs
  23 0  crates/printer/src/hyperlink_aliases.rs
  38 2  crates/core/path_printer.rs
  45 7  crates/printer/src/util.rs 
  60 13 crates/printer/src/summary.rs
  200   58 crates/printer/src/standard.rs
  662   0  crates/printer/src/hyperlink.rs
  ```
Hopefully it is considered, it is a nice addition

---

_Comment by @ltrzesniewski on 2023-07-07 22:46_

Sorry, I thought you'd rather have a PR without merge conflicts. Yes, I totally expected to squash this into a single commit, and will do so right now since you prefer it that way.

But it depends on https://github.com/BurntSushi/termcolor/pull/65, which means it still has a few changes that can't be merged as-is (in the CI and Cargo.toml). It's ready for review though.

I understand it's a large change, but it adds a brand new feature, which I tried to keep isolated. As @mitchcapper points out, most of the new code is in a separate file. I did my best to write good code, with comments and tests, and that unfortunately doesn't make it short.


---

_Comment by @BurntSushi on 2023-07-08 00:03_

Thanks. Yes, no conflicts is good. But not with merge commits. :-) Check out the commit history of ripgrep. There are no merge commits anywhere. (Except for the beginning IIRC.)

I appreciate the work done here. I don't even know whether I'm going to support hyperlinks at all yet. I think it's likely, but it's something I have to sit down and think through myself. I understand y'all did a lot of that work already, but once I merge something like this, it then becomes my responsibility. So it has to be something I'm happy to continue to maintain indefinitely.

Anywho, I'm going through the process of clearing out the backlog so that I can put out a new release, and I plan to review this soon. But it could take some time.

The partial point of my comments here are to set expectations. I generally prefer folks to get an OK from me before spending so much time on something like this for that reason. I hate to see folks spend a bunch of time on work that doesn't wind up getting merged. I realize this is difficult to do because I have bursts of activity at unpredictable points. My schedule and workflow for my free time projects unfortunately makes a lot of my projects less friendly to contributions than I would like.

---

_Comment by @ltrzesniewski on 2023-07-08 00:31_

No worries, I understand the expectations very well, as I have similar ones for my own projects.

You seemed to be open to this feature, and I *really* wanted to have it in a tool I use every day. Besides, developing this helped me tremendously with learning Rust, so I consider this a very positive experience even if it doesn't end up being merged.

Once you think it through, I'll be happy to implement any changes you'll request until you're satisfied. That'll be another opportunity to play with the language. :slightly_smiling_face: 

---

_Comment by @dandavison on 2023-07-12 20:06_

Hi @ltrzesniewski, re https://github.com/BurntSushi/ripgrep/issues/86#issuecomment-1633123047 the JetBrains URL protocols I'm aware of are documented at https://dandavison.github.io/delta/grep.html (I don't remember where I got them from but I remember testing it with PyCharm at least).

---

_Comment by @BurntSushi on 2023-09-18 22:29_

I've done a very quick review, and I have to say, this is an extremely high quality PR. It looks like you put a ton of effort into this. I like the idea of configuring hyperlink format as you've done since it doesn't seem like we can get away with just one format.

I need to do a deeper review of how everything fits together, but I'd say I'm pretty likely to accept this PR or something close to it. You don't need to do anything from this point I think, I'll take it over from here.

I do have a high level question though: are these hyperlinks enabled by default? If so, how necessary do you think that is? My problem is that, AIUI, emitting hyperlinks requires path canonicalization, which I perceive to be a somewhat heavyweight operation. I'm not sure I want that to happen by default (even if it's just when writing to a tty).

---

_Comment by @ltrzesniewski on 2023-09-18 22:54_

Thanks for your kind words, I'm glad you like it! 🙂


> are these hyperlinks enabled by default?

Currently they're enabled by default when color is enabled and termcolor supports them (which means we're not using the old Windows console host).


> If so, how necessary do you think that is?

I think that's the most user-friendly solution. I don't expect most users to configure `--hyperlink-format` in a config file, but I suppose they'd enjoy having hyperlinks by default.


> emitting hyperlinks requires path canonicalization, which I perceive to be a somewhat heavyweight operation

This is done once per matching file, and then cached for subsequent matches in the same file: I added a `hyperlink_path` field to `PrinterPath` which stores this data.

I admit I didn't run any benchmark, but I wouldn't expect this to be an issue.



---

_Comment by @BurntSushi on 2023-09-19 02:34_

The `gethostname` dependency is a problem. ripgrep is still currently using `winapi` for Windows bindings, but `gethostname` utilizes the new `windows-targets` stuff from Microsoft. Arguably ripgrep should switch, but:

1. It's a big effort.
2. I perceive the Windows crates out of Microsoft as high-churn.
3. I'd like to investigate how much work it would be to just maintain the bindings I need myself since `winapi` is de facto unmaintained. I already maintain the `winapi-util` crate, so there is a central place for them to exist. Not great from an ecosystem perspective, but more cohesive for my use cases.

There's also the problem that `gethostname` is a micro-crate IMO, and I generally try to avoid such things. Taking a quick glance at it, it's just a couple dozen lines of not-too-tricky FFI. Plus, it panics when an error occurs. I'd rather that bubble up an error.

So I will probably:

* Add a hostname wrapper to `winapi-util`.
* Find some place to stick the Unix wrapper... Probably `grep-cli`? Not sure. (It also looks like `grep-printer` is calling the actual `gethostname` function every time it needs it, but such things should be cached once per process execution since I think it's safe to assume it won't change. Or if it will, that it's okay to use the hostname computed at startup throughout the lifetime of a typically short-lived process such as ripgrep.)

---

_Comment by @ltrzesniewski on 2023-09-19 08:59_

> It also looks like `grep-printer` is calling the actual `gethostname` function every time it needs it, but such things should be cached once per process execution since I think it's safe to assume it won't change.

It's called a single time per process, then embedded in the `HyperlinkPattern` as a constant string.

---

_@dandavison reviewed on 2023-09-19 10:35_

---

_Review comment by @dandavison on `crates/core/app.rs`:1521 on 2023-09-19 10:35_

Another type of file link that will be useful for people to construct is a link to the file and line in a remote Git hosting app, such as

```
https://github.com/BurntSushi/ripgrep/blob/master/crates/core/main.rs#L7
```

This is typically the form of link that is most useful to share with colleagues. Here the prefix portion (after host) also varies according to the local state of the repo that the file is in. I just wanted to mention this: I can imagine users asking for ways to make this convenient, but I'm not sure what if anything should be done there. Obviously it's technically possible to read the required repo metadata from disk, but that would be taking on completely new areas of responsibility. Perhaps they'd need to write a wrapper for ripgrep that sets `--hyperlink-format` dynamically per-repo.

---

_@BurntSushi reviewed on 2023-09-19 11:41_

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1521 on 2023-09-19 11:41_

I agree that such a thing could be useful, and indeed, ripgrep should not do it. It looks to be like it could be done in a wrapper script given the functionally that this PR exposes. Would you agree with that?

I actually have a little script I use for generating github permalinks (that I call from my editor): https://github.com/BurntSushi/dotfiles/blob/4f29beb508708e8a88676ffa194f0dc9c19057ae/bin/github-link

But it very intentionally only works for a tiny number of cases. It is actually a quite complex thing to do in the general case, and itself probably wants to be configurable in various ways. Definitely not going to be rolling that logic in ripgrep.

We should resist the urge to just keep piling things on. This hyperlink feature is principally useful so that folks can click to open results. And it needs to expose enough flexibility for custom links because one format won't work everywhere.

---

_Comment by @BurntSushi on 2023-09-20 15:03_

OK, [`winapi-util` now has its own safe wrapper for getting the computer name](https://docs.rs/winapi-util/latest/winapi_util/sysinfo/fn.get_computer_name.html). It also supports fetching all different possible computer names, in case we need to pivot to something other than what the `gethostname` crate uses by default. It is also fallible, although I do agree with the `gethostname` crate docs that it _should_ never return an error. But since it's interface with the OS and ripgrep runs on a lot of different systems, I'd rather play it safe. Especially since there is essentially no cost to doing so. (I'll do the same for Unix targets as well.)

---

_Comment by @BurntSushi on 2023-09-21 17:57_

@ltrzesniewski How did you come up with `wsl$/{distro}` as the hostname for WSL?

---

_Comment by @ltrzesniewski on 2023-09-21 18:16_

I use WSL so I knew that Windows lets you access files from your WSL filesystems through a share named `\\wsl$\<distro>`. Of course I tested such links: they let you open a file on a Linux filesystem in your Windows' VSCode for instance.

[Here are some docs](https://learn.microsoft.com/en-us/windows/wsl/filesystems) about the `\\wsl$` share  and file system interop with WSL.

---

_Comment by @BurntSushi on 2023-09-21 18:26_

@ltrzesniewski Hmmm okay, thank you for that link! It looks like it's a "share" though and not really a hostname? I suppose the alternative would be to introduce a new `{share}` variable or something in a `HyperlinkPattern`, but that doesn't seem great...

---

_Comment by @ltrzesniewski on 2023-09-21 18:36_

Yes, I wondered about that because it's not technically a hostname, but in the end I thought it would be best to have the same hyperlink pattern work on Linux and WSL: you may want to reuse the same config file on multiple platforms. Also, it's what just works by default on WSL.

Not sure if relevant, but note that Microsoft announced very recently [future networking changes](https://devblogs.microsoft.com/commandline/windows-subsystem-for-linux-september-2023-update) to WSL. I'm not really a networking expert so take it with a grain of salt, but it may be possible that one day WSL could be accessible through its own hostname, though I wouldn't expect for this to be enabled by default. Looks like that would require setting `networkingMode` to `mirrored`, and the post says it would enable you to "Connect to WSL directly from your local area network (LAN)".

---

_Comment by @BurntSushi on 2023-09-21 18:45_

Yeah I am very very apprehensive about shoe-horning things that aren't hostnames into a hostname placeholder. I understand it might give a little more convenience to the end user, but if WSL ever does have "real" hostnames, then fixing ripgrep will become a nightmare because there will be a bunch of end-user configuration already using `{hostname}` in WSL.

Basically, once ripgrep comes out with this hyperlink format string, then changing it in the future in a non-compatible way is going to be so nightmarish that I'm unlikely to do it. So this basically biases the design into a conservative area even if it means less convenience.

It almost looks like `wsl$\{distro}\` is a file path prefix than a hostname. So we could add a `{wslprefix}` parameter, and when `WSL_DISTRO_NAME` isn't set, it expands to the empty string. That should enable one to have the same configuration in WSL and non-WSL environments.

There is still the potential issue that `{hostname}` expands to something in WSL that doesn't work. But I don't have a WSL setup to test that.

---

_Comment by @BurntSushi on 2023-09-21 19:02_

I am also conflicted about calling `canonicalize` here, and I wonder whether it makes more sense to just build relative paths manually by just appending them to the current working directory. Otherwise, with canonicalization, users may click on a file path and have it open a path that, while it may point to the same file, could be completely different from the one they could actually see when they clicked on it. I've found this to be quite annoying and unexpected in other contexts.

Did you call canonicalize for any other reason?

---

_Comment by @ltrzesniewski on 2023-09-21 19:03_

Yeah I understand.

The problem with WSL is that its hostname is currently the same as the Windows one, so you're right: the generated link won't work if you put `{host}` in the pattern.

This can be easily fixed with a custom pattern such as `file://wsl$/Ubuntu/{file}` for instance. The `{wslprefix}` placeholder would certainly make it easier: `file://{wslprefix}/{file}` but that pattern wouldn't work on a "real" Linux.

Maybe something like `{wslhost}` which would expand to the hostname on a real Linux and to `wsl$/<distro>` on WSL (the current `{host}` implementation basically) would be better since it would always work?

That way we could have a `{host}` placeholder without any magic, and a `{wslhost}` which indicates it has some WSL support in its name.

(as an aside, I suppose you could consider `wsl$` as a "virtual" hostname, since you can access it just like an SMB share: it lists the WSL distros as subdirectories.)



---

_Comment by @ltrzesniewski on 2023-09-21 19:10_

Hmm canonicalizing seemed the right thing to do. ~Does it do anything else than concatenating the path to the current working directory while also handling `../`  and `./` correctly?~



> Otherwise, with canonicalization, users may click on a file path and have it open a path that, while it may point to the same file, could be completely different from the one they could actually see when they clicked on it

~How would that be possible?~

Ok, it expands symlinks. I suppose it would indeed be better to leave them as-is while handling `.` and `..` properly. Looks like we need [`path::absolute`](https://doc.rust-lang.org/std/path/fn.absolute.html), but it's unstable. 😞 


---

_Comment by @BurntSushi on 2023-09-21 20:02_

I don't think `std::path::absolute` works either sadly, because that leaves `..` in the path on Unix. I commented here: https://github.com/rust-lang/rust/issues/92750#issuecomment-1730218090

OK, it looks like we'll probably want to use canonicalize for now because it will at least work, but it is definitely not my preference.

---

_Comment by @ltrzesniewski on 2023-09-21 20:16_

Wow ok, I should have actually read the docs page. I must say a function named `path::absolute` that can return `..` parts is *very* surprising to me.

Having a `..` symlink not pointing to its parent dir is a pathological case, but I can understand it needs to be considered since it's possible.


---

_Comment by @BurntSushi on 2023-09-21 20:19_

Yeah. We can always revisit this too. I'll stick with `canonicalize` for now.

---

_Comment by @BurntSushi on 2023-09-21 20:26_

Actually, do we really need to get rid of `..` in the path in the first place? I assumed we did, but I just looked through [RFC 8089](https://datatracker.ietf.org/doc/html/rfc8089), and I don't see anything about resolving things like `.` and `..` out of the path. And it's worth pointing out that if there is a `.` or a `..` in the path, then it's only there because the user typed it somewhere. So retaining it seems okay? (Echoing the comment from @ChrisDenton on the issue about `std::path::absolute`.)

---

_Comment by @ltrzesniewski on 2023-09-21 20:34_

I made a quick test on Windows: `file://` doesn't handle `..` but `vscode://` does. Looks like this depends on the app associated with the URI scheme.


---

_Comment by @BurntSushi on 2023-09-21 20:36_

Oof. Wow. What an absolute mess.

---

_Comment by @ltrzesniewski on 2023-09-21 20:43_

Yes, unfortunately, `file://` handling isn't exactly great on Windows. 😞 

You may want to canonicalize on Windows only, since it's done lexically, and skip it on Linux if it handles those URIs properly (you'd have to test this, I don't have a Linux GUI). This wouldn't work on WSL with paths containing `..` along with the `file://` scheme, but you may consider this as a corner case.


---

_Comment by @ltrzesniewski on 2023-09-21 20:46_

Oh wait, actually `canonicalize` calls `GetFinalPathNameByHandleW` on Windows (which is not lexical, since the file is opened to get its handle), and `absolute` calls `GetFullPathNameW` (which seems to be lexical).

---

_Comment by @Frenzie on 2023-09-21 20:53_

> skip it on Linux if it handles those URIs properly (you'd have to test this, I don't have a Linux GUI).

Yes, at a glance it does what you expect. (I.e., file://tmp/lala/../bla.txt opens /tmp/bla.txt), but I might not be testing the precise context you expect.

I'd swear Explorer did too for that matter.

---

_Comment by @BurntSushi on 2023-09-21 20:55_

Yeah, even though I am on Linux, I don't really have links-in-terminals setup. I don't expect to be a user of this feature in general. The only "GUI" I use is a web browser, and very occasionally `git gui`. Everything else is in the terminal, and terminal hyperlinks seem to be most useful for opening them in other non-terminal GUI applications.

So I'm probably just going to have to stick with canonicalize for now, since that seems to give the best chance of the link actually working.

---

_Comment by @ChrisDenton on 2023-09-21 21:00_

Just to be clear, Windows is the easy case in terms of `..`. A `..` component will always be resolved lexically so if all else fails you could even do it manually. Unixes are trickier because `..` is a link. On Linux there's not a good solution if a protocol does not support `..` components. Canonicalization is really the only option unless you're ok with potentially opening a different file to the one you expected to.

---

_Comment by @BurntSushi on 2023-09-21 21:03_

Right. The issue is knowing (on Unix, where handling `..` is trickier) whether the application that handles the link supports `..` in the path or not. Maybe it works just about everywhere and maybe not. I dunno.

---

_Comment by @Frenzie on 2023-09-21 21:06_

It's not quite clear to me where this theoretical `..` would be coming from btw? User config only?

---

_Comment by @BurntSushi on 2023-09-21 21:09_

Just what the user types. For example, `rg foo ../sibling-of-parent/dir/foo`.

---

_Comment by @BurntSushi on 2023-09-22 17:56_

@ltrzesniewski In your PR, I found this comment about constructing the hyperlink on Unix:

```rust
        // On Unix, this function returns the absolute file path without the
        // leading slash, as it makes for more natural hyperlink format, for
        // instance:
        //
        //   file://{host}/{file}   instead of   file://{host}{file}
        //   vscode://file/{file}   instead of   vscode://file{file}
        //
        // It also allows for the format to be multi-platform.
```

I am trying to decide whether it makes sense to strip the leading `/` from a file. On the one hand, I see the convenience in how one writes the hyperlink format strings when stripping it. On the other, it feels strange to me, because at least on Unix, an absolute path without a leading slash is a bit of a weird thing. Moreover, RFC 8089 defines the `absolute-path` component to include the leading slash (including on Windows). So I'm not sure what was meant by the format being multi-platform. I think, for example, that `vscode://file{file}` would work on both Unix and Windows. So would `file://{file}` and `file://{host}{file}` (modulo the bit about certain things on Windows not liking the host).

I guess from my view, I would rather the variables be easier and more intuitive to understand as individual pieces than to make the format as a whole a little more natural to look at. When someone is trying to write a new hyperlink format, it will require reasoning about each individual part, and `/foo/bar/baz` except without the leading `/` just seems a bit too unexpected from my perspective.

I'm also thinking of renaming `{file}` to `{path}`. I think the latter is a bit more precise and is more in line with the terminology used by RFC 8089.

---

_Comment by @BurntSushi on 2023-09-22 18:00_

If we leave the leading slash on Unix, I think the implication is that I would _add_ a leading slash on Windows. Which I guess also feels a little unintuitive to me from the Windows perspective. There, you'd expect an absolute local path to start with a drive letter I think. But maybe this is less bad than removing the leading slash on Unix?

Another way to think about it is, as I said above, that `{file}` is really just `path-absolute` from RFC 8089 (and actually defined in [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986)). And in that context, it has the leading slash.

---

_Comment by @ltrzesniewski on 2023-09-22 19:32_

Well TBH I'd prefer removing the leading slash, because it feels more natural and intuitive that way IMO.

You'd have to write `editor:/{file}` instead of `editor://{file}`. That `editor:/` part feels wrong with a single slash, and using `editor://{file}` would produce `editor:///foo/bar/baz` with three slashes.

`vscode://file{file}` wouldn't work on Windows as that would generate `vscode://fileC:/foo/bar/baz` - you'd have to prefix the path with a slash on Windows as you said, which feels wrong.

On the other hand, if you don't prefix the path with a slash on Windows, the patterns would become platform-specific, and you'd have to write them twice in `hyperlink_aliases.rs` with `#[cfg(unix)]` and `#[cfg(windows)]`.

But more importantly, you *know* the path will be canonicalized and are reasoning with that in mind, but I wouldn't expect the users to think about this detail. They may expect `{file}` to generate a relative path (just like what's shown in the terminal), one which doesn't start with a slash, or simply not think about this. And I certainly wouldn't expect them to compare the hyperlink format against RFC 8089. 😅 

That's why I think a pattern such as `file://{host}/{file}` feels more natural.

> I'm also thinking of renaming `{file}` to `{path}`.

As you prefer. Both seem fine to me.


---

_Comment by @BurntSushi on 2023-09-22 20:07_

Yes, I would add a leading slash to Windows paths. I've tried this out and I like it best among all choices that I can think of. I don't see any one choice that leads to good and intuitive behavior in all cases.

I don't think there are any cases where you need to write `editor:/{path}` when `{path}` contains a leading slash. Here are my rewritten aliases for example:

```
pub(crate) const HYPERLINK_PATTERN_ALIASES: &[(&str, &str)] = &[
    #[cfg(not(windows))]
    ("file", "file://{host}{path}"),
    #[cfg(windows)]
    ("file", "file://{path}"),
    // https://github.com/misaki-web/grepp
    ("grep+", "grep+://{path}:{line}"),
    ("kitty", "file://{host}{path}#{line}"),
    // https://macvim.org/docs/gui_mac.txt.html#mvim%3A%2F%2F
    ("macvim", "mvim://open?url=file://{path}&line={line}&column={column}"),
    ("none", ""),
    // https://github.com/inopinatus/sublime_url
    ("subl", "subl://open?url=file://{path}&line={line}&column={column}"),
    // https://macromates.com/blog/2007/the-textmate-url-scheme/
    ("textmate", "txmt://open?url=file://{path}&line={line}&column={column}"),
    // https://code.visualstudio.com/docs/editor/command-line#_opening-vs-code-with-urls
    ("vscode", "vscode://file{path}:{line}:{column}"),
    ("vscode-insiders", "vscode-insiders://file{path}:{line}:{column}"),
    ("vscodium", "vscodium://file{path}:{line}:{column}"),
];
```

> But more importantly, you know the path will be canonicalized and are reasoning with that in mind, but I wouldn't expect the users to think about this detail. They may expect {file} to generate a relative path (just like what's shown in the terminal), one which doesn't start with a slash, or simply not think about this. And I certainly wouldn't expect them to compare the hyperlink format against RFC 8089.

The docs for the format will need to explicitly mention that `{path}` is absolute and starts with a leading slash. I could also rename it (or add an alias) called `{path:absolute}` to make things even clearer.

My reasoning for sticking closer to the RFC is that, in my experience with these sorts of things, the RFC winds up serving as a lowest common denominator. And if we introduce concepts that diverge from the RFC, then it's plausible they can be combined in ways we didn't anticipate (perhaps with future features) that lead to a worse experience than if we just stuck with the RFC. This is a very hand-wavy argument and I could absolutely be wrong, but this is just what my experience says in these sorts of scenarios.

Finally, ideally, most users won't need to actually write their own hyperlink format and can instead select from one of the existing pre-defined formats. I expect that list to grow over time too.

---

_Comment by @BurntSushi on 2023-09-22 20:39_

The next change I'm looking to make involves the syntax of the format. I like the interpolation syntax, `{var}`, for variables. But, I think we'll want special syntax for aliases. For example, `[vscode]` instead of `vscode`. The special syntax makes it a little less convenience, but provides an opportunity for improving failure modes. For example, this is not a great failure mode:

```
$ rg 'Result<Hyperlink' --hyperlink-format vscodee
the {path} placeholder is required in a hyperlink format
```

And we'll also want to make it possible to escape special characters. Just like `$` can be escaped with `$$` in ripgrep's `-r/--replace` flag, we can allow doubling like `[[` and `{{` to stand-in as a literal `[` and `{`.

---

_Comment by @ltrzesniewski on 2023-09-22 20:52_

> The special syntax makes it a little less convenience, but provides an opportunity for improving failure modes.

Maybe we could improve the error message instead, for instance by providing a list of known aliases when there's no `{path}` placeholder? TBH I wanted to do that but didn't implement it since I wasn't sure the PR would be accepted.

But since those aliases would mainly be used in a config file, the `[]` characters wouldn't cause much inconvenience I suppose.

Also `--hyperlink-format [none]` doesn't feel right IMO (for the `none` alias I mean).


> And we'll also want to make it possible to escape special characters.

Yes, I also thought about that, but in the end I didn't think it was necessary since I didn't expect `{` to be included literally in a URL.

I suppose `[` wouldn't really be a special character since it would only be used for aliases?


---

_Comment by @BurntSushi on 2023-09-22 21:52_

My frame of reference here are these two points:

1. We should make failure modes good because this is a fairly complex feature.
2. Once we ship this, making incompatible changes will become nearly impossible. So if it ever turns out that someone needs to write a literal `{` they will be shit out of luck unless we handle that case initially.

I'll continue to noodle on the syntax.

---

_Comment by @ltrzesniewski on 2023-09-23 05:11_

Actually, a simple solution could be:
- if there's no `:` in the argument, then treat it as an alias, and show an appropriate error when unknown
- if there is one, treat it as a pattern and require `{path}`

---

_Comment by @BurntSushi on 2023-09-23 19:41_

I wonder if it makes sense to include `wsl$/Ubuntu` as a prefix of `{path}` automatically whenever `WSL_DISTRO_NAME` is set. But I don't think that's right. And I note that _most_ of the aliases in this PR, at least as written, won't include the `wsl$` prefix at all because, in this PR, it's only inserted if `{host}` is present. And on Windows, by default, in this PR, `{host}` is absent from the default hyperlink format.

So I guess, under what conditions does this `wsl$/{distro}` prefix need to be inserted? I wonder if it makes sense to ship this initially without any WSL-specific knowledge at all, and let folks configure it as needed until we can get a firmer grasp.

.....

Now that I've typed all of that out, I realize the `cfg(windows)` is probably not set in WSL, is it? Instead, I imagine `cfg(unix)` is. Which means the default hyperlink format does include the WSL prefix.

I could find very little documentation or acknowledgment of how WSL paths fit into file URIs. Maybe I'm not looking in the right place.

---

_Comment by @ltrzesniewski on 2023-09-23 20:10_

> Now that I've typed all of that out, I realize the `cfg(windows)` is probably not set in WSL, is it? Instead, I imagine `cfg(unix)` is.

Yes, WSL2 runs a real [Linux kernel](https://github.com/microsoft/WSL2-Linux-Kernel) under Windows, and it can execute standard Linux binaries. We're in `cfg(unix)` territory.



> I wonder if it makes sense to ship this initially without any WSL-specific knowledge at all, and let folks configure it as needed until we can get a firmer grasp.

Sure, you can just skip it. I added this as a little convenience feature because I thought it would be nice to have and easy to implement. I suppose people who use ripgrep under WSL will know how to customize their hyperlink patterns. Sorry for the additional trouble. 😅 


---

_Comment by @BurntSushi on 2023-09-23 20:30_

No no, not trouble at all. It's great that you put that in there, because it would have been an unknown unknown otherwise.

I think what I'll do is provide a `{wslprefix}` variable that expands to `wsl$/{distro}` where `{distro}=WSL_DISTRO_NAME`, but only when `{distro}` is non-empty. However, I'll stop short of using `{wslprefix}` anywhere in the default formats and let users add it in themselves. Once we get a feel for what works well, we should be able to add it to some or all of the default formats. (Or potentially even make it a prefix of `{path}` in all cases? Dunno.)

---

_Comment by @ltrzesniewski on 2023-09-23 20:40_

If you're curious about WSL paths, here's an example of what the Windows Explorer shows:

![image](https://github.com/BurntSushi/ripgrep/assets/7913492/3c79c3de-d577-492f-aa0f-d37e0bed75a4)

It shows `wsl$` as a CIFS server on the network, and `Ubuntu` (in my case) as a share on that server. The share contains the root of the Linux filesystem.

Since you can get access to the Linux filesystem in the same way as a remote CIFS share on the network, `wsl$` being a virtual hostname, I replaced `{host}` with `wsl$/{distro}` in this PR.

---



> I think what I'll do is provide a `{wslprefix}` variable that expands to `wsl$/{distro}` where `{distro}=WSL_DISTRO_NAME`, but only when `{distro}` is non-empty.

That would be nice to have, since you can't expand environment variables in ripgrep config files, so you'd have to hardcode the distro name without it.



> However, I'll stop short of using `{wslprefix}` anywhere in the default formats and let users add it in themselves.

Yes, you'd have to replace `{host}` with `{wslprefix}` in any case - using those placeholders together doesn't really make sense.

That's why I think having a `{wslhost}` placeholder which expands to either `{host}` or `wsl$/{distro}` would be a better option, since you'd be able to use the same ripgrep config file on WSL and on a real Linux.

And I agree that having just a plain `{host}` by default is the best (least surprising) option. 👍



> Or potentially even make it a prefix of `{path}` in all cases? Dunno.

Yeah, I don't know either. 😕 

Both options would work, but I initially put it into `{host}` because `wsl$` is a kind of virtual host.

I suppose having a WSL-specific placeholder would be the better option, as if you prefix it to `{path}` there wouldn't be a way to opt out of this.


---

_Comment by @BurntSushi on 2023-09-23 20:49_

Gotya. Yeah, there are a lot of options here. We could also add a `{wslpath}` variable or something. I'm also open to a `{wslhost}` variable. But I think just starting with `{wslprefix}` for now is the most conservative option. It gives us some flexibility to maneuver in the future.

---

_Closed by @BurntSushi on 2023-09-25 18:39_

---

_Comment by @garawaa on 2024-07-12 16:39_

I implemented a gui wrapper to it. you can download it at https://github.com/garawaa/ripgrep-gui
### with result files:
- open in system default editor or viewer. 
- copy file path
- show in explorer(windows)

---
