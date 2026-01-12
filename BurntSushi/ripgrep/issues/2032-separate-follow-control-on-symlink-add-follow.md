```yaml
number: 2032
title: separate follow control on symlink, add --follow_link_file?
type: issue
state: closed
author: ryanhuanli
labels:
  - wontfix
assignees: []
created_at: 2021-10-21T19:42:21Z
updated_at: 2023-11-13T15:51:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2032
synced_at: 2026-01-12T16:13:24Z
```

# separate follow control on symlink, add --follow_link_file?

---

_@ryanhuanli_

I'm working in a version control system that all files are symlinks to a shared cache system (to save space).
So I need to use -L (--follow), otherwise ripgrep hit nothing.

-L turning on two things together:
1. when linking to file, it greps
2. when linking to directory, it recursively search within

However it's very common that some people have links pointing to 
other directory inside workarea (cause duplication or deep recursive) or something outside workarea which I don't really care, 
so what I really need is only #1(file), but not #2(dir).

fdfind have a similar case where it handles with '-type l', which ONLY do #1 (to include links pointing to file), 
and -L only do #2(dir recursively).

It will be great if I can control this separately in ripgrep, or if there is any way to workaround .
I currently have to add all these dir links to ignore list whenever I'm getting hit ... 

Thanks!

 


---

_Comment by @BurntSushi on 2021-10-21 20:51_

Could you please provide a reproduction along with actual output and expected output? Just to make sure we are on the same page.

IMO, this is not actually "very common," so I'm skeptical of the motivation for this. Certainly, the meaning of `-L` at least isn't going to change. Adding new flags seems like a pretty subtle thing for this.

The only work arounds I can think of at present are to use ignore files (as you point out), or to use hard links for files.

---

_Comment by @ryanhuanli on 2021-10-25 02:17_

> Could you please provide a reproduction along with actual output and expected output? Just to make sure we are on the same page.

Sure here is a case in your regression test format:
1/2 are current behavior.
and 3 is what I hope it could do w/ new option

```rust
rgtest!(r9999, |dir: Dir, mut cmd: TestCommand| {
    dir.create_dir ("regular_dir");
    dir.create     ("regular_dir/regular_file", "line1\nkeyword\nline2\n");
    dir.link_file  ("regular_dir/regular_file", "regular_dir/link_file", );
    dir.link_dir   ("regular_dir", "link_dir");

/*
// 1. -L off (default) : no file link or dir link
    cmd.args(&["--files"]);
    eqnice!("regular_dir/regular_file\n", cmd.stdout());

// 2. -L on : both file link and dir link
    cmd.args(&["--files", "-L"]);
    eqnice!("\
link_dir/link_file
link_dir/regular_file
regular_dir/link_file
regular_dir/regular_file
"        , cmd.stdout());
*/

// 3. --follow_link_file : only include link to file
    cmd.args(&["--files", "-follow_link_file"]);
    eqnice!("\
regular_dir/link_file
regular_dir/regular_file
"
        , cmd.stdout());
});
```

> IMO, this is not actually "very common," so I'm skeptical of the motivation for this. 

I suppose you are talking about all files being link, 
yes, it's not common in software world, 
but I'm in Semiconductor Industry, we have a lot of huge files, 
so nearly all version control systems (DesignSync/SOS, etc.) chosen by the companies have link cache system, 
to take less disk space and checkout time.

> Certainly, the meaning of `-L` at least isn't going to change. Adding new flags seems like a pretty subtle thing for this.

Agree, I'm not suggesting changing default, just wish more precise control on links w/ new option.


---

_Comment by @ComedyTomedy on 2022-07-22 14:58_

I was about to open a new issue, when I saw this very similar one.

Is there any reason why the default behaviour (without flags) shouldn't be to treat symlinks that point to regular files as if they were regular files themselves? Soft links to directories would of course still be skipped, as suggested above.

> IMO, this is not actually "very common," so I'm skeptical of the motivation for this.

A lot of people *do* symlink regular files -- It's quite common to symlink dotfiles in order to synchronise or version control them without checking in (or synching) one's entire home directory. GNU Stow would be one example, but there are many others. I believe Nix does a system-wide version, so for NixOS users `rg` might be unusuable or at least silently give "wrong" results.

Equally if symlinking files isn't common among software developers, there might be little negative impact from changing the default behaviour ;)

For now I'll add `-L` to an alias, but following symlinks to other parts of the filesystem isn't usually what I'd want (ideally)

---

_Comment by @BurntSushi on 2022-07-22 15:56_

> Equally if symlinking files isn't common among software developers

That isn't what I said. Obviously symlinking itself is common. What I was saying wasn't common was this: "all files are symlinks to a shared cache system."

> Is there any reason why the default behaviour (without flags) shouldn't be to treat symlinks that point to regular files as if they were regular files themselves?

One reason is probably that this is how existing recursive grep tools work. The other reason is probably the reason why those grep tools chose that behavior: searching symlinks by default, even if just limited to files, is not unlikely to lead to undesirable behavior, like searching and reporting the results for identical files multiple times.

Personally, I'd still rather that folks used the `-L` flag here and used an ignore file to tell ripgrep to ignore whatever directories you don't want. I am not particularly keen on adding a new flag for this.

---

_Comment by @ComedyTomedy on 2022-07-22 18:43_

That's reasonable. `-L` will pretty much always do what I want anyway - just wanted to make the suggestion in case it had been overlooked. Thanks for the quick and considered response!

---

_Closed by @BurntSushi on 2023-11-13 15:51_

---

_Label `wontfix` added by @BurntSushi on 2023-11-13 15:51_

---
