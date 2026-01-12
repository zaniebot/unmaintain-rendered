```yaml
number: 2322
title: Add hyperlinks
type: pull_request
state: closed
author: ltrzesniewski
labels: []
assignees: []
draft: true
base: master
head: hyperlinks
created_at: 2022-10-02T21:13:12Z
updated_at: 2023-04-02T17:03:22Z
url: https://github.com/BurntSushi/ripgrep/pull/2322
synced_at: 2026-01-12T18:23:14Z
```

# Add hyperlinks

---

_@ltrzesniewski_

This PR adds hyperlinks to the file paths for terminals which support it:

![image](https://user-images.githubusercontent.com/7913492/193475900-7fbd250b-7b1f-4c52-98cd-56fe316a082f.png)

It's just a draft for now, there's still work to do, but I'm opening this in case anyone would like to provide some early feedback. I'm still new to Rust, so this surely looks like garbage right now, but I'd like to make the code more idiomatic, bring it to the adequate speed and quality level, and learn in the process.

I ~still need to add~ added hostnames on Unix for better SSH support, but that [requires approval](https://github.com/BurntSushi/ripgrep/issues/665#issuecomment-840735760) from @BurntSushi.

This depends on https://github.com/BurntSushi/termcolor/pull/65, ~which will certainly need to be rewritten (because of the clunky API it currently introduces), or at the very least enhanced with stuff like `supports_hyperlinks()`.~

Fixes #665

---

This is what I have in mind for this PR:

 - On Unix: `file://hostname/path/file.txt`
 - On Windows: `file:///C:/path/file.txt`
 - On WSL: `file://wsl$/distribution/path/file.txt`

Hostnames on Unix would be useful for using ripgrep through SSH (SSH is mostly used to connect to Unix destinations). They could also be added on Windows after https://github.com/microsoft/terminal/issues/14116 is fixed.

---

Preview builds can be found [here](https://github.com/ltrzesniewski/ripgrep/actions/workflows/preview.yml?query=is%3Asuccess).

---

_Converted to draft by @ltrzesniewski on 2022-10-02 21:14_

---

_Comment by @kovidgoyal on 2022-10-03 08:09_

I suggest you look at https://sw.kovidgoyal.net/kitty/kittens/hyperlinked_grep/ In particular 

By default, this kitten adds hyperlinks for several parts of ripgrep output: the per-file header, match context lines, and match lines. You can control which items are linked with a --kitten hyperlink flag. For example, --kitten hyperlink=matching_lines will only add hyperlinks to the match lines. --kitten hyperlink=file_headers,context_lines will link file headers and context lines but not match lines. --kitten hyperlink=none will cause the command line to be passed to directly to rg so no hyperlinking will be performed. --kitten hyperlink may be specified multiple times.

I am pretty sure users will want to customize what parts of the output are hyperlinked.

---

_Comment by @ltrzesniewski on 2022-10-03 09:20_

I thought about adding a `--hyperlink` command line option, but wasn't sure exactly what options I should put there. That's something that should be discussed I guess.

It looks like hyperlinked_grep is able to point the editor directly at the matched line, which is *awesome*.  Unfortunately, that doesn't seem to be possible to achieve in a *standard* way with just the `file://` protocol alone. I took a quick glance at your code, and you're adding the line number in the fragment part of the URI (`file://host/file#line`), which, you guessed it... doesn't work on Windows (the file won't even open). 😭



---

_Comment by @kovidgoyal on 2022-10-03 09:59_

On Mon, Oct 03, 2022 at 02:20:23AM -0700, Lucas Trzesniewski wrote:
> I thought about adding a `--hyperlink` command line option, but wasn't sure what exactly what options I should put there. That's something that should be discussed I guess.

Yes, indeed, it should be discussed.

> 
> It looks like hyperlinked_grep is able to point the editor directly at the matched line, which is *awesome*.  Unfortunately, that doesn't seem to be possible to achieve in a *standard* way with just the `file://` protocol alone. I took a quick glance at your code, and you're adding the line number in the fragment part of the URI (`file://host/file#line`), which, you guessed it... doesn't work on Windows. 😭

I feel for you, maybe come over to the dark side :)) On a serious note,
there is nothing preventing a windows based terminal from parsing the
fragment part of the URL and either discarding it or using it, as it
sees fit.



