```yaml
number: 342
title: ripgrep output is invisible in PowerShell
type: issue
state: closed
author: nzbart
labels:
  - doc
assignees: []
created_at: 2017-01-24T18:15:57Z
updated_at: 2017-07-17T22:01:16Z
url: https://github.com/BurntSushi/ripgrep/issues/342
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep output is invisible in PowerShell

---

_@nzbart_

When I launch cmd.exe and run PowerShell from within that console window, the output is perfectly legible:

![image](https://cloud.githubusercontent.com/assets/1592130/22260190/287ee1de-e2cd-11e6-8edd-7a36cc123aa5.png)

However, if I launch PowerShell and run rg within the default window, it appears that ripgrep renders the file name in blue rather than magenta, making it invisible:

![image](https://cloud.githubusercontent.com/assets/1592130/22260289/8c94631a-e2cd-11e6-9094-acc822e6c123.png)

When I tried setting my colour scheme using the PowerShell console configuration window (Alt-Space, P from within the console) so that it matched the cmd.exe settings, I noticed that ripgrep was outputting blue text.

Why would ripgrep change the output colour of the file name in some cases, particularly when the background colour is blue, and is it possible to make this algorithm a bit smarter?

---

_Comment by @BurntSushi on 2017-01-24 18:22_

These are the default styles used by ripgrep in all circumstances: https://github.com/BurntSushi/ripgrep/blob/f5a2d022ecd16dfe67ea829267e9220136f20167/src/args.rs#L706-L711

```rust
        let mut specs = vec![
            "path:fg:magenta".parse().unwrap(),
            "line:fg:green".parse().unwrap(),
            "match:fg:red".parse().unwrap(),
            "match:style:bold".parse().unwrap(),
        ];
```

In other words, ripgrep isn't changing anything on you. I can't really explain what you're observing though. Maybe the blend of colors makes it invisible? Maybe Powershell has a more limited color palette and can't display magenta? I don't know.

You might consider trying to experiment with the color settings, which are configurable on the command line:

```
        --colors <SPEC>...
            This flag specifies color settings for use in the output. This flag may be provided
            multiple times. Settings are applied iteratively. Colors are limited to one of eight
            choices: red, blue, green, cyan, magenta, yellow, white and black. Styles are limited
            to nobold, bold, nointense or intense.
            
            The format of the flag is {type}:{attribute}:{value}. {type} should be one of path,
            line or match. {attribute} can be fg, bg or style. {value} is either a color (for fg
            and bg) or a text style. A special format, {type}:none, will clear all color settings
            for {type}.
            
            For example, the following command will change the match color to magenta and the
            background color for line numbers to yellow:
            
            rg --colors 'match:fg:magenta' --colors 'line:bg:yellow' foo.
```

> and is it possible to make this algorithm a bit smarter?

I would rather have a single set of defaults with no smarts. If those don't work for whatever reason, then I'd like to encourage users to set their own colors with the `--colors` flag.

---

_Comment by @nzbart on 2017-01-24 18:55_

I'm not sure why the colour changes between the consoles, but this is the solution I'm using to work around it thanks to your suggestion.

![image](https://cloud.githubusercontent.com/assets/1592130/22261820/469ed600-e2d3-11e6-8c3d-67d048fce48d.png)

For anyone else who runs into this issue, this should fix it:

```powershell
Add-Content $PROFILE -Encoding UTF8 "`r`nfunction rg { rg.exe --colors 'path:bg:white' `$args }`r`n"
```

---

_Comment by @BurntSushi on 2017-01-24 18:56_

@nzbart Oh cool! Could you explain your `Add-Content` command in more details for folks that aren't familiar with Powershell? I'd love to add something like to the docs.

---

_Comment by @nzbart on 2017-01-24 19:08_

That command line adds the following to the current user's PowerShell profile, whose path is stored in the variable `$PROFILE`:
```powershell
function rg { rg.exe --colors 'path:bg:white' $args }
```

Unfortunately, it may not be 100% reliable in all cases depending on how people have configured their profile, so I'd be a bit reluctant to promote it to the official docs.

Something that might be worth adding to the docs is that the simplest way to install ripgrep on Windows may be the Chocolatey package that @dstcruz has kindly created:
* [ripgrep package on Chocolatey.org](https://chocolatey.org/packages/ripgrep)

Chocolatey is an increasingly popular way of installing software from the command-line. For example to install ripgrep:
```powershell
choco install ripgrep
```



---

_Comment by @BurntSushi on 2017-01-24 19:18_

@nzbart Awesome, thanks for the info! I'll try to improve the docs soon with your helpful feedback. :-)

---

_Label `doc` added by @BurntSushi on 2017-01-24 19:19_

---

_Renamed from "File name not readable in default PowerShell colour scheme" to "improve tips for Windows users in docs" by @BurntSushi on 2017-01-24 19:19_

---

_Comment by @DoumanAsh on 2017-02-11 12:34_

@nzbart Note that when you use such alias (through function) rg will not work through pipe
i.e. `rg -h | rg hidden` will not search output of `rg -h` but instead will regularly search files in directory.

P.s. the problem is that when you try to pipe output to such function. It will not pipe output to `rg.exe`

---

_Comment by @BurntSushi on 2017-02-11 13:35_

@DoumanAsh is there anyway to work around that and write a functional alias on Windows?

---

_Comment by @DoumanAsh on 2017-02-11 14:03_

It took a bit of re-search but so far i found simple solution:
```powershell
function grep {
    $input | rg.exe --hidden $args
}
```

$input goes obviously for piping, and it works just fine even if you would have no pipe.

Reference to powershell builtin variables https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.core/about/about_automatic_variables

---

_Comment by @nzbart on 2017-02-11 17:25_

@DoumanAsh nice improvement, thanks!

---

_Renamed from "improve tips for Windows users in docs" to "Improve tips for Windows users in docs" by @nzbart on 2017-02-28 22:12_

---

_Comment by @nzbart on 2017-03-06 02:06_

@DoumanAsh I've found that the function above doesn't seem to work unless piping into it (Windows 10):
```powershell
> gcm rg | select -expand definition

    $input | rg.exe --colors 'path:bg:white' $args

