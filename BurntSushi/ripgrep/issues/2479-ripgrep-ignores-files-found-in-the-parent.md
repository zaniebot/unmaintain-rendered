```yaml
number: 2479
title: "Ripgrep ignores files found in the parent directories `.ignore` file"
type: issue
state: closed
author: scottchiefbaker
labels:
  - doc
  - rollup
assignees: []
created_at: 2023-03-27T21:55:08Z
updated_at: 2023-11-25T20:03:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2479
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep ignores files found in the parent directories `.ignore` file

---

_@scottchiefbaker_

#### What version of ripgrep are you using?

ripgrep 13.0.0

#### How did you install ripgrep?

Installed directly from the GitHub release

#### What operating system are you using ripgrep on?

CentOS Stream 8

#### Describe your bug.

Ripgrep is ignoring files found in an `.ignore` file in the **parent** directory?

#### What is the expected behavior?

I was ripgreping for a pattern I knew was in my source, and wasn't getting a result. I immediately checked `.gitignore` and `.ignore` files in the current directory and there was nothing that would have filtered out my source. Testing again with `--no-ignore` showed the content I was expecting.

I'm assuming this is the intended behavior? I couldn't find any reference in the docs to `ripgrep` going **up** the tree looking for `.ignore` files. I would maybe expect it to look in each sub directory, but not go up. If this is not a bug then the [documentation](https://github.com/BurntSushi/ripgrep/blob/a7ae9e4043533ba1b6e2f63b989b84f27d961b37/doc/rg.1.txt.tpl#L122) should be updated to indicate that 

---

_Comment by @BurntSushi on 2023-03-27 22:10_

Yes, it is definitely intended. Basically, ripgrep farms out a lot of the docs for this to `man gitignore`. For example, from the GUIDE:

> For a more in depth description of how glob patterns in a .gitignore file are interpreted, please see `man gitignore`.

Now obviously `man gitignore` doesn't mention `.ignore`, so I think we can improve the docs there by essentially mentioning that "`.ignore` and `.rgignore` work just like `.gitignore`, but with no special connection to git and the presence of `.git` directories."

I don't think I documented this because it never occurred to me that someone would expect the behavior you want. Moreover, the presence of the `--no-ignore-parent` flag (and its docs) implies the behavior that ripgrep has.

> I was ripgreping for a pattern I knew was in my source, and wasn't getting a result. I immediately checked .gitignore and .ignore files in the current directory and there was nothing that would have filtered out my source. Testing again with --no-ignore showed the content I was expecting.

I'd strongly recommend that you check out the `--debug` flag. It will tell you what things ripgrep is skipping. That way, you don't have to read gitignore files and apply the matching rules mentally. You can just see exactly what ripgrep is doing.

---

_Label `doc` added by @BurntSushi on 2023-03-27 22:11_

---

_Comment by @scottchiefbaker on 2023-03-27 22:19_

My real-world use case for this was that I had a directory `~/html/content/books/2023/` that I was trying to `ripgrep`. I had a `.gitignore` file in `~/html/` which was three directories up from where I was, so it was out of sight, out of mind. I had an overly broad rule that applied to the HTML directory, but not the books sub-dir. I didn't know about `--no-ignore-parent` until you mentioned it. That would have helped also. 

Ultimately I think a modification to the docs that mentions that `ripgrep` will look for ignore files in **all** parent directories would be sufficient. That's where I was expecting to find this information.

Thanks for a great tool. I could **not** code nearly as efficiently without `ripgrep`. It's been a real game-changer in my workflow.

---

_Comment by @BurntSushi on 2023-03-27 22:25_

Yeah the problem is where to put them. There are a lot of these sorts of caveats, and they just can't all be front and center. So if this information is buried in the GUIDE somewhere, would have it actually helped you? If it has to go in the very top of the man page to have helped you, then I'm not actually sure it's worth doing. Because that space is very content dense, and adding more to it reduces the visibility of other things that are perhaps more important than a detail like this.

---

_Comment by @Timmmm on 2023-05-22 15:42_

It would be good to put this comment in the docs for the `ignore` crate too (on the first page). It's currently a bit unclear which `.gitignore` files it uses.

---

_Comment by @githorse on 2023-08-24 13:17_

This one bit me as well. I had `**/node_modules/**` in my `.gitignore`. But sometimes I do need to find something inside `node_modules/`, so I `$ cd node_modules/library-whatever` and `rg thingamabob` ... and I get nothing. 

If I replace `**/node_modules/**` with `**/node_modules/` in `.gitignore`, then everything works as expected. So seems the problem is this overly broad ignore pattern is screening out any file that has `node_modules` anywhere in the path. 

Ok, my bad. But maybe `rg` could handle this case better? It took me a while to figure out what was going on here. The distinction between `**/node_modules/**` and `**/node_modules/` is pretty subtle, and it's non-obvious how that should affect the behavior of `rg`. If I'm currently *inside* `/node_modules`, my `.ignore` file says to ignore `**/node_modules/**`, and I run a `rg` search, clearly I'm doing something wrong. So maybe `rg` could either (1) ignore my `.ignore` in this case, since I didn't really mean it; or (2) issue a (more specific) warning about what's going on. (I did try `--debug` and the answer is in there but hard to find and parse in all that output.)

(1) is a little questionable but I think you could make a pretty good argument that, if the current working directory (not subdirectories, obviously) matches a pattern in some parent `.ignore` file, that particular ignore pattern should be skipped (since not doing so can only ever result in zero results, which is clearly not what the user wanted).



---

_Comment by @BurntSushi on 2023-08-24 13:23_

@githorse I suspect doing something smarter like what you want without breaking other things is harder than you imagine. And in particular, "looking for patterns that match the cwd" is highly suspect.

ripgrep should emit a warning if it searches nothing. That is the intended failure mode here.

---

_Label `rollup` added by @BurntSushi on 2023-11-22 01:19_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
