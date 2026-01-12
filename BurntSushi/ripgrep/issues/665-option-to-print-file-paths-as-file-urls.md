```yaml
number: 665
title: Option to print file paths as file URLs
type: issue
state: closed
author: sanmai-NL
labels:
  - enhancement
  - question
assignees: []
created_at: 2017-11-06T21:18:34Z
updated_at: 2023-09-25T18:56:21Z
url: https://github.com/BurntSushi/ripgrep/issues/665
synced_at: 2026-01-12T16:13:22Z
```

# Option to print file paths as file URLs

---

_@sanmai-NL_

Printing file paths as `file` URLs is very handy since you can click on them and the file will open. This happens in Konsole at least.

---

_Comment by @BurntSushi on 2017-11-06 21:25_

Do other command line tools have an option like this? I don't think I've seen anything like it.

---

_Label `question` added by @BurntSushi on 2017-11-06 21:25_

---

_Comment by @sanmai-NL on 2017-11-07 16:05_

@BurntSushi: I don't know off the top of my head. You also encounter such such links while paging log output, for example.

To be clear, this isn't a question but a feature request. 🙂 

**Similar feature requests**
* robotframework/robotframework#2435
* ...

**Added value**
* Quicker access to files due to clickable paths in KDE Konsole, macOS Terminal, ...
* [URLs can be read by `wget` and `curl`](http://blog.gypsydave5.com/2016/02/04/xargs-and-curl/)

---

_Comment by @kpp on 2017-11-23 13:52_

```
echo -e "/path1\n/path2" | xargs -n1 -I{} echo file://{}
file:///path1
file:///path2
```

You may do the same for `rg` without modifying it.

---

_Comment by @BurntSushi on 2018-01-31 02:38_

I think I'm going to close this. I don't see a lot of other tools doing this, and it should be possible to write a wrapper script to do this for you if you really want it.

---

_Closed by @BurntSushi on 2018-01-31 02:38_

---

_Comment by @sanmai-NL on 2018-09-15 17:35_

@BurntSushi:
Important interesting precedent: https://github.com/systemd/systemd/blob/903511951892008b991991962fef2e8ecc65bc6c/NEWS#L220-L232.

---

_Comment by @adrianocr on 2019-07-22 21:22_

I think this should be reopened and properly considered. Its insanely useful. Certain terminal apps such as iterm2 does it by default. But it could work across ALL of them if rg outputted links with the `file://` syntax. 

---

_Comment by @alex on 2020-06-28 22:16_

@BurntSushi I am not quite sure how this is implemented, but I just learned that `ls` has a feature `ls --hyperlink=auto` (or `always` or `never`), which causes it to emit things with ANSI shell magic that makes them clickable links in my terminal. Here's an example:

```
alexgaynor@penguin ~/p/step-expirements> ls --hyperlink=always example.py
example.py
alexgaynor@penguin ~/p/step-expirements> ls --hyperlink=always example.py | xxd
00000000: 1b5d 383b 3b66 696c 653a 2f2f 7065 6e67  .]8;;file://peng
00000010: 7569 6e2f 686f 6d65 2f61 6c65 7867 6179  uin/home/alexgay
00000020: 6e6f 722f 7072 6f6a 6563 7473 2f73 7465  nor/projects/ste
00000030: 702d 6578 7069 7265 6d65 6e74 732f 6578  p-expirements/ex
00000040: 616d 706c 652e 7079 0765 7861 6d70 6c65  ample.py.example
00000050: 2e70 791b 5d38 3b3b 070a                 .py.]8;;..
```

If `ls` can have nice things, surely a program as awesome as `rg` can 😂 

---

_Comment by @saleemrashid on 2020-06-28 22:41_

This is fairly comprehensive documentation on the hyperlink feature that @alex mentioned: https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda

---

_Comment by @mx-psi on 2020-08-05 15:19_

Any chance this will be reopened? As mentioned above, there is widespread support for hyperlinks on modern terminal emulators and tools like `ls` have built-in support for this.

---

_Comment by @kovidgoyal on 2020-10-06 08:28_

FYI: Here is a wrapper implementation that actually allows you to click on results and open them in your editor at the matched line. https://sw.kovidgoyal.net/kitty/kittens/hyperlinked_grep.html

Note that it works with the kitty terminal.

---

_Comment by @adrian-gierakowski on 2021-02-06 22:15_

@BurntSushi would you be so kind and reconsider? it would be amazing to have it supported out of the box and not have to use wrappers 

---

_Comment by @bluss on 2021-05-13 15:42_

The following things seem to be needed for supporting hyperlinked file paths in ripgrep. I'm just writing for no reason, maybe it's useful.

- Resolve paths to absolute paths (I would suggest do so without touching the filesystem, don't want canonicalization) and url escape paths to be printable-ascii-safe as `file://` URIs.
- Lookup current hostname (bonus feature but should preferably be in the URL according to the spec, this seems to be a bit of a layer-busting complication)
- Add cli parameter --hyperlink={always|ansi|auto|never}  (I don't know if ansi is useful here, but I think so)
- Use the hyperlink escapes in the printer crate. I'm not sure if it's a great fit to try to put it inside termcolor - even if the spec says it works very similarly to colors.

This feature is supported with the same escape code in Linux (various terminals including gnome-terminal), Windows (microsoft/terminal), macos (iTerm2). The pager less supports it in new versions.

---

_Comment by @BurntSushi on 2021-05-13 18:08_

@bluss Thanks for laying that out! Some quick feedback:

* If possible, I'd like to avoid the hostname lookup aspect of this. You're spot on about it being layer busting. I think the key thing here is, what does the hostname buy us? i.e., What do we lose if we don't bother with it.
* Putting this inside of termcolor might be nice to be honest, especially if it's an ANSI escape sequence. Or at least, putting the _emission_ of that ANSI escape inside termcolor would be good. Having it be outside of termcolor would probably be more problematic.
* When supporting the legacy Windows console, I think we would just not emit the escape sequence, like we do now for things not supported by the Windows console.

I think that's all I can think of for right now. I'll re-open.

---

_Reopened by @BurntSushi on 2021-05-13 18:08_

---

_Label `enhancement` added by @BurntSushi on 2021-05-13 18:08_

---

_Comment by @NightMachinery on 2021-05-13 19:17_

@burntsushi The `hostname` can enable usage on SSH. With the hostnames present, one can configure their editor to open the remote files (both Vim and Emacs support this very well, and VSCode kind of supports it as well). If there is a performance concern, just cache the hostname every, say, week.

---

_Comment by @BurntSushi on 2021-05-13 19:21_

The concern isn't performance. Folks asking for file URIs can afford to pay for one hostname query per ripgrep execution. The issue (in my head) is abstraction. It likely means refactoring. I don't know how severe it is.

---

_Comment by @bluss on 2021-05-13 19:36_

Thanks! The main feature of the hostname is just for the terminal to know if it's a local path or not, as I understand it, so that it can avoid trying to open a remote path locally. (I haven't seen other features that are worth the bother, but they might come, too.)  Just for fun, here's a snippet for making hyperlinks in the shell `printf '\e]8;;%s\e\\%s\e]8;;\e\\\n' "$LINK" "$TITLE"`

