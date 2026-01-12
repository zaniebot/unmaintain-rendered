```yaml
number: 196
title: "Support persistent configuration (`.rgrc`?)"
type: issue
state: closed
author: SimenB
labels:
  - question
assignees: []
created_at: 2016-10-27T15:17:49Z
updated_at: 2018-02-15T18:30:04Z
url: https://github.com/BurntSushi/ripgrep/issues/196
synced_at: 2026-01-12T16:13:21Z
```

# Support persistent configuration (`.rgrc`?)

---

_@SimenB_

I'd love to keep different types for instance, and not have to specify `--type-add` each time.

My specific case is type zsh which searches zsh config files (`.zshrc` `*.zsh`, and some custom `.zsh*`).

Maybe it could support more stuff like `no-heading`, `--heading` etc as well, which would allow people to customize rg more.

(I know I can just `alias rgzsh="rg --type-add 'zsh:*.zsh' --type-add 'zsh:.zshrc' --type zsh"`, but meh)

Loving the tool, btw! 


---

_Comment by @BurntSushi on 2016-10-27 15:22_

Can you make do with creating an alias? Or even a script in your `~/bin`? For example, I have `~/bin/rgp`:

```
#!/bin/sh

exec rg -p "$@" | less -RFX
```


---

_Label `question` added by @BurntSushi on 2016-10-27 15:22_

---

_Comment by @SimenB on 2016-10-27 15:25_

I just updated the OP with alias stuff 😄 

And yes, I could, but then I have to remember my aliases (woe is me, right?). Doing `rg -tzsh` is more natural when I've gotten used to types than doing `rgzsh` or whatever.

`rgp` makes sense, though!


---

_Comment by @BurntSushi on 2016-10-27 15:27_

Why don't you just alias `rg` itself? This works, for example:

```
alias rg="rg --type-add 'zsh:*.zsh' --type-add 'zsh:.zshrc'"
```

Then you can do `rg -tzsh`.

(Also, `zsh` seems like a common enough type that it should be part of ripgrep proper! :-))


---

_Comment by @SimenB on 2016-10-27 15:31_

Aliasing rg seems pretty good!

I can provide a PR for zsh type (just add an entry here I suppose: https://github.com/BurntSushi/ripgrep/blob/master/src/types.rs)

But I'd still really like for that to be defined for rg, and not drowned amongst all of my other aliases (I still do `alias | rg whatever` since I forget. I'd like to think `rg --type-list` is nicer (and yes, I realize `rg --type-list` would work with your suggested `rg` alias)).


---

_Comment by @BurntSushi on 2016-10-27 15:35_

> I can provide a PR for zsh type (just add an entry here I suppose: https://github.com/BurntSushi/ripgrep/blob/master/src/types.rs)

Sounds good!

> But I'd still really like for that to be defined for rg, and not drowned amongst all of my other aliases (I still do alias | rg whatever since I forget. I'd like to think rg --type-list is nicer (and yes, I realize rg --type-list would work with your suggested rg alias)).

You can also do `command -V rg` to see the alias.


---

_Comment by @SimenB on 2016-10-27 15:40_

Conclusion: I can achieve what I want using simple aliases. I'd still like an `.rgrc` file though, to match `.ackrc`, `.ptconfig.toml` etc. 😄  I do understand if you feel like aliasing is enough, though


---

_Comment by @SimenB on 2016-10-27 15:42_

Or maybe support `$RG_OPTIONS` to match `grep`? It just seems more explicit than aliasing rg itself.


---

_Comment by @BurntSushi on 2016-10-27 15:43_

I do feel like aliases are a pretty good solution to this problem, and I'd like to stick with them for now.

I think we should leave this ticket open so that others can chime in. For example, aliases may not be easy to define on all platforms.


---

_Comment by @SimenB on 2016-10-27 16:05_

BTW, is it possible to _not_ recurse through directories? Case in point is searching for something in a zsh file in`$HOME`. Having `rg` recurse through everything is slow as hell (naturally).

Additionally: Is it possible to add ignore patterns? I managed to hit chrome application cache, which is an absolutely monstrous file


---

_Comment by @BurntSushi on 2016-10-27 16:15_

> BTW, is it possible to not recurse through directories?

Yes:

```
    --maxdepth NUM
        Descend at most NUM directories below the command line arguments.
        A value of zero only searches the starting-points themselves.
```

> Additionally: Is it possible to add ignore patterns? I managed to hit chrome application cache, which is an absolutely monstrous file

Yes. Create a `.ignore` file and add patterns to that. (Or add them to your `.gitignore`.)


---

_Comment by @SimenB on 2016-10-27 16:17_

Ok, but not add ignore to the execution itself?

Will an `.rgignore` file be respected? I just feel having an `.ignore` file in `$HOME` might be confusing if it's just used for rg


---

_Comment by @BurntSushi on 2016-10-27 16:21_

> Ok, but not add ignore to the execution itself?

I don't understand the question.

> Will an .rgignore file be respected?

Please use `.ignore`. `.rgignore` is deprecated. Where did you see `.rgignore`?

> I just feel having an .ignore file in $HOME might be confusing if it's just used for rg

I don't understand what's confusing about it. The Silver Searcher supports the same format.

The `.ignore` file can be anywhere. It could live right next to the thing you want to ignore. It works just like `gitignore`.


---

_Comment by @BurntSushi on 2016-10-27 16:21_

Perhaps you're looking for the `-g/--glob` flag?


---

_Comment by @SimenB on 2016-10-27 16:25_

> I don't understand the question.

I want to do e.g. `rg --ignore-dir 'Application Cache'` or something like it, not have to create a file on disk.

> Where did you see `.rgignore`?

Nowhere, it was just a name proposal. I can guess you don't like it though 😁 

> Perhaps you're looking for the `-g`/`--glob` flag?

That should work! Just `rg -g '!Application Cache/'`?

BTW, using `--maxdepth 0` never works, I always get

> No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug

Doing `--maxdepth 1` seems to just search current dir, though. Bug or should the man file get an update?