---

_Comment by @ltrzesniewski on 2022-10-03 12:47_

> I feel for you, maybe come over to the dark side :))

I have WSL on Windows, so I'm getting the best of both worlds. 😜 

> On a serious note, there is nothing preventing a windows based terminal from parsing the fragment part of the URL and either discarding it or using it, as it sees fit.

That's not really how things work on Windows, and I don't think Microsoft would change their terminal that way. This would need special-casing the `file://` protocol in the terminal, enhancing it with `#line` support, then adding configurable apps by file types with custom command line argument patterns. They won't do such a thing.

Let me explain: URIs are handled by the shell (a system component), and applications can be associated with URI schemes. For instance, `https://` would be associated with the web browser. So, the shell decides how to handle a particular URI, not the terminal app. That way, you get an integrated UI experience which is consistent across applications. I'm not sure how exactly `file://` is set up, but it must be tied to something that decodes the URI to a file path, and chooses the application to open based on the file extension, which is configurable in the system settings. But this configuration doesn't let you specify additional parameters to pass to the target application, such as the file line.

This could be worked around with a custom URI scheme such as `ripgrep://`, but I'm pretty sure that wouldn't be accepted. 🙂 


---

_Comment by @kovidgoyal on 2022-10-03 13:27_

On Mon, Oct 03, 2022 at 05:47:38AM -0700, Lucas Trzesniewski wrote:
> That's not really how things work on Windows, and I don't think Microsoft would change their terminal that way. This would need special-casing the `file://` protocol in the terminal, enhancing it with `#line` support, then adding configurable apps by file types with custom command line argument patterns. They won't do such a thing.

No, they can just strip the fragment from the URL before passing it to
the shell. If they want to remain backwards that's their lookout, I don't
see why the rest of the world should suffer for it. 

> 
> Let me explain: URIs are handled by the shell (a system component), and applications can be associated with URI schemes. For instance, `https://` would be associated with the web browser. So, the shell decides how to handle a particular URI, not the terminal app. That way, you get an integrated UI experience which is consistent across applications. I'm not sure how exactly `file://` is set up, but it must be tied to something that decodes the URI to a file path, and chooses the application to open based on the file extension, which is configurable in the system settings. But this configuration doesn't let you specify additional parameters to pass to the target application, such as the file line.

Umm, I am pretty sure if you pass a http:// url with a fragment via
the shell, the browser will handle the fragment just fine. I dont see
why the shell cant pass file:// urls with fragments to applications, all
it requires is an extra check box in their configuration UI, to indicate
to the shell not to modify file urls when passed to the specified
application. 

Or just use a non-Microsoft terminal if you must use windows.

Regardless, I am very strongly against crippling hyperlink support in rg
to satisfy the limitations of Microsoft's software. If you really want
to do it, add an option to control whether to add line numbers to the
file URLs, or better yet create a template system with a default
template that is file://{hostname}{path}#{line}

Then users of rg can adapt the hyperlink to use whatever scheme and
level of usefulness they require. 



---

_Comment by @ltrzesniewski on 2022-10-03 14:11_

> I don't see why the rest of the world should suffer for it.

Like I said before, the behavior can be slightly different on Windows and Linux. You don't need to suffer anything.

Do you know if other terminals can interpret the fragment part of the URI as a line number, and act accordingly?

> I dont see why the shell cant pass file:// urls with fragments to applications, all it requires is an extra check box in their configuration UI, to indicate to the shell not to modify file urls when passed to the specified application.

Because of the intermediate step *in the Windows shell* which translates the URI to a file path, which apparently cannot handle it. The target appliction is passed the *file path*, not the URI.

*"all it requires"* is an obscure change at the OS level. That won't happen.

> Or just use a non-Microsoft terminal if you must use windows.

I want this feature to work *by default*. Otherwise, there's no point. It also happens I want it to work with the terminal I use every day.

