```yaml
number: 1719
title: "support default config paths when RIPGREP_CONFIG_PATH isn't set"
type: issue
state: closed
author: mvdan
labels:
  - wontfix
assignees: []
created_at: 2020-10-30T10:47:07Z
updated_at: 2021-01-14T16:26:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1719
synced_at: 2026-01-12T16:13:24Z
```

# support default config paths when RIPGREP_CONFIG_PATH isn't set

---

_@mvdan_

This is an extension of https://github.com/BurntSushi/ripgrep/pull/772. It reads:

> This does not add any support for auto-loading config files from pre-determined locations like one's `$XDG_CONFIG_HOME` directory. [It was decided](https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-362850321) to punt on that for now, and one wonders how far we can get with just the env var approach!

It seems like the main reason was because Rust didn't have good software to do the right thing on all platforms in a nice way. It's been nearly three years, so I hope that situation has changed :) 

I think this could be solved without going into project-specific or PWD-relative config files. Those use cases can continue to use explicit env vars like before, for now.

My personal use case is not needing to pollute my global environment to have a global ripgrep config file. Pretty much every other tool I use already knows to look in `~/.config/<name>/somefile` by default, and I think it's reasonable to expect the same here. Another advantage would be to create a standard for where such a config file should be, making "dotfiles" repos more consistent with their ripgrep config locations.

---

_Comment by @BurntSushi on 2020-10-30 17:25_

This was recently discussed in #1668, but unbelievably, the user who opened that issue deleted it. Sigh. And unfortunately, my email record of it only includes the user's comments, not mine. And unfortunately, the [Internet Archive does not have it](http://web.archive.org/web/*/https://github.com/BurntSushi/ripgrep/issues/1668*). So I guess I'll re-litigate it. I've updated my notification settings so that I should now receive emails including my own replies.

> It seems like the main reason was because Rust didn't have good software to do the right thing on all platforms in a nice way. It's been nearly three years, so I hope that situation has changed :)

I don't think it has actually. And even if it has, I'm not sure that I want to do it anyway. The `RIPGREP_CONFIG_PATH` environment variable has worked really well IMO. It's simple and gives users full control over whether the configuration lives.

> My personal use case is not needing to pollute my global environment to have a global ripgrep config file.

Personally, I am not sympathetic to this. My environment is littered with oodles of variables. And even so, I don't think it's a problem worth solving. That is, I don't perceive this as a real problem that many users experience.

> Another advantage would be to create a standard for where such a config file should be, making "dotfiles" repos more consistent with their ripgrep config locations.

That's a good one, but `RIPGREP_CONFIG_PATH` would (probably) never go away for backwards compatibility reasons even if ripgrep started supporting XDG. So I guess I'd file this under "unfortunate."

---

_Label `question` added by @BurntSushi on 2020-10-30 17:26_

---

_Comment by @mvdan on 2020-10-31 09:07_

I certainly won't delete this thread :) I did try to search for previous incarnations but found none.

> My environment is littered with oodles of variables. And even so, I don't think it's a problem worth solving.

I think that's up to personal preference, but I see not being forced to litter one's global environment akin to not being forced to litter one's home directory with "dot files". This is the same reasosn why many tools are being lobbied to accept `~/.config/foo` as well as `~/.foo`.

> That's a good one, but `RIPGREP_CONFIG_PATH` would (probably) never go away for backwards compatibility reasons even if ripgrep started supporting XDG. So I guess I'd file this under "unfortunate."

You'd keep backwards compatibility for sure, but I also imagine that the vast majority of users would gradually switch to the default location. That seems like the best of both worlds to me :)

---

_Comment by @mvdan on 2020-10-31 09:28_

As an aside, I'm very surprised that an external user can delete their thread and, by extension, delete the repo owner's comments as well. This seems like bad design. At most, they should be able to delete their own comments, but not take the entire thread down with them. Only the admins should be able to take that nuclear option.

---

_Comment by @mvdan on 2020-10-31 09:38_

> the user who opened that issue deleted it

Hm, actually, I can't find any delete button in this issue, for example. Maybe it was a report that resulted in a deletion by GitHub staff. Have you reached out to them?

---

_Comment by @egonelbre on 2020-10-31 09:54_

> Personally, I am not sympathetic to this. My environment is littered with oodles of variables. And even so, I don't think it's a problem worth solving. That is, I don't perceive this as a real problem that many users experience.

Unfortunately this is a problem on Windows where you are limited to 32K bytes of environment variables. And I've hit that limit a few times. Although, I doubt this being a problem for most people.

---

_Comment by @thepudds on 2020-10-31 14:42_

Hi @BurntSushi, at least part of the 1668 conversation is in the google cache of the gitmemory site:

https://webcache.googleusercontent.com/search?q=cache:tzcRYmo7rH4J:https://gitmemory.com/issue/BurntSushi/ripgrep/1668/682573882+&cd=1&hl=en&ct=clnk&gl=us&client=safari

---

_Comment by @thepudds on 2020-10-31 15:14_

Here is a comment from @BurntSushi from there:

_________

Thanks. Yes, this is a sore topic for me. Because people continually underestimate the difficulty and complexity involved in this. Even after I told you how hard it was, you just kind of shrugged it off and pointed me to functions that get the home directory of a user, but that isn't the hard part. Not even close. Even just describing the complexity of loading configuration files takes time, because the process for it is very different across Linux, macOS and Windows. (And even that is a controversial statement, because some people would prefer that Linux and macOS use the same process!)

