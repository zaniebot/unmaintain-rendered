```yaml
number: 51
title: Color customization
type: issue
state: closed
author: eugene-bulkin
labels:
  - enhancement
assignees: []
created_at: 2016-09-24T04:57:24Z
updated_at: 2018-02-22T21:18:21Z
url: https://github.com/BurntSushi/ripgrep/issues/51
synced_at: 2026-01-12T16:13:21Z
```

# Color customization

---

_@eugene-bulkin_

We should definitely try to support customizing colors in the output similar to how `ag` does it. Currently they support 3 color customizations, as per [this issue](https://github.com/ggreer/the_silver_searcher/issues/90):
- `--colors-match`
- `--colors-path`
- `--colors-line-number`

Implementing the part where we set colors based on an argument is not particularly hard (just a matter of translating user input to `term::Attr` enums). Parsing input in the bash terminal format like `ag` does was not difficult to implement. However, this limits Windows usage because we don't have an easy translation between those colors.

How should we go about supporting these color customizations for both Linux, Mac and Windows?


---

_Comment by @eugene-bulkin on 2016-09-24 04:58_

For reference, here's a working example of using the bash format:

![Colored matches](https://cloud.githubusercontent.com/assets/3990681/18806198/c6ece33a-81d8-11e6-90c6-9a742b8a5cbf.png)


---

_Comment by @BurntSushi on 2016-09-24 05:15_

I think we should take the common subset that is supported by all platforms and expose them as English names. I think that means these: https://stebalien.github.io/doc/term/term/color/index.html --- I don't know precisely which ones are or aren't supported on Windows though. Things like "bold" or background colors don't work either (I think), or rather, bold "RED" is "BRIGHT_RED". So maybe we do something like: `--color-path COLOR` where `COLOR` is `FgName[:Bold][/BgName[:Bold]]`, and `FgName`/`BgName` is any one of (case insensitively) `black`, `blue`, `cyan`, `green`, `magenta`, `red`, `white`, `yellow`. We could note that background colors aren't supported on Windows.

We should probably look at other tools that permit color customization and support both Linux and Windows and see what they do.

I'd like to avoid specifying colors as ANSI escape codes.


---

_Comment by @BurntSushi on 2016-09-24 05:19_

We could also do something a bit more generic that might be better for future expansion, like, `--color-path COLOR` where `COLOR` is `ATTR[,VAL][...]`. `ATTR` can be one of `Bold`, `Dim`, `Italic`, `Underline`, `Blink`, `Standout`, `Reverse`, `Secure`, `Fg` or `Bg`. If `Fg` or `Bg`, then `VAL` can be one of `black`, `blue`, ... and so on.

If any option chosen is known not to be supported on Windows, then `rg` should return an error.


---

_Comment by @eugene-bulkin on 2016-09-24 05:42_

So it looks like [Windows 10 supports ANSI codes](http://superuser.com/questions/413073/windows-console-with-ansi-colors-handling/1050078#1050078), but I see no evidence of anything earlier than that supporting any non-full-window coloring at all.


---

_Comment by @BurntSushi on 2016-09-24 05:51_

Yeah, it doesn't seem wise to jump on that so soon. We should stick with things that we know work. Maybe in a few years. :-)


---

_Comment by @cloudhead on 2016-09-24 17:25_

It might make more sense to do something like `--colors=path:blue,line:red`, instead of polluting the arg-space.


---

_Comment by @BurntSushi on 2016-09-24 18:07_

@cloudhead You'd actually need `--colors=path:fg:blue,line:bg:black`, for example. That doesn't sound like a bad idea.


---

_Comment by @eugene-bulkin on 2016-09-25 17:42_

I quite like that syntax, I think that would be reasonable.

And yeah, we can live with not customizing colors for Windows, but implementing for ANSI code-supporting terminals shouldn't be too bad. I'm thinking something like this:

`--colors=((path|line|match):(fg|bg|style):(color or bold/italic/etc.))*`

Not sure if `style` is the best word there, though.


---

_Comment by @BurntSushi on 2016-09-25 17:56_

@eugene-bulkin I'm OK with `style`. Maybe `attr` is a little better.

I think we should absolutely support customizing colors for Windows. I just think we should have good failure modes if an end user tries to specify something that won't work (like, `attr`, for instance).


---

_Comment by @eugene-bulkin on 2016-09-25 17:58_

Yeah, agreed. Should be reasonable to display a warning, right? Just say "Your current terminal does not support [insert thing here]."


---

_Comment by @BurntSushi on 2016-09-25 18:00_

I think it should be an error and cause `rg` to fail. Warnings are easy to miss, especially if they're emitted before search results.


---

_Comment by @eugene-bulkin on 2016-09-25 18:09_

Yeah, but is not being able to render a color they wanted really that worth killing the program? I view it as a soft error at best. But I don't have a problem with strict failure on all invalid input.


---

_Comment by @BurntSushi on 2016-09-25 18:21_

I actually feel pretty strongly about this. If an end user asks ripgrep to do something, and ripgrep knows it can't, then it should just fail and tell the user how they can fix their problem. We should try to follow this philosophy everywhere in ripgrep.

That's not to say that soft errors don't exist. One example of a soft error is if `ripgrep` can't descend into a directory. It emits the reason why it can't but otherwise continues on its merry way.


---

_Comment by @eugene-bulkin on 2016-09-25 18:25_

That's convincing enough for me. Makes sense!

So:
- Check if the user's environment allows them to change colors on their terminal. If not, `rg` fails with a relevant error message.
- If the user can change colors, we go through the argument, which looks something like `--colors=((path|line|match):(fg|bg|style):(color or bold/italic/etc.))*` and parse into `Attr` values.
- The `Printer` is then passed these values if they exist, overriding any default values.
- Where `write_matched_line` and other methods manually set colors, use the user-provided ones or defaults.

I think that's a relatively sufficient overview of the changes needed.


---

_Comment by @homeworkprod on 2016-09-25 18:29_

Since this kind of colorization is usually more of a somewhat permanent configuration thing rather than on a per-search basis, I think this should be settable via configuration file as well (although an alias will probably do just fine on Linux and Mac OS X for now, but not sure if that's a practical option on Windows).

Whether or not ripgrep currently reads settings from any non-ignore files (I don't know actually), it seems at least to be a good idea to design a syntax that doesn't interfere with what parsers for, say, TOML or INI-style syntax can handle. Or maybe there's nothing to worry about?


---

_Comment by @BurntSushi on 2016-09-25 18:44_

Let's please please avoid the question of a config file in this issue. There isn't actually an issue for a config file yet, but there probably should be. I'd like to at least try to resist the addition of a config file and see how far we can get with aliases.

> Check if the user's environment allows them to change colors on their terminal. If not, rg fails with a relevant error message.

Funnily enough, if you pass `--color` today and it doesn't work, ripgrep silently ignores it (but puts it in the debug log). Foot, meet mouth. :-)

What I was think here was if the end user specified an attribute on windows. I think those just never work.

But yes, otherwise that sounds OK.


---

_Comment by @BurntSushi on 2016-09-25 18:50_

> Funnily enough, if you pass --color today and it doesn't work, ripgrep silently ignores it (but puts it in the debug log).

Since this behavior is already in `ripgrep`, if you want to continue with soft errors, I would still merge it.


---

_Label `enhancement` added by @BurntSushi on 2016-09-25 19:03_

---

_Comment by @zachriggle on 2016-09-26 22:08_

I'd vote for making things compatible with `ag`.

Here's what I currently have:

``` sh
alias ag="ag --color-path '1;35' --color-line-number '0;37' --color-match '0;32' --color --break --group --heading"
```


---

_Comment by @BurntSushi on 2016-09-26 22:28_

@zachriggle I'm strongly opposed to specifying colors with ANSI escape codes. That doesn't seem like good UX to me and will leave Windows users scratching their heads.


---

_Comment by @zachriggle on 2016-09-26 22:32_

I don't see why it can't be both.  Specifying color /names/, without knowledge of the user's terminal theme, means the color mappings will be wrong.


---

_Comment by @BurntSushi on 2016-09-26 22:42_

@zachriggle Could you please explain more? I'm not especially knowledgable about ANSI color escape codes. What would fail if we only asked for color names?


---

_Comment by @anlutro on 2016-09-27 07:12_

Some people switch around colors in their terminal - for them "purple" does not necessarily mean purple. The color names will be correct for 99% (my guess) of people though. I think allowing both would be fine.

May I also suggest allowing configuration of colors in a config file? My biggest annoyance with `ag` is that I have to define an alias to make the colors the way I want them all the time (the default colors blend in with my `$PS1`).


---

_Comment by @BurntSushi on 2016-09-27 12:19_

@anlutro I'd like to keep discussion of a config file out of this issue. A config file doesn't have its own issue yet, but it probably should. I'd like to resist the idea and see how far we can get with aliases, but I expect I won't be able to hold out forever.

I kind of suspect that I'd like to punt on support ANSI escape codes for now. Not only because it seems like a pretty niche use case, but the term coloring library we're using presents a (mostly) platform independent abstraction, which isn't going to allow using ANSI escape codes directly. It's a much bigger change to support them.


---

_Comment by @llimllib on 2016-10-26 01:34_

Just to add a voice to the conversation, here's what the colors look like under [base16-twilight](https://chriskempson.github.io/base16/):

![1 bash 2016-10-25 21-32-23](https://cloud.githubusercontent.com/assets/7150/19710186/910eed0e-9afa-11e6-9191-8dc2f244c422.png)

It's nearly impossible to read the file name; the theme has switched the terminal's bold green for a color close to the background. Which is not your fault, I get, just a vote that I'd love to be able to customize the color.

(Also: I really love ripgrep, thanks for the tool. I use search a lot more now that it's an order of magnitude faster. Hope this is helpful and not carping)


---

_Comment by @BurntSushi on 2016-10-26 10:54_

@llimllib I don't think there's any question of whether we _should_ allow color customization. I'm currently trying to tie off gitignore handling, which is taking longer than I hoped, but I hope to move into refactoring terminal related things like colors after that.


---

_Comment by @BurntSushi on 2016-11-13 20:13_

OK, I'm finally ready to tackle this issue (and the litany of other color related issues). In particular, my plan is to jettison use of the `term` crate and roll my own platform independent abstraction.

I am going to move forward with this specification:

```
--colors=((path|line|match):(fg|bg|style):(value)) ...
```

The `--colors` flag specifies color settings for use in ripgrep's output. The `--colors` flag may be given multiple times. Each use of `--colors` sets the foreground/background color or the text style of a particular part of ripgrep's output. Currently, colors are limited to one of eight choices (red, blue, green, cyan, magenta, yellow, white and black) and text style is limited to either `nobold` or `bold`. There are otherwise three configurable aspects of ripgrep's output:
- **path** - Whenever a file path is printed.
- **line** - Whenever a line number is printed.
- **match** - Whenever text that explicitly matches the pattern is printed.

For example, this sets the match color to red and its style to bold, and set the line number background to green and not bold:

```
--colors 'match:fg:red' --colors 'match:style:bold' --colors 'line:bg:green' --colors 'line:style:nobold'
```

If you'd like to disable colors, then you can use the special `none` directive. For example, this disables any colors or styles when printing file paths:

```
--colors 'path:none'
```

Color settings are applied iteratively. For example, to clear any existing color settings for `match` and set its foreground to blue, one can do:

```
--colors 'match:none' --colors 'match:fg:blue'
```

---

While the above specification doesn't resolve _all_ use cases (e.g., "I want to set the exact ANSI escape code"), it does solve the most important ones (e.g., "I can't read the output of ripgrep") and dodges the question of platform differences by only supporting the intersection of ANSI color support and Windows console support. Most importantly, this specification leaves the door open for future expansion, such as additional text styles or supporting ANSI escape sequences directly.


---

_Assigned to @BurntSushi by @BurntSushi on 2016-11-19 15:05_

---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Comment by @CallumHoward on 2016-12-06 06:11_

Let me know if it would be better to make a new issue, but is there a way to make text font bold without using bold colors? Ag does this as seen here:  
![screenshot](http://i.imgur.com/cGxFyBO.png)

---

_Comment by @anlutro on 2016-12-06 07:30_

ANSI color codes don't support "just" bold, you have to specify which color to be bold.

---

_Comment by @BurntSushi on 2016-12-06 11:51_

@CallumHoward I don't really know how to interpret your screenshot, but you can try, for example, `--colors match:none --colors match:style:bold`. Check out the documentation for `--colors` in `rg --help`.

---

_Comment by @CallumHoward on 2016-12-06 12:54_

My apologies, I was too brief before, as the difference in bolding between top and bottom is subtle. In the above screenshot with `ag` on top and `rg` below, the green path is bolded with font, but not with color. Likewise with the line numbers. This may be more clear when with a screenshot of my ansi color pallet settings:
![screenshot](http://i.imgur.com/YiPnAhT.png)
Many terminals support bolding font independently from the bright ansi colors, as shown in the screenshot. With match:style:bold, the green path would appear almost black.
I would like to be able to bold the _font_ of the path and line numbers in `rg` to match the format of `ag` if possible.

---

_Comment by @BurntSushi on 2016-12-06 12:57_

@CallumHoward Ah I see now, thanks for the clarification! I think you're experiencing the same problem as in #266.

---

_Comment by @ilohmar on 2016-12-26 16:11_

The no-bold-escape-sequence output is not only a nuisance in terminal use, it is also a real problem for software trying to use `ripgrep` and parsing its output, e.g., when replacing `grep` by `ripgrep` in Emacs...  Are you planning to make this configurable, such that one could get actual 'bold' escape sequences?  Otherwise I will have to hack around the parsing code in Emacs, because I really like what I've seen of `ripgrep` so far!

---

_Comment by @BurntSushi on 2016-12-26 16:40_

@ilohmar Please file a new issue and describe the specific problem you're trying to solve. It would help if you left Emacs out of it completely (for example) because I don't use Emacs, and therefore I don't have the same context as you.

---

_Comment by @ilohmar on 2016-12-26 16:57_

Done, see #293.  Emacs was just an example, sorry for not making this clear enough.

---

_Comment by @jason-s on 2017-04-10 15:44_

Is there a way to do this in an environment variable or a settings file?

---

_Comment by @BurntSushi on 2017-04-10 15:46_

@jason-s No. Please see #196 to track progress. On Unix, it is recommended to use an alias or a wrapper script.

---

_Comment by @Hnasar on 2018-02-22 20:34_

For anyone coming here trying to make `rg` look exactly like `ag`, you can do this:
```
rg --colors 'match:bg:yellow' --colors 'match:fg:black' --colors 'match:style:nobold' --colors 'path:fg:green' --colors 'path:style:bold' --colors 'line:fg:yellow' --colors 'line:style:bold' 
```

---

_Comment by @BurntSushi on 2018-02-22 21:18_

@Hnasar It is also a FAQ item: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#silver-searcher-output

---