> set-content -Path test.txt -Value "Something" -Encoding UTF8
> rg thing
> rg.exe thing
test.txt
1:﻿Something
> ls | cat | rg thing
1:Something
```

Does the test scenario above work for you (run steps in a clean directory)?

---

_Comment by @DoumanAsh on 2017-03-06 05:19_

It works for me:

```
08:15:21 > set-content -Path test.txt -Value "Something" -Encoding UTF8
D:\repos\test
08:15:21 > rg thing
test.txt
1:﻿Something
```

Maybe it depends on version of powershell?
Which do you use?
Try something above version 2.

You can do it by starting powershell with option `-version <num>`

I tested on version from 3 to 5 and it works. Cannot go below because my profile wouldn't load :D

---

_Comment by @nzbart on 2017-03-06 22:07_

@DoumanAsh That is strange. Could you please confirm that `rg` in your test above is calling the function and not `rg.exe` directly? One way to do this is to run `gcm rg | select -expand definition`.

This is my PowerShell version:
```powershell
> $PSVersionTable.PSVersion.ToString()
5.1.14393.693
```

---

_Comment by @DoumanAsh on 2017-03-07 04:54_

Opps... you're right, i forgot that i aliased it to `grep`
Yes, it doesn't work like that. I wonder why i didn't notice it before :(

---

_Comment by @DoumanAsh on 2017-03-07 06:02_

Ok, i suspect `$input` has something even when stdin is empty and therefore `rg` is trying to read from there regardless.

---

_Comment by @DoumanAsh on 2017-03-07 06:27_

Ok, i found work-arond:
```powershell
function grep {
    $count = @($input).Count
    $input.Reset()

    if ($count) {
        $input | rg.exe --hidden $args
    }
    else {
        rg.exe --hidden $args
    }
}
```

Basically i convert $input to Array and count number of elements.
And then i reset it back and depending wether there is something inside $input i pipe it to rg

---

_Comment by @nzbart on 2017-03-30 19:31_

Unfortunately, it's just much simpler to go back to the Silver Searcher than it is to have to work around this issue on every Windows machine I work on. Below is a comparison of these two tools when run in in the default PowerShell window on a clean Windows 10 install:

![image](https://cloud.githubusercontent.com/assets/1592130/24522125/0ae469cc-15eb-11e7-9abe-e4a52299e88c.png)

As stated in my original post, it does work when starting the default cmd.exe console rather than PowerShell:

![image](https://cloud.githubusercontent.com/assets/1592130/24522234/57f72d44-15eb-11e7-8e28-ee5788096525.png)

Hopefully, the out-of-the-box experience will be improved in a future version for Windows PowerShell users.

As an additional curiosity, I'm unsure why the UTF8 BOM is rendered differently by ripgrep between the two consoles.

---

_Comment by @BurntSushi on 2017-03-30 19:59_

@nzbart I don't understand how ripgrep can work on all possible terminal color schemes out of the box. I think what you're saying is, "ag happens to pick a default color that works in my color scheme"?

---

_Comment by @BurntSushi on 2017-03-30 19:59_

@nzbart Would #196 solve it? I guess I don't understand why your work-around is so annoying.

---

_Comment by @elirnm on 2017-03-31 00:10_

I think the underlying issue here is not that the ripgrep color scheme doesn't match the Powershell scheme, but that the colorization isn't always working properly on Powershell.

`"path:bg:yellow"` renders with a white background, but not the same white as `"path:bg:white"`. See pictures below.

`"path:bg:magenta:"` renders with the exact same background color as the default background. This is a blue color, but not the same blue that's produced by `"path:bg:blue"`.

`"path:bg:green"`, `"path:bg:red"`, `"path:bg:blue"`, `"path:bg:cyan"`, `"path:bg:white"`, and `"path:bg:black"`all render correctly.

The same thing happens with `"path:fg:whatever"`.

Note that this seems to only happen with the powershell.exe executable itself. If I open cmd.exe and invoke powershell within that window, everything works fine.

These were all tested with Powershell 5.0.10586.117 (note: not the most recent version) on Windows 7 with the default Powershell color scheme.

`"path:bg:yellow"`:

![bg-yellow](https://cloud.githubusercontent.com/assets/19817004/24530998/9ce9d446-156a-11e7-86e0-1e866412c131.PNG)

`"path:bg:white"`:

![bg-white](https://cloud.githubusercontent.com/assets/19817004/24531006/a3dc21f0-156a-11e7-8aea-f72fd8beb9da.PNG)

`"path:bg:magenta"` (with the background color of Powershell changed to make ripgrep's colorization visible, and `"path:fg:white"` set to make the foreground text visible):

![gb-magenta](https://cloud.githubusercontent.com/assets/19817004/24531028/cd864e86-156a-11e7-95e2-1e9627f9c691.PNG)

`"path:bg:magenta"` (in the default Powershell color scheme, with `"path:fg:white"` set to make the text readable. The background color is completely invisible.

![bg_magenta-default](https://cloud.githubusercontent.com/assets/19817004/24531304/c3a02bce-156c-11e7-9a44-8cd7b43f59ad.PNG)

`"path:bg:blue"` (in the default Powershell color scheme, with `"path:fg:white"` set to make the text readable). Note that this is not the same blue as `"path:bg:magenta"` gets, and it's (somewhat) readable even on the default color scheme.

![bg-blue](https://cloud.githubusercontent.com/assets/19817004/24531210/148393e2-156c-11e7-8ab9-08638e15046f.PNG)


---

_Comment by @BurntSushi on 2017-03-31 00:20_

@elirnm If you toggle between `--colors 'path:style:intense'` and `--colors 'path:style:nointense'`, does that help at all?

---

_Comment by @elirnm on 2017-03-31 00:25_

`--colors "path:bg:yellow" --colors "path:style:intense" --colors "path:fg:black"`

![bg-yellow-intense](https://cloud.githubusercontent.com/assets/19817004/24531419/da8ebfca-156d-11e7-8c5f-ce597dabf77d.PNG)

`--colors "path:bg:magenta" --colors "path:style:intense" --colors "path:fg:white"`

![bg-intense-magenta](https://cloud.githubusercontent.com/assets/19817004/24531424/e382ab14-156d-11e7-8e8a-c65482bf9159.PNG)

---

_Comment by @BurntSushi on 2017-03-31 00:27_

Interesting. So I can't really tell if there's an actual bug in the communication with the Windows console. It seems like not? Maybe ripgrep should pick a default color scheme on Windows that fits the default color scheme of PowerShell and call it a day?

---

_Comment by @elirnm on 2017-03-31 01:12_

It seems like it might be a bug in powershell.exe, actually, or powershell.exe has a weird idea of colors, because this is happening natively as well.

In powershell.exe:

![write-ps](https://cloud.githubusercontent.com/assets/19817004/24532314/67b3672e-1574-11e7-8ac1-55349933e858.PNG)

In a Powershell session invoked from cmd.exe:

![write-cmd](https://cloud.githubusercontent.com/assets/19817004/24532319/7538e572-1574-11e7-8793-7ca1d59c02fe.PNG)

---

_Comment by @elirnm on 2017-03-31 01:26_

I don't know much about console coloring, but https://github.com/PowerShell/PowerShell/issues/2381 might be relevant. Also https://stackoverflow.com/questions/20541456/list-of-all-colors-available-for-powershell

---

_Comment by @DoumanAsh on 2017-03-31 04:54_

I'm using black color sceheme in powershell and it looks fine to me.
It is a bit dark but overall ripgrep's default color scheme is fine.

---

_Comment by @elirnm on 2017-03-31 05:11_

@DoumanAsh By "looks fine" do you mean it's readable or do you mean the colors are actually correct? If you execute `Write-Host "blah" -ForegroundColor DarkYellow` is the output actually dark yellow or is it white? Does `Write-Host "blah" -ForegroundColor DarkMagenta` produce dark magenta or a dark blue? Are the file paths that ripgrep prints out in magenta or dark blue?

It should obviously be perfectly readable on any shell where the background color doesn't conflict with ripgrep's colors.

---

_Comment by @DoumanAsh on 2017-03-31 05:22_

Here is example of my look: http://i.imgur.com/gXU5l00.png
To me it seems to match `-ForegroundColor`

`-ForegroundColor DarkYellow` is actual dark yellow and the same for Magenta and Red

---

_Comment by @elirnm on 2017-03-31 05:23_

What version of Powershell do you have? `$PSVersionTable`

---

_Comment by @DoumanAsh on 2017-03-31 05:24_

```
> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.14393.0
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.14393.0
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