> all those question have a default, as sometimes people realize that defaults have value.

I mean, I created ripgrep. It and its ancestors (ack and ag) are perhaps one of the most obvious examples that defaults matter. So this argument doesn't really need to be made. The point is whether setting a default in this case is worth the benefits. I don't need to be convinced that defaults have benefits in the first place.

Given that you suggested dropping a config file into a user's $HOME directory, my guess is that you just simply aren't aware of the vociferousness of people who absolutely hate this. As a practical matter, I do not want to be the target of their zeal. Similarly, building out the infrastructure to get platform specific configuration file placement correct is just not something that I'm willing to spend my free time on. Somebody else has to do it. Some folks have, to varying levels of success. The directories crate is probably the best example, but I won't use it. (Partially due to a couple technical reasons, but primarily due to non-technical ones and I won't discuss those publicly.) With that said, I'd encourage you to examine the implementation of that crate to get an idea of just how complex the task is.

> "look, since no standard exists for this thing, I choose X. If that dont suit you then use environment variable"

This ignores the costs of having both an environment variable and support for platform specific config directories, and the complexity of maintaining both instead of one.

> Look at this, its even existed since 2005:
php/php-src@1623537#diff-43cd40d427070a1355f39a283ae22ac7R1187

I don't know what you're trying to show me with this link. Like, do you think I'm unaware that many tools plop config files into a user's home directory? I don't need to be shown that.

There are a few final points remaining:
* As a matter of subjective value judgment, I personally think you are greatly overstating the annoyance of setting an environment variable. I admit it is a cost over having a default location to search for, but it is a teeny cost and avoids 1) attracting the zeal of Internet mobs that never stop whining about improper placement of config files and 2) avoids the implementation complexity of getting it correct.
* An environment variable has benefits. For example, if you know that RIPGREP_CONFIG_PATH isn't set, then you also know that ripgrep will never read anything from any configuration file. Similarly, if you need to know the location of ripgrep's config file, then all you need to do is consult RIPGREP_CONFIG_PATH. There's no need to go searching for the config file across various directories (depending on platform).
* A configuration file is not the only way to configure ripgrep. Aliases, functions or wrapper scripts are other fine ways to do it. AIUI, this is difficult to do correctly on Windows, and this was one of the primary reasons why configuration files were added in the first place. Otherwise, I probably would have insisted on folks using some other mechanism.


---

_Comment by @BurntSushi on 2020-10-31 16:26_

@thepudds Thanks! I guess reading that, not all of it is directly relevant to this issue. There were some peculiarities with the previous issue that I was addressing that don't really apply to this one.

I think this issue comes down to whether it's worth adding support for default platform specific configuration files. One of the problems that I've said historically is that there aren't any crates yet that solve that problem to my satisfaction (which is still true, AFAIK). But even if there were, I'm not really convinced that setting an environment variable is enough of a problem to be worth adding this. [Setting random env vars is a common practice](https://github.com/BurntSushi/dotfiles/blob/master/.envrc) I think, and I don't really buy the comparison with littering one's `$HOME` directory with dotfiles. It's common to list the contents of one's home directory, but less common to list the entire environment and read each variable. On my system, `env | wc -l` outputs `63`, which is quite big. And many of them aren't even variables that I've explicitly set (e.g., `SSH_AUTH_SOCK`). But then again, I'm not bothered by dotfiles littered in my `$HOME` directory anyway.

---

_Comment by @jacobeatsspam on 2021-01-14 15:58_

It's hard to look at the mountain of work that was done to make ripgrep possible and think that config files are too challenging. Considering the number of config related issues created and the hours you've poured into replying ~and considering~ to them, this seems to come down to a simple choice to not see the value others see and have explained for years.

For those focused on the code, I don't touch rust, but a quick search shows a few options. e.g. https://github.com/cjbassi/platform-dirs-rs

---

_Comment by @BurntSushi on 2021-01-14 16:21_

> and think that config files are too challenging. 

ripgrep has support for config files.

> into replying ~and considering~ to them

I consider this to be a comment made in bad faith. If you do it again, you won't be welcome here.



> this seems to come down to a simple choice to not see the value others see and have explained for years.

This also feels like bad faith to me. I understand the value of platform specific default configuration directories. I even addressed it in my comments above. I just don't think they provide enough value over setting an environment variable.

I personally am getting pretty close to banning all discussion of config file location from this tracker. I initially believed that delegating the decision to an environment variable would be enough to satiate users: it's simple to understand and implement, permits putting the config file anywhere and you generally don't see folks complaining about setting env vars. Most of us probably have a lot set and an established mechanism to do so, regardless of platform. But lately, I've received a number of forceful and frustrating comments from users seemingly complaining that this isn't good enough. I have yet to see any compelling reason to reverse course. For that reason, I'm closing this issue.

---

_Closed by @BurntSushi on 2021-01-14 16:21_

---

_Label `question` removed by @BurntSushi on 2021-01-14 16:21_

---

_Label `wontfix` added by @BurntSushi on 2021-01-14 16:21_

---

_Comment by @mvdan on 2021-01-14 16:26_

Thanks for the patience, @BurntSushi. I of course was not aware of the previous deleted thread when I opened this one. I sympathize with the struggle of having to say "no" to features in large open source projects and getting reactions because of that.

> I have yet to see any compelling reason to reverse course.

At least from my point of view, it's just "it would be nice to not clutter my global environment with an extra variable". Though I get that that's not a big deal and, ultimately, a subjective choice. It's not a hard-coded limitation, after all.

---