![image](https://cloud.githubusercontent.com/assets/1404810/19776062/f39c80b6-9c72-11e6-8aa6-88a25fc66abe.png)


---

_Comment by @BurntSushi on 2016-10-27 16:30_

> That should work! Just rg -g '!Application Cache/'?

You probably want `rg -g '!**/Application Cache/**'`, since the glob is applied to the full file path.

> Doing --maxdepth 1 seems to just search current dir, though. Bug or should the man file get an update?

Seems correct to me. Read the docs again please:

```
    --maxdepth NUM
        Descend at most NUM directories below the command line arguments.
        A value of zero only searches the starting-points themselves.
```

A **value of zero** only searches the starting points. So if you run `rg --maxdepth 0 foo`, then the starting point is the current directory, which obviously can't be searched, since it isn't file. If you do `rg --maxdepth 1 foo`, then it descends one level. These are the same semantics as `find`, e.g., see the output of `find ./ -maxdepth 0` and `find ./ -maxdepth 1`.


---

_Comment by @SimenB on 2016-10-27 16:39_

Ah of course, so `--maxdepth 0` is only when searching a file? Makes sense when I read it again!

Thanks for answering all of my questions, super helpful! 😄  Especially the `-g` one will be useful


---

_Comment by @BurntSushi on 2016-10-27 16:43_

> Ah of course, so --maxdepth 0 is only when searching a file?

`--maxdepth` can be used any time. All it does is control the number of times it descends into a directory. If the value is zero, then it will _never_ descend into any directories. Therefore, since `rg foo` is equivalent to `rg foo ./`, the directory `./` isn't descended into, so there's nothing to search.

It's most likely that a value of `0` is only useful in a generic context, e.g., when you don't know if your file parameters will be files, directories or a mixture and you don't want to do any directory recursion.


---

_Comment by @SimenB on 2016-10-27 16:47_

A more useful thing for `--maxdepth` doc might be "A value of one will only search files in the passed in directory" or something like it. But now I know, so I'm good 😄 


---

_Comment by @kastiglione on 2016-10-28 18:53_

I've hit one issue with using an alias: zsh completion doesn't work when I've shadowed the `rg` command with a `rg` alias. When I unalias `rg` the completion works. There's probably some way to get the zsh completion system to handle this case, but I don't yet know how to do that.

EDIT: The answer is `setopt complete_aliases`.


---

_Comment by @gvol on 2016-11-23 05:08_

Just my two cents, but I really like .ackrc because it's used at the command line and when invoked by other tools like Emacs (a place that aliases don't work).

---

_Comment by @BurntSushi on 2016-11-23 10:38_

In that case, can you put a wrapper script in your $HOME/bin?

On Nov 23, 2016 00:08, "Ivan Andrus" notifications@github.com wrote:

> Just my two cents, but I really like .ackrc because it's used at the
> command line and when invoked by other tools like Emacs (a place that
> aliases don't work).
> 
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-262435651,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34niAsM6kJe2XlAWQGP9sFz0-gg5dks5rA8pWgaJpZM4Kieh1
> .


---

_Comment by @BurntSushi on 2016-11-24 22:49_

I know of at least one Windows user who wants this pretty badly. (cc @retep998) It sounds like the claim is that, on Windows at least, defining aliases and/or wrapper scripts is neither common nor convenient to do. Is this others' experience as well? Could someone please outline exactly what one has to do on Windows to create a wrapper script? Is it really an unreasonable expectation?

If we go down this path, we should consider the following:

* Whether `clap` can give this to us for free some day: https://github.com/kbknapp/clap-rs/issues/748
* If clap doesn't, then we need to come up with our own specification.
* For example, where are config files read from? On *nix, I'm familiar with the xdg basedir specification, so we could follow that, but I don't know what the idioms are on Windows.
* We may need to solve problems like "I set `--context 5` in my config file, but I want to override occasionally with `--context 0` on the command line." We can't just trivially merge the config with the CLI parameters since `rg --context 5 --context 0 foo` is illegal today. This is probably true of several other flags, and many other flags probably need the opposites added. e.g., If `--no-ignore` is in the config file, how does a user re-enable ignore functionality?

IMO, the above implies *significant* complexity and likely an easy source of bugs. If we moved forward with this, I think I'd have to demand a rock solid specification (which I'm not inclined to work on any time soon) or some kind of conscious concessions at least (like, "not every option is overridable," for example, which seems like bad UX honestly).

---

_Comment by @BurntSushi on 2017-01-12 15:10_

#314 asks for project specific configs. Part of the initial design for this should address that (which may include punting on it).

(I still remain skeptical of the whole idea. See my [previous comment](https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-262853109).)

---

_Comment by @kbknapp on 2017-01-13 05:11_

> We may need to solve problems like "I set --context 5 in my config file, but I want to override occasionally with --context 0 on the command line." We can't just trivially merge the config with the CLI parameters since rg --context 5 --context 0 foo is illegal today. This is probably true of several other flags, and many other flags probably need the opposites added. e.g., If --no-ignore is in the config file, how does a user re-enable ignore functionality?

This is the primary discussion in kbknap/clap-rs#748

I've got some ideas for implementation I just haven't had to time to sit down and do it. The way it'll end up working if you use the `clap` implementation for configs is `ripgrep` would be responsible for reading in the config file off the disk (initially TOML) and hand off a supported struct to `clap` as an "external argv" (such as a `TomlTable`). `clap` then handles all the merging/overriding of options where whats passed on the command line manually will override any of these "external argv"s. 

Any number of these external argv's could be given to `clap` (think global and per project, such as a current directory `.rgrc`), and will be evaluated in order for example:

```rust
let global = /* read global .rgrc and parse into TOML */;
let project = /* read current dir .rgrc and parse into TOML */;
app.external_argv(global)
    .external_argv(project)
    .get_matches()
```

(The name `external_argv` is still somewhat bikeshedable and will probably be implemented with generic `fn external_arg<T: Into<ArgvExt>>(mut self, argv: T) -> Self` and providing impls to convert from `TomlTable`, `&str` as the two initially supported types of external argvs. The other option is to have separate methods for all the diferent types of "external argvs" which is easier to document for things like simple strings).

Where the command line overrides PROJECT and PROJECT overrides GLOBAL.

The alternative is passing `clap` a `Path` and having it do all the reading and such, but I personnally would rather leave that to the consuming binary since there's so much that could go wrong, or be done differently, or custom errors, just too much I don't particularly want to do with `clap`. But parsing a `Toml`/`Yaml`/`&str`/etc against the valid args seems like it's well within the scope of `clap`.

There are some UX oddities which are initilaly may seem confusing, but IMO make sense when you think about them.