And finally, please stop being aggressive. I'm just trying to *add a feature* here, in good faith. I'm not trying to cripple anything. It doesn't need to be perfect right from the start. Remember it can always be enhanced afterwards. In the meantime, your kitten can continue to fill the gaps.


---

_Comment by @kovidgoyal on 2022-10-03 14:33_

On Mon, Oct 03, 2022 at 07:11:31AM -0700, Lucas Trzesniewski wrote:
> > I don't see why the rest of the world should suffer for it.
> 
> Like I said before, the behavior can be slightly different on Windows and Linux. You don't need to suffer anything.

ripgrep cannot know if its output is being interpreted on linux or
windows. It can be running over ssh on a linux machine but its output
can be interpreted on a windows machine. Or vice versa. You really
cannot include compile time tests to control output in a terminal application.

> 
> Do you know if other terminals can interpret the fragment part of the URI as a line number, and act accordingly?

No clue. Probably not, if I had to guess.

> 
> > I dont see why the shell cant pass file:// urls with fragments to applications, all it requires is an extra check box in their configuration UI, to indicate to the shell not to modify file urls when passed to the specified application.
> 
> Because of the intermediate step *in the Windows shell* which translates the URI to a file path, which apparently cannot handle it. The target appliction is passed the *file path*, not the URI.

Yes, hence the suggestion to have a checkbox to pass the URI *instead*
of the file path.

> 
> *"all it requires"* is an obscure change at the OS level. That won't happen.

I already know it wont happen. I was simply pointing out that it is not hard to do.

> 
> > Or just use a non-Microsoft terminal if you must use windows.
> 
> I want this feature to work *by default*. Otherwise, there's no point. It also happens I want it to work with the terminal I use every day.

You want it to work by default on *your terminal* in *your use case*,
breaking it by default for other people and other usecases. And you
picked the terminal and usecase that is the *most limited*. No remote
hosts and no line numbers. There is simply nothing stopping your
favorite terminal from stripping fragments and hostnames if it doesnt
like them. But no, according to you instead by default ripgrep must
produce crippled output, so that it works by default for *you*. I don't
consider clicking on a search result and being taken to a random point
in the file (as will happen if the editor remembers last opened
position) or the top of the file *working by default*.

> 
> And finally, please stop being aggressive. I'm just trying to *add a feature* here, in good faith. I'm not trying to cripple anything. It doesn't need to be perfect right from the start. Remember it can always be enhanced afterwards. In the meantime, your kitten can continue to fill the gaps.

You are adding the feature *wrong*. I thought you were asking for
feedback, so I wasted my precious time giving you some, my mistake. But
OK, whatever. I will continue to work around this in hyperlinked_grep.
Sorry for wasting your time and good luck. You wont be hearing from me
again.



---

_Comment by @ltrzesniewski on 2022-10-03 16:00_

*Sigh.* Looks like you don't understand what I've been saying in the issue thread.

> It can be running over ssh on a linux machine but its output can be interpreted on a windows machine.

Yes. And SSH support will work fine *if I include the hostname on Linux*. Because there's no Linux to Windows SSH.

> No clue. Probably not, if I had to guess.

So, in other words, you're trying to change the default behavior of ripgrep to nonstandard links just for kitty support.

> You want it to work by default on *your terminal* in *your use case*, breaking it by default for other people and other usecases.

Well, *of course* I want to make it work for myself. But I won't be breaking anyone else. On the contrary, I want this to work for most people out of the box.

You're trying to make me generate nonstandard links which only *your* terminal understands. Now *that* is breaking it by default for other people, like me.

> And you picked the terminal and usecase that is the *most limited*.

Yes. That's the lowest common denominator which works for everyone.

> No remote hosts and no line numbers.

- Remote hosts will be supported if I include the hostname on Linux.
- Line numbers are only supported by kitty, and can be handled by hyperlinked_grep just fine.

> ripgrep must produce crippled output

ripgrep must produce *standard* output.

> I don't consider clicking on a search result and being taken to a random point in the file (as will happen if the editor remembers last opened position) or the top of the file *working by default*.

Then, as you're fond of saying, change your settings. Or continue to use hyperlinked_grep. Just don't break it for other people.