I'm using latest PS

---

_Comment by @elirnm on 2017-03-31 06:04_

I just updated to Powershell version 5.1.14409.1005 and it still doesn't work correctly for me. Both magenta and yellow print the wrong colors. Maybe something with different Windows versions? I don't know.

---

_Comment by @nzbart on 2017-03-31 07:21_

@elirnm I think you might have hit upon the problem in your comment above (https://github.com/BurntSushi/ripgrep/issues/342#issuecomment-290588540) - well diagnosed!

@BurntSushi you asked above (https://github.com/BurntSushi/ripgrep/issues/342#issuecomment-290527465) whether this was a big problem. I prefer ripgrep over Silver Searcher, but they are both great tools, and one of them works out of the box on multiple machines without having to come up with unsatisfactory workarounds. Each of the PowerShell workarounds we tried above have some kind of deficiency that makes it annoying to use, along with the fact that I work on multiple machines that need the workaround applied instead of just running `choco install ag` or `choco install ripgrep` to get up and going.

I still use ripgrep in Vim via [Grepper](https://github.com/mhinz/vim-grepper) because console colour isn't a problem in this case, and because ripgrep has some very useful features such as understanding .gitignore files and having the `--smart-case` option.

---

_Comment by @latkin on 2017-07-13 23:44_

This is because the default shortcuts for Windows Powershell use a customization of the standard console color palette, redefining "DarkMagenta" to be RGB(1,36,86) and "DarkYellow" to be RGB(238,237,240), then setting the background color as "DarkMagenta."  This was done in order to give powershell a custom, distinctive blue color, see http://www.leeholmes.com/blog/2008/06/01/powershells-noble-blue/.

They overwrote dark magenta because it was judged to be rarely used (speculation). Dark yellow was updated because it didn't have enough contrast against the new blue background.

You can see the 2 altered colors when you compare options from Powershell and Cmd:

![image](https://user-images.githubusercontent.com/5943573/28192119-04f7d2ec-67e9-11e7-9482-27b16e086567.png)

The overriding of the RGB values is done via the registry:

![image](https://user-images.githubusercontent.com/5943573/28192178-4980c3f6-67e9-11e7-917c-61db861f16c6.png)

Upshot is that any tools that write with DarkMagenta foreground are gonna be invisible in standard powershell consoles. IMO this was a total hack from Microsoft, but at this point it's spilled milk.

I think one possible workaround is to check if rg is 1. running on windows and 2. current console background color is dark magenta. That's a very strong indication that you are within a powershell console, and regardless file names will be invisible (either magenta/magenta or "noble blue"/"noble blue"). In this case output file names with intense magenta (which will still look good).

---

_Renamed from "Improve tips for Windows users in docs" to "ripgrep output is invisible in PowerShell" by @BurntSushi on 2017-07-15 16:19_

---

_Comment by @BurntSushi on 2017-07-15 16:20_

I am open to any easily maintainable hack that fixes this. Even something like, "change the default colors on Windows regardless of shell" would be OK with me I think.

---

_Comment by @latkin on 2017-07-16 05:25_

> one possible workaround is to check if rg is 1. running on windows and 2. current console background color is dark magenta. ... In this case output file names with intense magenta (which will still look good).

I have this implemented (though I'm new to rust, so hacks, surely). Should I send a PR?

![image](https://user-images.githubusercontent.com/5943573/28244875-3634777e-69ac-11e7-8b14-ebf2254d62f5.png)



---

_Comment by @BurntSushi on 2017-07-16 13:50_

@latkin Sure! I can help you get it into a non-hacky state. :-)

---

_Closed by @BurntSushi on 2017-07-17 22:01_

---