Most importantly, what `clap` calls flags would need to be considered *very* carefully. Because putting `--foo` in a config is impossible to "undo" unless there is a corresponding [overriding or negating flag](https://docs.rs/clap/2.20.0/clap/struct.Arg.html#method.overrides_with). Reason being, if the user doesn't provide the flag manually do they simply agree with the default, or did they leave off the flag becasue they *don't* want to use it? 

Positional/Free args wouldn't be supported at all and would have to be input on the command line (although they could have default values). This is to prevent a user running `rg <foo>` and not realizing it's actually running `rg <bar> <foo>` because `<bar>` is defined in a config somewhere. 

---

_Comment by @BurntSushi on 2017-01-13 11:55_

@kbknapp Thanks for chiming in! As far as the mechanics go, all of that sounds very reasonable. It sounds like we both agree on what the essential challenges are too. For example, it would be unfortunate to have to add negations of every flag. However, the *biggest* reason why I'd find that unfortunate is because of the space it would take up in the output of `--help`, and less because of the presence of the flag itself. (Although that's still painful, unless there's a way to encapsulate the negation on the `clap` side of things.) That might suggest a possible avenue forward, but technically, a good chunk of it is possible today since `clap` supports adding flags that are hidden from the output of `--help`.

---

_Comment by @kbknapp on 2017-01-13 16:59_

> Although that's still painful, unless there's a way to encapsulate the negation on the clap side of things.

I see two possibilities with pros and cons to each (also, both would be possible concurrently).

 * Add `Arg::overridable(bool)` which would only apply to flags and automatically creates a negation flag. There could also be a `Arg::overridable_hidden(bool)` that does the same but hides the negation flag from `--help` output.
 * Add `AppSettings::OverridableFlags` which does the above, but automatically for all flags. Again there could be a counterpart of `AppSettings:OverridableFlagsHidden` which hides all these negation flags.

For the specifics, a negation flag would simple take the long version of the regular flag and pre-pend `no`, for exmaple `--follow` would get `--no-follow`. If a flag only specifies a short version (`ripgrep` doesn't have any examples of this), the `no` would be prepended to the short such as `-L` gets `--no-L`.

My personal opinion is that having hidden negation flags is best, as it doesn't clutter the `--help` output and could quietly mentioned where applicable, such as when speaking of config files `To override a flag specified in a config file, prepend --no to the flag, such as --no-follow`. Done. It's not something that everyone needs to know unless they're messing with things like aliases and config files.

Of course, when using something wide sweeping like `OverridableFlagsHidden` you'll end up with flags that technically have corresponding negation flags that are functionally useless, but pragmatically it shouldn't affect anything.

If something like that would help this issue (of course once I finish implementing the external argv portion), I'd be happy supporting that as they'd both be technically very easy to implement.

---

_Comment by @BurntSushi on 2017-01-13 17:06_

@kbknapp That sounds plausible. But if a flag is already preceded with `no-` (e.g., `--no-ignore`), then you'd wind up with `--no-no-ignore`. :P 

Personally, I think the external argv portion is the most important. I'd be OK just starting there and seeing how it all shakes out, even if that means me adding the `no` flags manually.

---

_Comment by @BurntSushi on 2017-01-13 17:07_

> But if a flag is already preceded with no- (e.g., --no-ignore), then you'd wind up with --no-no-ignore. :P

Note that this could actually be reasonable. It seems funny, but it's consistent and predictable.

---

_Comment by @ssokolow on 2017-01-31 14:21_

@SimenB If it helps, I wrote an `rg` wrapper function for zsh (which should work on bash with little or no modification) which implements `~/.config/rgrc` as a way to prepend arguments onto my `rg` command line.

(It also maps `-G ...` to `-g !...` to work around a zsh footgun regarding starting arguments with `!`)

The alias:
https://github.com/ssokolow/profile/blob/master/home/.zshrc.d/rg

Example rgrc:
https://github.com/ssokolow/profile/blob/master/home/.config/rgrc

I'm still looking for documentation on how to generate shell completions when building from source, so I don't know what effect it'll have on completion support in its current state.

---

_Comment by @BurntSushi on 2017-02-02 12:18_

@ssokolow That's a nice hack, and I'd encourage folks to try and use it if it helps.

The shell completion files are generated automatically and placed in `target/release/build/ripgrep-*/out/`.

---

_Comment by @mcepl on 2017-03-24 17:02_

Without vote how this should be resolved, could I at least ask that if there is any configuration could it be stored in [XDG](https://standards.freedesktop.org/basedir-spec/basedir-spec-latest.html) compliant location (e.g., ``~/.config/ripgrep``), please?

---

_Comment by @BurntSushi on 2017-03-24 17:15_

@mcepl If ripgrep has it, then it will definitely satisfy XDG on platforms where that makes sense. On platforms where it doesn't make sense, I don't know what to do. See: https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-262853109

---

_Comment by @mcepl on 2017-03-24 19:35_

Yeah, that’s a problem. I have decided in [jbrout](https://gitlab.com/jbrout/jbrout/blob/master/jbrout/jbrout/conf.py) that we use ``glib`` anyway (because of ``Gtk``), so I can use whatever directories it gives me on Windows or Mac, but I guess you don’t want to depend on Glib.

---

_Comment by @ssokolow on 2017-03-24 21:45_

> On platforms where it doesn't make sense, I don't know what to do.

The [app_dirs](https://crates.io/crates/app_dirs) crate will handle that for you.

Just specify `AppDataType::UserConfig` and you'll get `${XDG_CONFIG_HOME:-$HOME/.config}/ripgrep` on XDG platforms, `$HOME/Library/Application Support/ripgrep` on OSX, and `%APPDATA%\BurntSushi\ripgrep` on Windows.

**EDIT:** ...or you could step up a layer and use the [preferences](https://crates.io/crates/preferences) crate which depends on it.

---

_Comment by @BurntSushi on 2017-03-24 21:46_

> I guess you don’t want to depend on Gli

You could say that. :-) perhaps we can borrow the logic that glib uses for Windows?

---

_Comment by @BurntSushi on 2017-03-24 21:50_

@ssokolow  nice! Thanks for that.

---

_Comment by @ssokolow on 2017-03-24 21:52_

No problem. I'd actually have made a mention ages ago, but I was caught in that "Judging by ripgrep, he's clearly much smarter and more knowledgeable than me. He must already know this." trap.

---

_Comment by @BurntSushi on 2017-03-24 22:07_

@ssokolow Haha. Indeed, that is a trap! Glad to be reminded of it.

Now that I think of it, I also had a kinda-sorta feeling that clap might just up and solve this problem for us. More thought still needs to be given to how config files actually work though. e.g., If you specify `--no-ignore` by default, how do you override it? Do we need to add a new flag? Some flags are have that like `-i`/`-s`/`-S`, but not all do.

---

_Comment by @kbknapp on 2017-03-24 22:17_

I still intend to work on this issue in clap, but my bandwidth has been very limited lately with my day job. Unfortunately, I can't give a timeline for when I'll have it done. :-(

---

_Comment by @BurntSushi on 2017-03-24 22:27_

@kbknapp Oh heavens, please do no worry one little bit! I think I already owe you several &lt;drinks-of-your-choice&gt;. :-)

---

_Comment by @sgraham on 2017-03-28 20:02_

Oops, missed this issue when I filed mine. In my case, I'd be fine with a wrapper script on Linux/Mac, but on Windows, while making a .bat file "works", it's annoying in typical use because of the unfortunate "Terminate batch job (Y/N)?" prompt that happens if you Ctrl-C during a search.

So I'd enjoy an rc or an env var also. (Or just changing the default behaviour to --no-heading. ;)

---

_Comment by @BurntSushi on 2017-03-28 20:04_

Are `.bat` files the only way to create simple scripts in Windows? Surely there must be something more convenient?

---

_Comment by @sgraham on 2017-03-28 20:05_

Oh, you asked about aliases on Windows too -- it's possible via something like `doskey rg=rg-win.exe --myoptions $*`, but unfortunately there's no standard ".bashrc"-alike for cmd, so there's nowhere to put those and have them always available.

---

_Comment by @elirnm on 2017-03-29 04:16_

@BurntSushi In Powershell you can write a function in a Powershell script and have Powershell load the module on startup, but that's arguably more complicated than a simple batch script (not to mention it doesn't work in cmd.exe, although it does avoid the annoying "terminate batch jobs" prompt). .bat/.cmd files are essentially the equivalent of .sh scripts on Unix. Powershell scripts are nicer to use than .bat/.cmd scripts and are more integrated into the shell but require the use of Powershell over Command Prompt and are a bit more work to write if you want them to be nicely loaded as a module.

They're not really too inconvenient and work perfectly fine except for the "terminate batch jobs" prompt that gets more annoying the more you run a command, especially for search where you might terminate with ctrl+c a fair amount.

---

_Comment by @elirnm on 2017-03-29 04:35_

If you're fine with being restricted to Powershell, creating a wrapper function isn't actually too difficult. All you need to do is add a function to [your profile](https://msdn.microsoft.com/en-us/library/windows/desktop/bb613488%28v=vs.85%29.aspx), which will then automatically be loaded on starting Powershell.

So you could add a function like

```powershell
function rg2 () {
  rg --no-heading $args
}
```

and just call `rg2` from the command line to get --no-heading by default (although be sure to provide the full path to `rg.exe` if you want to actually name the function `rg`). Command Prompt is unfortunately not as nicely integrated with anything, as far as I'm aware.

Of course, running it from Powershell with everything using the default color schemes will make the file names unreadable https://github.com/BurntSushi/ripgrep/issues/342.

---

_Comment by @BurntSushi on 2017-03-29 10:58_

@sgraham @elirnm I think what I'm hearing is that there is reasonably solid motivation for adding a config file to ripgrep, so I *think* I'm sold on that. (Others have mentioned Windows being difficult to write wrapper scripts in, but I don't think I ever got the full scoop.)

---

_Comment by @BatmanAoD on 2017-03-30 18:42_

I think it makes sense to wait for clap to handle this.

For Windows, it sounds like part of the problem with the "just use an alias or similar customization" solution is that PowerShell is much easier to personalize than `cmd.exe`, but most Windows users (including myself) are reluctant to learn a new shell (or have never used a decent shell in the first place and don't understand the benefits) and therefore either stick to `cmd.exe` or use something like `git-bash` or Cygwin (which come with a host of complications and oddities*, but are better than raw `cmd.exe`). I know that personalizing `cmd.exe` with alias-like things is *possible*, but every time I think about trying it and look up the steps, it seems like it's more trouble than it's worth.

*As an example: recently, I learned the hard way that in `git-bash`, `ln -s` silently *copies* the target to the new "symlink" file, rather than making a true symlink. [Here is complete workaround in all its gory glory.](http://stackoverflow.com/a/25394801/1858225) Relatedly, once a real directory symlink is made in Windows, `del` (which typically behaves like `rm`) will *recursively delete the entire **linked** directory*. The only way to actually delete *just the symlink* is to use...`rmdir`. Really. Fun times!

---

_Comment by @teeberg on 2017-06-19 18:35_

Another argument against using an alias is that config files might allow you to have per-project settings, as `ack` lets you have by creating an `.ackrc` right in your project folder.

It should be doable to do that with a bash function though, so I may be able to help myself.

Reading the above thread, it sounds like this is not a use case that's been considered or planned so far, so please consider this a feature request while planning this. :-)

---

_Comment by @BurntSushi on 2017-06-19 20:18_

@teeberg Thanks! That is a good one, and certainly seems like a logical addition. But it is good to keep in mind!

---

_Comment by @wsdjeg on 2017-07-02 13:55_

@BurntSushi really thanks for this great tool, I am author of spacevim, and I am writting a background search tools for spacevim, now we support: `ag`,  `rg`,  `pt`, `ack` and `grep`. and I use neovim's job api to implement this feature. in `ag`,  `pt`,  `ack`  the line number is enable by default, but when use `rg`, I need to add an extra argv `-n` to jobstart().

and here is the PR https://github.com/SpaceVim/SpaceVim/pull/699
and here is the fix commit for rg support in spacevim searching tools  https://github.com/SpaceVim/SpaceVim/pull/699/commits/b30a3bb7d6af71b66e89a8a2ca9e085411444b77

what I hope is `-n` will be enabled by default.

sorry,  I think it is not a bug, so I am not sure if I should open issue for it.

---

_Comment by @BurntSushi on 2017-07-02 14:49_

@wsdjeg Thanks for the feedback, but I am pretty confused! Perhaps you can help me understand better what problem you're trying to solve?

In particular, what does enabling line numbers by default have to do with persistent configuration? Moreover, I'm not sure you should expect that every tool has the same set of default behaviors. Having to customize flags based on which tool is used seems entirely reasonable to me.

To clarify, line numbers are actually enabled by default *when printing results to a tty*. Try running ripgrep in your terminal and you'll see line numbers. But if you redirect the output to a pipe or a file, then the output reverts to the same output format you'd get if you were running `grep -r` or `git grep`. I don't think this will ever change, and certainly, having to add an `if` statement in one's vim config isn't nearly a strong enough use case to cause that change IMO.

---

_Comment by @wsdjeg on 2017-07-02 14:54_

@BurntSushi ok, Thanks, I understand now. nothing need to be change now. 😄 

---

_Comment by @sarahgerweck on 2017-07-13 01:17_

I would really like this too. I've tried to get along with aliases, but they are not perfect, in particular because of #553.

E.g., I really prefer smart case sensitivity. (I believe that it should be the default, but that's orthogonal and a matter of opinion.) It's a good example of the limitations here.

You can make an alias like this:

```
alias rg='rg -S'
```

This mostly works, but it means that I now get an error if I ever execute any commands that include `-S` in them. Ripgrep give the error below:
```
error: The argument '--smart-case' was provided more than once, but cannot be used multiple times
```

This throws a bit of a wrench into the notion of using aliases as a way to set defaults, because it means you can't pass around command lines with people or machines that don't have _exactly_ the same defaults configured, because it's not safe to be explicit anymore. E.g., I can't send a command to a coworker that contains `-S` if they happen to have `-S` configured as their default without it breaking `ripgrep`.

Were #553 fixed, I think the need for a defaults file would decrease dramatically—though I still think it's desirable.

---

_Comment by @BurntSushi on 2017-07-13 01:28_

@sarahgerweck I think that is indeed one of the key issues that needs to be solved. Getting a persistent config goes hand-in-hand with ironing out exactly the problem you're describing. A bit upthread (this is a long one!) I mentioned this:

> We may need to solve problems like "I set `--context 5` in my config file, but I want to override occasionally with `--context 0` on the command line." We can't just trivially merge the config with the CLI parameters since `rg --context 5 --context 0` foo is illegal today. This is probably true of several other flags, and many other flags probably need the opposites added. e.g., If `--no-ignore` is in the config file, how does a user re-enable ignore functionality?

I don't have the full context on what clap can do (I need to re-read @kbknapp's posts above). I think the key next step here is going through each of the flags and specifying its behavior in the presence of multiple flags. Some things that might be tricky (as in, we will be at the mercy of clap---our argv parser---I think) are when multiple flags toggle the same sort of behavior and you want the "last one" to win. Such is the case for `-S/-s/-i`, for example.

---

_Comment by @sarahgerweck on 2017-07-13 01:34_

@BurntSushi I skimmed the full thread but missed that part. You're right that it's effectively the same problem. As I mentioned in #553, I think you probably want this last-one-wins behavior for command-line options rather than a "specified twice" error, as it's more friendly for scripts or shell functions that want to build up a complicated command line that might contradict themselves.

I will say that, although I'm generally a proponent of reusing standard components instead of inventing one, argument parsing is not a particularly difficult thing to do. If clap can't do what you want, it would be very feasible for someone to extend or replace clap.

---

_Comment by @BurntSushi on 2017-07-13 01:38_

> argument parsing is not a particularly difficult thing to do

I've written my fair share of them, and while I can't speak for @kbknapp, I think this is underestimating the task. :-) I'm sure we will find a way to make this work with clap.

---

_Comment by @unphased on 2017-11-21 01:46_

Pardon me if I've missed a core part of this discussion, but I find it surprising that there can't be a simple feature which reads `~/.rgrc` or `~/.rgignore` containing a list of regexes whose paths should be excluded from search. 

While I completely understand that a (rather more unwieldy) collection of args provided to `rg` can achieve this, here are four reasons why I still want an exclude-file. 

It means that any time I edit this global exclude-list (and I anticipate doing it a lot) I have to:

1. edit an alias in a shell config script 
2. incur mental overhead about whether #553 is a risk
3. compose my file/path exclusion patterns inside shell-strings, rather than as plaintext patterns

Furthermore, (4) I want a clean and safe global config that I can tuck in a dotfiles repo because I may carefully construct a set of search exclusion paths for a given large project, and then soon after clone that project on another machine (or three) in subtly different directories on the filesystem. Manually editing in-tree `.ignore`s is wonderful but tends to scale poorly for scatterbrain distributed shotgun cowboy coding especially in repos of which I am not the owner.

Admittedly, I'm jumping straight past `ag` from `ack`, and I have to say that `ack`'s configuration method/syntax/implementation is pitifully bad (by comparison 😎). So this isn't so much a complaint as idealistic musing.

The main thing that strikes me here is that there has already been more effort spent by folks discussing this topic than the effort needed to implement the functionality! Surely? 

If I'm not far off the mark with my points here I'd gladly contribute to the project. It's a great excuse to finish learning Rust. 

---

_Comment by @BurntSushi on 2017-11-21 01:58_

@unphased You seem to be conflating two orthogonal issues: an ignore file and persistent configuration. It would help if you could please clearly delineate these issues. Also,  I'd like to kindly suggest that spending time talking about how easy something is when you don't know the actual implementation path isn't productive. The fact that this issue is so long should be a dead giveaway that there is more to this than you might expect.

First of all, you are entirely mistaken about the lack of a global exclude list. You can get it today using the --ignore-file flag. You'll need to put the flag in an alias, but the rules are specified in normal gitignore syntax.

Secondly, persistent config is something i would like to add, but haven't yet found the time to do the preliminary work required. I think that has already been covered. #553 is exactly the kind of thing that needs to be resolved for persistent config.

Please. Let's not rehash this. It's just delayed until I find time, pain and simple. Someone else could step up here, but it's a lot of tedious work and may not have clearly defined answers.

---

_Comment by @unphased on 2017-11-21 02:03_

Thank you for responding so quickly! It looks like i have indeed missed a few important details! Very sorry about that.

The only purpose I've ever needed persistence of config for are the only config that I use, which are ignore patterns! Using an alias to pass ignorefile (thus turning it into global semi-persistent config) will do great for now. 

---

_Comment by @BurntSushi on 2018-02-01 16:03_

Does anyone want to brainstorm the format for a ripgreprc config file?

Ack's format is [documented here](https://beyondgrep.com/documentation/ack-2.22-man.html#THE-.ackrc-FILE):

> The .ackrc file contains command-line options that are prepended to the command line before processing. Multiple options may live on multiple lines. Lines beginning with a # are ignored. A .ackrc might look like this:
>
>     # Always sort the files
>     --sort-files
>
>     # Always color, even if piping to another program
>     --color
>
>     # Use "less -r" as my pager
>     --pager=less -r
>
> Note that arguments with spaces in them do not need to be quoted, as they are not interpreted by the shell. Basically, each line in the .ackrc file is interpreted as one element of @ARGV.

This seems somewhat sensible in that we don't need to bother with shell escaping or other rules, so it is simple. The only real downside from my view is that it makes some intuitive constructions invalid. For example, a line containing the string `--context 5` is wrong because it will be interpreted as a single argument. Instead, you either need `--context=5`, or you could write it out over multiple lines:

    --context
    5

Do people find this acceptable? If we wanted to support `--context 5`, for example, then I think we need to start doing things like, "characters separated by any amount of whitespace are treated as distinct shell arguments." And if you say that, then you need rules for quoting in cases where CLI arguments have spaces in them.

Is there another simple construction I'm missing? Is adopting the ackrc format good enough?

---

_Comment by @BurntSushi on 2018-02-01 16:04_

I suppose the other downside with the ackrc format is that I believe it makes it impossible to write a literal `\n` into your arguments. This seems OK to me, and if we ever needed to support that use case, ripgrep could/should interpret an escaped `\n` as a newline character.

---

_Comment by @BatmanAoD on 2018-02-01 16:54_

I like the ackrc format, and it has the added advantage of keeping the ripgrep learning curve minimal for ack users.

For literal newlines, if it's ever interesting to put those in an argument, I think a more general solution would be to have a syntax for naming a file that contains the literal binary content of the next argument token. This would place the burden of figuring out which encoding to use on the user, though. 

---

_Comment by @BurntSushi on 2018-02-03 03:52_

Another possible solution here is to use a TOML file (or similar) and just define a "proper" configuration format. This would fix the aforementioned issues with the "rc" file format types, but it definitely introduces issues of its own. In particular, it's another layer between a specification of how ripgrep should behave and how it actually interprets that specification. With an rc format, we provide a very straight-forward translation based on something users (presumably) already understand very well: shell arguments. With a config format, we would need to be careful to support anything that shell arguments support, which is probably tricky.

---

_Comment by @okdana on 2018-02-03 06:29_

I like the idea of keeping the config file one-to-one with the CLI options, because it's easy for end users to understand, and it seems like the implementation would be simpler too: you pretty much just collect it all into an array and pass it to `clap`, right? `clap` already knows which options conflict with or override which others, what kind of arguments they all take, &c., so it's easy.

With TOML/whatever, you have to add your own layer of abstraction to deal with all of that. Or you can try to shove them into the CLI-option paradigm, in which case the main weird thing would be the handling of boolean-ish options (`line-numbers = false`? `no-line-numbers = true`? `smart-case = false`? `ignore-parent = false`?).

The `ackrc` format does have some limitations like you mentioned. The requirement to use the same-word `=` format is kind of bothersome, in terms of both aesthetics and usability. Not being able to use special characters isn't a *huge* deal, but i can see cases where you might want it. For example, macOS creates these stupid meta-data files that end with a carriage return, so maybe you'd want a way to specify the equivalent of `--glob $'!Icon\r'`.

Several other tools besides `ack` use config files that are based on command-line options; off the top of my head i can think of cURL, Dnsmasq, and Redis. All three of those offer various combinations of 'sugar' on top of the `ackrc` style, and if you picked the best ones you might end up with something like this:

```sh
# .rgrc: Specify one command-line option per line

# Allow omission of leading `--`
smart-case

# Support both same-word and next-word optarg styles
--colors=match:bg:red
colors match:fg:white

# Provide quoting mechanism, support C-style escape sequences in double-quotes
ignore-file 'c:\some\windows\path\idk'
glob "!Icon\r"
```

I am very possibly gold-plating it now though; ignore the last half of this comment if it's ridiculous

---

_Comment by @BurntSushi on 2018-02-03 15:03_

@okdana Thanks for brainstorming with me! I think we are probably in strong alignment in our opposition to something like a TOML config format. I forgot about the additional implementation complexity you pointed out. The rc format allowing us to push the merging logic of arguments down into clap is a really nice advantage.

w.r.t. to special characters, I suspect we probably want to fix ripgrep here maybe? Here's some interesting examples:

```
$ echo testing > 'foo        bar'

$ echo testing > foobar

$ l
total 8.0K
-rw-rw-r-- 1 andrew users 8 Feb  3 09:52 foobar
-rw-rw-r-- 1 andrew users 8 Feb  3 09:52 foo?bar

$ rg testing
foobar
1:testing

foo     bar
1:testing

$ rg testing -g 'foo bar'
foo     bar
1:testing

$ rg testing -g 'foobar'
foobar
1:testing

$ rg testing -g 'foo\tbar'
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

$ rg testing -g 'foo\tbar' --debug
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        't',
        'e',
        's',
        't',
        'i',
        'n',
        'g'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(testing)], limit_size: 250, limit_class: 10 }
DEBUG:ignore::walk: ignoring ./foobar: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./foo      bar: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

$ rg --files | rg 'foobar'
foobar

$ rg --files | rg 'foo       bar'
foo     bar

$ rg --files | rg 'foo\tbar'
foo     bar
```

Notice that for regex searches, `\t` is interpreted as a tab (this is an intention feature of the regex engine), but for glob searches, `\t` is not interpreted as a tab (although you can still type a literal tab character and it will work as expected). So it might be the case here that we actually want to fix the glob parser to interpret some escape sequences, but I suppose that needs some careful thought and inspection as to whether other glob engines do the same.

> \# Support both same-word and next-word optarg styles

This one is interesting. We could adopt a simple heuristic where if the line begins with a `-`, then the next sequence of non-whitespace characters is interpreted as a single shell argument followed by the next sequence of non-LF characters as a second shell argument, if such a sequence exists. I suppose we could get into trouble there with something like

```
--foo=bar quux
```

Which would translate to `--foo=bar` as one argument and `quux` as another. I suppose we could modify the above rule to include `=` since we can also say "flag names will never contain `=` characters." :-)

It seems like we could start with the ackrc format basically as-is, and then build conveniences as we go. We might also want to consider a strategy for evolving the format in a non-breaking way, but that is profligate with its own set of unique challenges and trade offs...

> For example, macOS creates these stupid meta-data files that end with a carriage return, so maybe you'd want a way to specify the equivalent of `--glob $'!Icon\r'`.

O_O

---

_Comment by @BurntSushi on 2018-02-03 20:09_

OK... So I've hit another snag. As far as I can tell, nobody has yet solved this problem of configuration files in Rust-land. What I'd be after is something that lets me read configuration files in a way that is idiomatic on Linux, macOS and Windows. There are various (many, actually) crates that purport to do something in this space, but I don't feel comfortable using _any_ of them for various reasons (mostly, they are unmaintained). I personally do not want to jump into those waters as I've observed how dicey they can be from afar.

Therefore, I suspect the initial implementation of this feature is going to land behind a flag. That is, `rg --config /wherever/you/want/.ripgreprc`. We'll definitely want a flag like that anyway. This is probably fine on Unix-like systems where creating an alias is easy, but I guess doing something similar on Windows is not as easy. Which is kind of a bummer since Windows is kind of what pushed me over the edge to accept this feature request.

We _could_ start with "project specific" config files. That is, ripgrep will look in the CWD and its parents for a `.ripgreprc` file and load it automatically. One possible thing that somewhat worries me here is that it will encourage folks to add a `$HOME/.ripgreprc` file, which will work in many cases, but it won't actually be a global config. So when you cd into `/etc` (or whatever) you might be surprised to discover that your config isn't being applied.

Thoughts?

---

_Comment by @BurntSushi on 2018-02-03 20:22_

Another thought I had was permitting arguments to be specified in an environment variable. Both ack and grep support it via `ACK_OPTIONS` and `GREP_OPTIONS`, although I've heard Ack's author say that they would like to remove support for it, and GNU grep's man page says this:

>        GREP_OPTIONS
>              This  variable  specifies default options to be placed in front of any explicit options.  As this causes problems when writing portable scripts, this feature will be removed in a future release
>              of grep, and grep warns if it is used.  Please use an alias or script instead.

But the particular downside mentioned by grep here also strikes me as a downside of having auto-loaded config files too.

There is also the question of precedence for a hypothetical RIPGREP_OPTIONS environment variable. Is it loaded before all configs? After all configs? Before configs specified on the CLI but after any auto-loaded configs?

Questions abound...

---

_Comment by @kbknapp on 2018-02-03 20:37_

These were all the questions that started to put kbknapp/clap-rs#748 as well 😟 

The conclusion I've come to (and just not implemented it yet) is allowing "additional argv"s to be declared and parsed prior to the CLI (because of overrides, the CLI [IMO] should *always* be last and therefore able to override everything else). 

I don't really want clap to have to worry about loading config files in tons of different formats, file I/O, transforming a config into an argv, etc...so I'd prefer all that happen in an external crate or in application code. 

Once all that has happened, and we have something that looks like an argv, I'd like to make it as simple as saying: `app.argv("some --args=to -be --parsed -b=efore the --cli").get_matches()`. Of course, an ENV var may be the only exception to this, as clap already read ENV vars for argument values, so adding something like `app.env_argv("RIPGREP_OPTIONS").argv("options --from-some -config=file").get_matches()` wouldn't be out of the question.

My personal thoughts are the evaluation order should be:

* ENV vars
* Global config files
* Project config files
* CLI

Where each step can override any values from the step above it.

Edit: Note the "config file" ones are the only ones were clap couldn't control the order itself, and it would be up to application code to specify the order (i.e. if they'd prefer global configs be able to override project configs).

---

_Comment by @kbknapp on 2018-02-03 20:40_

Oh, that issue also brings up some more questions about, imagine an argument which accepts multiple values. If one value is defined in the ENV, one in a config file, and one at the CLI what should the matches look like? All three values? Only the CLI?

---

_Comment by @okdana on 2018-02-03 20:51_

>So it might be the case here that we actually want to fix the glob parser to interpret some escape sequences, but I suppose that needs some careful thought and inspection as to whether other glob engines do the same.

As far as i'm aware, there aren't (m)any that do. `fnmatch(3)` doesn't have any special handling for it at least: `\t` is treated the same as `t`. git doesn't seem to do anything special with it either. Anyway, related to #526 probably.

>It seems like we could start with the ackrc format basically as-is, and then build conveniences as we go. We might also want to consider a strategy for evolving the format in a non-breaking way, but that is profligate with its own set of unique challenges and trade offs...

Yeah, the only concern with starting out simple is how much it could eventually break. e.g., if you add quoting or escape sequences or whatever, who knows what'll happen to people's existing files. But it is still 0.x software after all, so it's not that big a deal i suppose.

>We could start with "project specific" config files.

I'm not familiar with any other tools that do this but this would bother me and i would immediately be looking for a way to turn it off. Project-specific ignore rules, OK, i get that, but i don't want random people screwing with my actual `rg` options.

>Thoughts?

I don't think i understand the issue exactly. It sounds like you're more worried about *finding* the config file than parsing/loading it? Is there a reason you wouldn't just check for `$HOME/.ripgreprc` and `$XDG_CONFIG_HOME/ripgrep/ripgreprc` (or whatever the Linux standard is)?

>But the particular downside mentioned by grep here also strikes me as a downside of having auto-loaded config files too.

AFAIK the main reason GNU are removing `GREP_OPTIONS` is that `grep` is used in loads of shell scripts (some of them quite critical), and people would constantly forget to unset `GREP_OPTIONS` at the top of those scripts, and it'd cause all kinds of bizarre (maybe even security-impacting) behaviour for them.

Since `rg` is probably not going to be used in any super important shell scripts, i don't think it's a big concern for this project. The only issue with supporting an environment variable is that you (or `clap`) have to have a mechanism for parsing options from a  'flat' string.

>There is also the question of precedence for a hypothetical RIPGREP_OPTIONS environment variable.

I would expect an environment variable to override any config-file options, based on the fact that the former is ephemeral and easier to modify than the latter. That's how git works too: config-file options -> environment-variable options -> command-line options

---

_Comment by @BurntSushi on 2018-02-03 20:59_

@kbknapp TL;DR I think any specific issues with `clap` for config in ripgrep are actually resolved on my end. See the latest few commits in my [`ag/persistent-config`](https://github.com/BurntSushi/ripgrep/tree/ag/persistent-config) branch. :-)

> Once all that has happened, and we have something that looks like an argv, I'd like to make it as simple as saying: `app.argv("some --args=to -be --parsed -b=efore the --cli").get_matches()`.

I think I can just do this already? `clap::App` has a `get_matches_from`, and I can just build the argv myself by concatenating all of the flags from the config files, and making sure `env::args` comes last. Or maybe you're just talking about convenience routines here?

[EDIT] - Ah I see, you're talking about one big string that clap then parses. Yeah that'd be interesting.

> Oh, that issue also brings up some more questions about, imagine an argument which accepts multiple values. If one value is defined in the ENV, one in a config file, and one at the CLI what should the matches look like? All three values? Only the CLI?

I've got this worked out now. :-) See https://github.com/BurntSushi/ripgrep/commit/90387bbd4c3f09b9f55e35bcd231220296ce5ee7 and in particular the `RGArgKind` type which has some docs on it.

I should revise what I just said. I have it worked out *for ripgrep*. I suspect solving this in a general case is far harder and there are probably lots of variants. I don't envy your task! To be honest though, I'm actually pretty happy with my work-around thus far. It caused me to clean up my arg definitions, and the changes to the code that interfaces with `clap::ArgMatches` were actually very small. Basically, it was just a matter of changing the `value_of` methods to return the *last* value instead of the first value.

@okdana 

> I'm not familiar with any other tools that do this but this would bother me and i would immediately be looking for a way to turn it off. Project-specific ignore rules, OK, i get that, but i don't want random people screwing with my actual rg options.

Interesting! ack has this feature AFAIK. Best to start without it then. :-)

> I don't think i understand the issue exactly. It sounds like you're more worried about finding the config file than parsing/loading it? Is there a reason you wouldn't just check for $HOME/.ripgreprc and $XDG_CONFIG_HOME/ripgrep/ripgreprc (or whatever the Linux standard is)?

Ah yeah I should have said more words. The `XDG_CONFIG_HOME` style works fine on Linux (and maybe Mac?), but from what I understand it isn't a good default on Windows. So while I'd probably be OK writing out the XDG logic for Linux (I've done it before, it's not that hard), I'm far less in tune with how other platforms do this, so I'm very hesitant to open that can of worms.

> The only issue with supporting an environment variable is that you (or clap) have to have a mechanism for parsing options from a 'flat' string.

Yup, good point. Drats. That kind of deflates that idea then!

> I would expect an environment variable to override any config-file options, based on the fact that the former is ephemeral and easier to modify than the latter. That's how git works too: config-file options -> environment-variable options -> command-line options

Yeah I think that is consistent with Ack but actually inconsistent with @kbknapp's thoughts above! Regardless, it seems wise to punt on the auto-loading aspect of this ticket for now anyway. If project specific configs are contentious (and you make a good point), then the other variant of autoloading is to look in platform specific directories where application configs are normally stored, and as far as I can tell, the Rust ecosystem doesn't offer anything in that space that I'd be comfortable using just yet.

---

_Comment by @BurntSushi on 2018-02-03 21:12_

@nagisa on IRC suggested an env var that could be used to specify the location of a config file. It's slightly roundabout, but I think would be a nice sweet spot. It would sidestep the Windows issue of needing to write an alias/wrapper script, and also sidestep the issue of needing to parse an argv out of an env var itself.

---

_Comment by @BurntSushi on 2018-02-03 21:14_

(Hah, if we support a `--config-file` flag, then it will require ripgrep to recursively load them... *sigh*)

Maybe only the env var for now? ^_^

---

_Comment by @kbknapp on 2018-02-03 21:57_

> Yeah I think that is consistent with Ack but actually inconsistent with @kbknapp's thoughts above! 

To be clear, I'm not stuck in a position either way, those were just my initial thoughts. The only thing I have strong fieelings about is CLI should override all, which it sounds like is a universa thought :wink:


---

_Comment by @BurntSushi on 2018-02-03 22:50_

> which it sounds like is a universa thought

Yes indeed!

---

_Comment by @BatmanAoD on 2018-02-04 00:54_

Some thoughts/comments:

 * I would be interested in supporting and/or maintaining a library for generically finding config files and sequencing (but not parsing) them appropriately. But I don't have a lot of time. @BurntSushi, were any of the libraries you looked at that aren't maintained look like promising starts?
 * Windows *does* have a concept of a `$HOME` directory, `%USERPROFILE%`. For XDG configuration, I believe the standard (e.g. used by nvim) is `%APPDATA%`. I think it would be sufficient to look in those two directories for config files.
 * An environment variable containing a single config file path (or more, if they could be split appropriately) would be a perfectly acceptable solution. I am highly in favor of this.
    * This could actually be quite useful for scripting, because the script could check some aspect of the environment and select a config file as appropriate. See for instance [this customization I use on Windows but not elsewhere](https://github.com/BatmanAoD/public-config/blob/6819910ab67f74e9aa2ecc8e0207a6b350e4f4ec/bash_aliases#L238).
 * I don't think a `--config-file` flag would be necessary, but something like Vim's `-u NONE` would, I think, be *very* important to support scripting. This option would prevent ripgrep from using *any* custom configuration, even if a config file is specified in an environment variable. I suggest `--no-config` or something similar.

---

_Comment by @BurntSushi on 2018-02-04 01:17_

@BatmanAoD Sounds great! When I did my survey on available crates, I basically dismissed almost all of them a priori without even looking at the code based on one (or more) of the following reasons:

1. It was effectively unmaintained. (By this, I mean there was very little recent commit activity **and** an issue/PR backlog with seemingly important things that need to be addressed.)
2. The documentation was either missing or insufficient.
3. It wasn't under a permissive license.
4. Some combination of the above coupled with the fact that nobody was using it. If nobody is using a library, then that means I need to do a thorough audit before bringing it in as a dependency.
5. The crate addressed XDG but nothing else.

So on those grounds they basically all failed the sniff test before I was willing to spend more time with them. So in that sense, I don't know if any were a good start. You'll need to do your own review. :-)

If you do take this on, beware the can of worms. Here are some relevant links:

* https://github.com/rust-lang/cargo/pull/2127 (yikes)
* https://github.com/rust-lang-nursery/rustup.rs/issues/247
* https://github.com/rust-lang/cargo/issues/1734
* https://github.com/rust-lang/cargo/pull/148

---

_Comment by @BurntSushi on 2018-02-04 01:19_

> I suggest --no-config or something similar.

Definitely. This will be in the initial support for persistent config.

---

_Comment by @wsdjeg on 2018-02-04 01:25_

If you want to add support for `.rgrc`, please consider the configuration file in HOME directory or the root of a project. 

I mean if I have a `.rgrc` which path is `~/src/Foo/.rgrc`, and then if I run rg command from the subdirectories， such as `~/src/Foo/src/utils/`.

---

_Comment by @BurntSushi on 2018-02-04 01:27_

@wsdjeg Please see comments above. I think we're going to punt on everything for now except for setting the config file path via an env var.

---

_Comment by @wsdjeg on 2018-02-04 01:29_

oh, sorry, use `evn var` seems better. :+1: 

---

_Comment by @BurntSushi on 2018-02-04 05:22_

PR submitted: https://github.com/BurntSushi/ripgrep/pull/772

---

_Closed by @BurntSushi on 2018-02-04 15:40_

---

_Comment by @jason-s on 2018-02-04 20:25_

I was going to ask what the eventual behavior was decided, but you wrote it up quite well in the commit message (a very helpful example of a good commit msg btw):

```
This commit adds support for reading configuration files that change
ripgrep's default behavior. The format of the configuration file is an
"rc" style and is very simple. It is defined by two rules:

  1. Every line is a shell argument, after trimming ASCII whitespace.
  2. Lines starting with '#' (optionally preceded by any amount of
     ASCII whitespace) are ignored.

ripgrep will look for a single configuration file if and only if the
RIPGREP_CONFIG_PATH environment variable is set and is non-empty.
ripgrep will parse shell arguments from this file on startup and will
behave as if the arguments in this file were prepended to any explicit
arguments given to ripgrep on the command line.

For example, if your ripgreprc file contained a single line:

    --smart-case

then the following command

    RIPGREP_CONFIG_PATH=wherever/.ripgreprc rg foo

would behave identically to the following command

    rg --smart-case foo

This commit also adds a new flag, --no-config, that when present will
suppress any and all support for configuration. This includes any future
support for auto-loading configuration files from pre-determined paths
(which this commit does not add).

Conflicts between configuration files and explicit arguments are handled
exactly like conflicts in the same command line invocation. That is,
this command:

    RIPGREP_CONFIG_PATH=wherever/.ripgreprc rg foo --case-sensitive

is exactly equivalent to

    rg --smart-case foo --case-sensitive

in which case, the --case-sensitive flag would override the --smart-case
flag.

Closes #196

---

_Comment by @elentok on 2018-02-06 09:13_

Awesome! thanks!

When are you planning to create a new release?

---

_Comment by @BurntSushi on 2018-02-06 10:44_

@elentok Soon. Releases happen when they happen. I do this in my spare time, so I will never set an explicit schedule.

---

_Comment by @kbknapp on 2018-02-06 16:03_

I was re-reading through old messages in this thread and noticed the `# allow omission of leading --`. Forgive me if this has already been addressed, but I wanted throw one last thought out there about this. I actually like that thought, but there is one edge case that is *possible*  and could lead to strange errors.

Assuming these are equivalent:

```
-H

# or

H
```

Since clap allows combining short switches (i.e. `-Ha` is the same as `-H -a`), and even combining with a trailing option/flag (i.e. `-Hatyaml` is the same as `-H -a -tyaml` is the same as `-H -a -t=yaml` etc.). One would need to know if what was found in the config file sans `--` or `-` was meant to be a long style flag `--word` or combination of short switches.

Since ripgrep has quite a few short switches, I'd says it's possible (not sure how likely) a combination of short switches would also equal a long flag sans `--`.

The fix for this would be to document, IF the user intends to combine switches in a ripgrep config, they should add a leading `-` otherwise it'd be treated as a long flag.

Just some thoughts :smile: 

---

_Comment by @BurntSushi on 2018-02-06 16:10_

@kbknapp Interesting. Good thing I did not implement that shortcut. Sounds like a good reason to just not do it!

---

_Comment by @okdana on 2018-02-06 16:33_

I think i had in mind (a) only one option per line and/or (b) not even supporting the short options, since (i believe) that's how the other tools i mentioned work. But yeah you couldn't omit the `--` having mixed them

---

_Comment by @danemacmillan on 2018-02-15 18:23_

I know I'm late to the discussion, especially since you've (@BurntSushi) already submitted a PR, but I do like how `rclone` handles persistent configs as environment variables: https://rclone.org/docs/#environment-variables.

---

_Comment by @BurntSushi on 2018-02-15 18:29_

@danemacmillan I'm happy with the approach I took.

---

_Comment by @BurntSushi on 2018-02-15 18:30_

@danemacmillan 

> already submitted a PR

Not only that, but the PR has been merged and is in the 0.8.0 release!

---