---

_Comment by @kovidgoyal on 2021-05-14 01:13_

If you leave out hostname, you make it impossible for things like this
to function: https://sw.kovidgoyal.net/kitty/kittens/remote_file.html

That allows the user to click on hyperlinks pointing to a file on a
remote machine via SSH and have the file edited locally and
automatically re-uploaded, seamlessly by just clicking on it.

Please do not leave it out.


---

_Comment by @XhstormR on 2021-08-16 11:07_

This feature is really helpful.

---

_Comment by @ghost on 2022-08-08 21:54_

Hello. This would be a good addition to rg. About the hostname part of this, I've never seen a file:/// url with the hostname part specified (I've been using Linux since 2007); yes the spec talks about it, but in the real world it doesn't seem to be used for file:/// urls.

`ls` has this feature for local files only, right?

I think adding this for local files only would cover 90% of the use-cases.

My 2p's.

---

_Comment by @Jackenmen on 2022-08-08 22:13_

> `ls` has this feature for local files only, right?

No, it properly includes the hostname. As said above, it actually allows for existence of useful features such as clicking on the hyperlink in SSH session and opening or saving it with the local editor:
![image](https://user-images.githubusercontent.com/6032823/183522483-e3692dc3-1bf6-4f5d-afaa-c63c9d385ae0.png)
![image](https://user-images.githubusercontent.com/6032823/183522548-777e7136-b527-477a-a7cf-843d4c9fbd7a.png)

> About the hostname part of this, I've never seen a file:/// url with the hostname part specified (I've using Linux since 2007); yes the spec talks about it, but in the real world it doesn't seem to be used for file:/// urls.

**Escape sequences** for hyperlinks did not exist in 2007. You might have not seen `file://` with the hostname part specified in the typical contexts where you've seen `file://` (browsers, desktop environments, etc.) but you need to look at this within the context of terminal emulators and within that context, specifying hostname is well-established by this extension of the VT protocol:
https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda#file-uris-and-the-hostname

---

_Comment by @ghost on 2022-08-08 22:41_

That would require support in the terminal emulators, to fetch the file using scp, edit it, then reupload it... I don't know what terminal emulators, other than kitty, will actually support that.

My main point is, this can be implemented for local files only now (looking at the above comments the hostname querying seems to be a big obstacle for implementing this), and extended later.

Also note that, even gnome-terminal itself, will just tell you it can't open such kind of url:
```
Could not open the address "file://foo/bar/baz"
"file" scheme with remote hostname not supported
```

---

_Comment by @Jackenmen on 2022-08-08 22:56_

> My main point is, this can be implemented for local files only now

If you want to follow the specification that I linked (and the spec that all the terminals seem to be using), I'm not sure you can as it seems to be quite clear on this:

> Web browsers, desktop environments etc. tend to ignore the hostname component of a `file://hostname/path/to/file.txt` URI. In terminal emulators, such ignorance would lead to faulty targets if you `ssh` to a remote computer. **As such, we don't allow this sloppiness. Utilities that print hyperlinks are requested to fill out the `hostname`**, and terminal emulators are requested to match it against the local hostname and refuse to open the file if the hostname doesn't match (or offer other possibilities, e.g. to download with `scp` as iTerm2 does).



---

_Comment by @ltrzesniewski on 2022-10-01 18:51_

Note that currently, Windows Terminal does not support the hostname:

https://github.com/microsoft/terminal/blob/54dc2c4432857919a6a87682a09bca06608155ed/src/cascadia/TerminalApp/TerminalPage.cpp#L2304-L2309

Which basically means the hostname should be omitted, at least on Windows. Note that the spec *requests* for utilities to add the hostname, it does not *require* it.

(I'd like to work on this feature BTW)

---

_Comment by @kovidgoyal on 2022-10-02 02:06_

On Sat, Oct 01, 2022 at 11:52:09AM -0700, Lucas Trzesniewski wrote:
> Note that currently, Windows Terminal does not support the hostname:
> 
> https://github.com/microsoft/terminal/blob/54dc2c4432857919a6a87682a09bca06608155ed/src/cascadia/TerminalApp/TerminalPage.cpp#L2304-L2309
> 
> Which basically means the hostname should be omitted, at least on Windows. Note that the spec *requests* for utilities to add the hostname, it does not *require* it.

One terminal not supporting it on Windows doesnt mean it should be
omitted on windows.

And as has been noted above multiple times, not adding hostname will
*break* functionality in advanced terminal emulators. Both iTerm2 and
kitty use hostname to allow hyperlinks to work over ssh.

If ls can output hostnames in its hyperlinks with no ill effects so can
rg. 



---

_Comment by @ltrzesniewski on 2022-10-02 10:10_

> One terminal not supporting it on Windows doesnt mean it should be omitted on windows.

Well, the problem on Windows is not really Windows Terminal itself (though that's the main terminal app). The problem is that the *shell* doesn't support the hostname in file hyperlinks. Any terminal app trying to open a link using the shell will have the same issue. You cannot make hyperlinks broken by default on Windows.

I just tried opening such an URI with the shell, and I got the following error: *"The system cannot find the file specified."*. Windows Terminal prevents it and displays a more user-friendly error instead.

Here are the options I can see:
 - Omit the hostname everywhere (breaks SSH)
 - Include the hostname everywhere (breaks Windows Terminal and other apps which use the shell)
 - Omit the hostname on Windows, include it everywhere else (good compromise IMO, SSH would work with kitty)
 - Omit the hostname if the `WT_SESSION` environment variable is set (special-cases Windows Terminal)
 - Add a command line argument for this (maybe overkill)


---

_Comment by @ssbarnea on 2022-10-02 10:26_

There is no need for hostname for local file URI, they only need `file://` prefix, followed by the FULL-PATH (whatever is that on windows, especially as running under WSL would mean a unix path). URI cannot have relative paths, not supported by default. Feel free to consult the specification or details at https://datatracker.ietf.org/doc/html/rfc8089#appendix-D.2


---

_Comment by @ltrzesniewski on 2022-10-02 10:32_

> There is no need for hostname for local file URI

The concern here is running ripgrep through SSH. That would output "local" files paths from the point of view of the remote machine. The terminal would need the hostname to detect those aren't really local.

> whatever is that on windows, especially as running under WSL would mean a unix path

Those work:
- `file:///C:/SomeDir/SomeFile.txt`
- `file://localhost/C:/SomeDir/SomeFile.txt`
- `file://wsl$/Ubuntu/SomeDir/SomeFile.txt` 

Those don't:
- `file://local_hostname/C:/SomeDir/SomeFile.txt`

I just realized WSL links work fine with the shell, but Windows Terminal blocks them. I'll open an issue for that.

Ew, actually, remote hostnames work fine, but the local hostname is rejected unless it's literally `localhost`. 😭 

---

_Comment by @kovidgoyal on 2022-10-02 11:42_

On Sun, Oct 02, 2022 at 03:10:22AM -0700, Lucas Trzesniewski wrote:
> > One terminal not supporting it on Windows doesnt mean it should be omitted on windows.
> 
> Well, the problem on Windows is not really Windows Terminal itself (though that's the main terminal app). The problem is that the *shell* doesn't support the hostname in file hyperlinks. Any terminal app trying to open a link using the shell will have the same issue. You cannot make hyperlinks broken by default on Windows.

I dont follow, any halfway decent terminal running on windows should
just parse the URI, if it detects that it has a hostname, match it
against the hostname of the local machine, if it is local it can remove
the hostname from the URI before passing it on to the shell. Or just
extract the local file path and pass it to the shell.

There is absolutely zero need for individual terminal applications to
try to specialize their output based on intended destination OS, which
is anyway not possible to determine, given that they could be running
over SSH.



---

_Comment by @ltrzesniewski on 2022-10-02 11:51_

@kovidgoyal it seems you're reading this discussion through your mails, so you probably didn't see it, but I have made lots of edits to my posts, and opened an issue with Windows Terminal which requests them to do exactly what you suggest. 🙂

But *today*, it's unfortunately broken, and we need to deal with it. If you run ripgrep over SSH, the remote process won't be on Windows anyway so it'll output the hostname just fine.

Note that ripgrep already specializes on the OS for much more trivial things:

https://github.com/BurntSushi/ripgrep/blob/4386b8e805e273a9795ad4e256c455c74407c949/crates/printer/src/color.rs#L16-L19


---

_Comment by @ltrzesniewski on 2022-10-19 18:17_

I started implementing this in #2322, and have a preview version available if you're interested.

If you'd like to try it out, I made a [GitHub workflow](https://github.com/ltrzesniewski/ripgrep/actions/workflows/preview.yml?query=is%3Asuccess) which builds ripgrep with this feature. Just open the latest build and download the artifact for your platform (at the bottom of the build summary - unfortunately, many warnings are produced by the rustup action).


---

_Comment by @guettli on 2023-01-06 09:59_

if i use `rg myterm | cat` I get output like this:

```
internal/annotation/load_balancer_test.go:                              annotation.LBSvcProtocol:            hcloud.LoadBalancerServiceProtocolTCP,
internal/annotation/load_balancer_test.go:                              annotation.LBSvcHealthCheckProtocol: hcloud.LoadBalancerServiceProtocolTCP,
```

In vscode I can click on `internal/annotation/load_balancer_test.go` and jump to the source code.

I don't think file URLs like `file://...` are needed.


But the line-numbers are missing. It would be great to include them somehow.

Update: I use this in my .bashrc now, and it works fine:

```
if [[ "$TERM_PROGRAM" == 'vscode' ]]; then
  alias 'rg'='rg --no-heading --column'
fi
```

---

_Comment by @ltrzesniewski on 2023-01-06 10:42_

Oh, that's a nice feature of VSCode 🙂 

If you use `rg -n myterm | cat` the output will include line numbers in the `file:line` format, and VSCode is able to interpret that and jump to the correct line number.

Unfortunately, that only works in the VSCode terminal (or in terminals which treat `file:line` command output as a link)... Also, note that VSCode underlines `file://` links so they stand out.


---

_Comment by @guettli on 2023-01-06 12:07_

Thank you for the hint. `rg -n myterm | cat ` works fine. I can jump the the correct line via vscode.

The pattern `file:linenumber ....` for output of compilers/lints is very common. This worked even 20 years ago (when I used emacs).

Adding `file://` would make the output more ugly for the human eyes.

Is there a way to configure rg to always use the output of `rg -n ... | cat` ?


---

_Comment by @BurntSushi on 2023-01-06 12:14_

Use the `--no-heading` flag.

---

_Comment by @ltrzesniewski on 2023-01-06 13:38_

> Adding `file://` would make the output more ugly for the human eyes.

It renders like that in VSCode:

![image](https://user-images.githubusercontent.com/7913492/211023176-53bbfa0a-cab0-443e-b653-83422fe757bd.png)


---

_Comment by @guettli on 2023-01-06 13:42_

> > Adding `file://` would make the output more ugly for the human eyes.
> 
> It renders like that in VSCode:
> 
> ![image](https://user-images.githubusercontent.com/7913492/211023176-53bbfa0a-cab0-443e-b653-83422fe757bd.png)

This looks great. 

The line-numbers are hyperlinks to the corresponding lines? 

How to enable this output?

---

_Comment by @ltrzesniewski on 2023-01-06 14:13_

No, unfortunately the line numbers aren't hyperlinks. Only the heading is a hyperlink, that's why it's underlined.

There would need to be a *standardized* way in the `file://` protocol to target a specific line in a file, and the terminal would need to understand these kinds of links, but there's no such standard AFAIK. Also, having each line number underlined wouldn't look great I suppose.

The screenshot was taken using a build of ripgrep which includes my #2322 PR. If you'd like to try it, you can [download it here](https://github.com/ltrzesniewski/ripgrep/actions/runs/3283869673) (scroll to the bottom of the page where the artifacts are located).

---

_Comment by @diktomat on 2023-01-06 19:42_

@guettli:
> The pattern `file:linenumber ....` for output of compilers/lints is very common. This worked even 20 years ago (when I used emacs).

Because editors can implement this, using the knowledge they have. Like in Vim, the exact format is specified via the buffer-local `grepformat` and `errorformat` options, the latter most times set by the filetype/compiler plugin. Terminals don't have this knowledge.


> Adding `file://` would make the output more ugly for the human eyes.

That's one reason why it's requested to implement the escape codes, so it's not visible to the eye, only to the terminal.

---

_Comment by @ghost on 2023-01-07 12:22_

> @guettli:
> 
> > The pattern `file:linenumber ....` for output of compilers/lints is very common. This worked even 20 years ago (when I used emacs).
> 
> Because editors can implement this, using the knowledge they have. Like in Vim, the exact format is specified via the buffer-local `grepformat` and `errorformat` options, the latter most times set by the filetype/compiler plugin. Terminals don't have this knowledge.
> 

FWIW, have a look at KDE's Konsole (Edit current profile -> Mouse -> Miscellaneous), it's had this functionality for some time (using regular expressions to detect file urls, both absolute and relative), i.e. clicking something in the output of grep/rg/compiler errors ...etc in the form `path/to/some/file.h:100:10` would open file.h at line 100 and column 10 in a variety of Linux text editors, Kate, gVim, Gedit ...etc, with an option to specify a custom command line to an editor that has cli options to open at line/column.

---

_Comment by @nbenitez on 2023-03-22 18:26_

It'd be cool to have this feature in, so we can eg. open matches directly in our code editor by clicking on them, or any similar use case.

The maintainer of the now default GNOME console app [has said](https://gitlab.gnome.org/GNOME/console/-/issues/142#note_1451871) that is not adding a custom hyperlink feature because they already support the standard link format. So I think ripgrep should use this standard link format so we can open files directly from ripgrep output.

---

_Comment by @ltrzesniewski on 2023-03-26 13:54_

How about making the hyperlink format configurable?

That's what [delta](https://github.com/dandavison/delta) does for its ripgrep support, as described by @dandavison [here](https://github.com/BurntSushi/ripgrep/issues/86#issuecomment-1469717706). See the [delta docs](https://dandavison.github.io/delta/grep.html).

In a nutshell:  
Given that any problem can be solved by adding an extra level of indirection, ripgrep could use a custom URI protocol, which would be registered to invoke a tool such as [open-in-editor](https://github.com/dandavison/open-in-editor), which would know how to open an editor at a specific line.  
(I'm assuming that knowing how to open a given editor at a specific line is out-of-scope for ripgrep itself).

This would need to be enabled with an additional parameter, maybe something such as:  
`--hyperlink-format 'editor://{file}:{line}'`

The default pattern could be `file://{hostname}/{file}` on Unix and `file:///{file}` on Windows (with a special provision for WSL).

Also, having the hyperlink format be configurable would accommodate everyone's needs.

I can add that to #2322 if there's a chance it could be accepted.

---

_Comment by @ltrzesniewski on 2023-03-31 17:02_

I noticed that VSCode [provides an URL handler](https://code.visualstudio.com/docs/editor/command-line#_opening-vs-code-with-urls) out of the box, so using it would be very simple with this option:

`--hyperlink-format 'vscode://file/{file}:{line}'`

I think I'm going to implement it, as it'll allow hyperlinking line numbers.

After a few months of using ripgrep with file hyperlinks, I can tell they make exploring match results much more convenient, but directly going to the matching line in the editor would be so much better.


---

_Comment by @ltrzesniewski on 2023-04-02 21:14_

I implemented customizable hyperlinks (which *can* link directly to a matching line/column) in #2483 if you want to give them a try. Optimized builds can be found [here](https://github.com/ltrzesniewski/ripgrep/actions/workflows/preview.yml?query=is%3Asuccess) (just pick the newest one). See the PR description for more details.

For Windows Terminal users, be aware that anything other than `file://` won't work until the next preview release, as [this change](https://github.com/microsoft/terminal/pull/14993) needs to ship first.

---

_Comment by @097115 on 2023-04-10 16:48_

@ltrzesniewski 

Thanks for your builds!

However, the hyperlinks seem not to work under `tmux` (`ls` still works, though). Tested in `kitty` and `alacritty`.

---

_Comment by @nbenitez on 2023-04-10 16:58_

@097115 Excerpt from https://superuser.com/questions/1771573/file-hyperlink-not-working-under-tmux :

_Support for hyperlinks has been added to tmux in Git a few months ago and will be available in the next release, probably 3.4_

---

_Comment by @097115 on 2023-04-10 17:04_

I'm on 3.4 and **as I said in my first comment**, `ls --hyperlink=always` does work for me in `tmux`. While @ltrzesniewski's custom build of `rg` doesn't.

---

_Comment by @ltrzesniewski on 2023-04-10 17:34_

@097115 thanks for your feedback. I'll take a look.


---

_Comment by @ltrzesniewski on 2023-04-12 21:02_

@097115 I believe this is an issue in tmux, and have reported it here: https://github.com/tmux/tmux/issues/3525

---

_Comment by @097115 on 2023-04-13 01:40_

Thanks again, Lucas!

---

_Comment by @BurntSushi on 2023-09-22 17:57_

For anyone following this issue, there is active discussion about adding support for it in this PR: https://github.com/BurntSushi/ripgrep/pull/2483

Opinions are welcome because this is clearly a very thorny feature to add support for. It is not simple and it looks like it is going to be integration specific. I won't be surprised if I get this wrong in some way in the initial release unfortunately.

---

_Closed by @BurntSushi on 2023-09-25 18:39_

---

_Comment by @BurntSushi on 2023-09-25 18:56_

Support for hyperlinks is on master. I would love feedback here: https://github.com/BurntSushi/ripgrep/discussions/2611

---