> You are adding the feature *wrong*.

This is your opinion. I've heard it. I just don't agree with it.

Thanks for your feedback.


---

_Comment by @kovidgoyal on 2022-10-07 05:31_

> _Sigh._ Looks like you don't understand what I've been saying in the issue thread.

Sigh. Looks like you dont have a clue what you are doing.

> 
> > It can be running over ssh on a linux machine but its output can be interpreted on a windows machine.
> 
> Yes. And SSH support will work fine _if I include the hostname on Linux_. Because there's no Linux to Windows SSH.

Yes there is. Google is your friend.

> 
> > No clue. Probably not, if I had to guess.
> 
> So, in other words, you're trying to change the default behavior of ripgrep to nonstandard links just for kitty support.

No, I am trying to make the output of ripgrep fit for purpose. The purpose here being, clicking on a search result and navigating to that result in an editor.

> 
> > You want it to work by default on _your terminal_ in _your use case_, breaking it by default for other people and other usecases.

It breaks only on terminals that pass file URLs without removing hostnames, fragments, queries etc from them onto systems which cant handle those. AKA terminals that dont sanitize untrusted input before passing it on. I feel for you that you are wedded to such a terminal.

> 
> Well, _of course_ I want to make it work for myself. But I won't be breaking anyone else. On the contrary, I want this to work for most people out of the box.

Yes, you will. You have conveniently ignored the fact that output from a search tool when clicked should go to the search result inside the file, not just the file. By leaving off line numbers you are *breaking that extremely reasonable expectation* for everybody, forcing them to use wrappers to work around your lack.

> 
> You're trying to make me generate nonstandard links which only _your_ terminal understands. Now _that_ is breaking it by default for other people, like me.

Non standard? Which standard are you referring to? Last I checked fragments are a part of every standard that determines what constitutes a valid URL.

> 
> > And you picked the terminal and usecase that is the _most limited_.
> 
> Yes. That's the lowest common denominator which works for everyone.

Except, it doesn't actually work.

> 
> > No remote hosts and no line numbers.
> 
> * Remote hosts will be supported if I include the hostname on Linux.
> * Line numbers are only supported by kitty, and can be handled by hyperlinked_grep just fine.

Is it really your position that you think clicking on search results should take you to random positions in a file?

> 
> > ripgrep must produce crippled output
> 
> ripgrep must produce _standard_ output.
> 

Again, what standard?

> > I don't consider clicking on a search result and being taken to a random point in the file (as will happen if the editor remembers last opened position) or the top of the file _working by default_.
> 
> Then, as you're fond of saying, change your settings. Or continue to use hyperlinked_grep. Just don't break it for other people.

You mean you and your input unsanitizing, crippled terminal.

> 
> > You are adding the feature _wrong_.
> 
> This is your opinion. I've heard it. I just don't agree with it.
> 
> Thanks for your feedback.

You are most welcome.


---

_Comment by @ltrzesniewski on 2022-10-07 09:07_

That discussion leads to nowhere. Now please stop insulting me and stick to your word:

> You wont be hearing from me again.


---

_Comment by @BurntSushi on 2022-10-07 10:56_

@kovidgoyal You are coming across as pretty agressive and it is a VERY POOR way to get your point across. Stop it or you will no longer be welcome here.

The overall temperature of this conversation should be reduced considerably. There is no reason for it to be heated.

---

_Comment by @kovidgoyal on 2022-10-07 11:02_

Sure, happy to leave. I dont know why I bothered. Good luck.

---

_Comment by @ltrzesniewski on 2022-10-29 11:14_

I think this should now be ready for a first review, but I'm keeping the PR marked as draft as it can't be merged yet:

- The [termcolor feature](https://github.com/BurntSushi/termcolor/pull/65) needs to be approved/merged/released first.
- I commented out a regression test temporarily until you decide if you'd like to add a `--hyperlinks` switch or similar.

Also, sorry for the drama in this thread.


---

_Comment by @ltrzesniewski on 2023-04-02 17:03_

Superseded by #2483

---

_Closed by @ltrzesniewski on 2023-04-02 17:03_

---
