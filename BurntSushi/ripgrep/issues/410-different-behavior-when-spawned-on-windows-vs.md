```yaml
number: 410
title: Different behavior when spawned on windows vs MacOS
type: issue
state: closed
author: roblourens
labels:
  - bug
assignees: []
created_at: 2017-03-16T02:49:58Z
updated_at: 2019-04-01T11:19:04Z
url: https://github.com/BurntSushi/ripgrep/issues/410
synced_at: 2026-01-12T16:13:21Z
```

# Different behavior when spawned on windows vs MacOS

---

_@roblourens_

Basically, when I spawn a ripgrep process, i.e. run it not in a TTY, on Windows, there is no output. It's probably trying to search stdin. On MacOS, it searches the `cwd`, which I think is what I'd expect.

Probably a simpler way to demonstrate the issue, but run this in node.js and observe the difference in Windows vs Mac.

```js
const cp = require('child_process');

let p = cp.spawn('rg', ['foo'], { cwd: '<some path>'})
p.stdout.on('data', data => {
    console.log('stdout: ' + data.toString());
});

p.stderr.on('data', data => {
    console.log('stderr: ' + data.toString());
});

p.on('close', code => {
    console.log('closed: ' + code);
});
```

I can easily work around it by adding `-- <path to search>` to the args list, so it's not blocking anything.

---

_Comment by @BurntSushi on 2017-03-16 11:21_

OK, so a lot of blood, sweat and tears have gone into fixing bugs like this on Windows. :-) I'll lay out the decision procedure for you on Windows. If ripgrep is called with no file paths, then it must either decide to search the CWD or to search stdin. It will search the CWD if and only if there is a console attached to stdin ([as determined by GetConsoleMode](https://github.com/softprops/atty/blob/db8d55f88eabfcc22cde39533c2b1d688d1cdfc6/src/lib.rs#L51)). I think that since you're calling it as a process, then no console is detected, and therefore ripgrep assumes something is on stdin.

Now, on Unix, there's an extra step here. Namely, even if there is no tty/console present, ripgrep will look to see [if stdin is *readable*](https://github.com/BurntSushi/ripgrep/blob/95bc678403b63d98b717636e79fdfbabc8949c8a/src/args.rs#L948), which is done by checking that stdin is either a proper file or a fifo. If stdin isn't readable, then ripgrep will search the CWD.

This extra logic on Windows doesn't exist mostly because I don't know how to do it. Paging @retep998 :-)

> I can easily work around it by adding -- <path to search> to the args list, so it's not blocking anything.

For the best results, I think you probably want to set your CWD to `<path to search>` and then use `./` as the path. (This will let you work around #278, which is probably unlikely to come up anyway.)

---

_Label `bug` added by @BurntSushi on 2017-03-16 11:21_

---

_Comment by @roblourens on 2017-03-16 14:55_

I see, thanks for the tip!

---

_Comment by @BurntSushi on 2017-10-22 01:52_

I am going to close this because it seems like ripgrep is working as intended.

---

_Closed by @BurntSushi on 2017-10-22 01:52_

---

_Comment by @BurntSushi on 2017-10-23 11:08_

I wonder if this might actually be a duplicate of #643. I wrote up my thoughts here: https://github.com/BurntSushi/ripgrep/issues/643#issuecomment-338624955

---

_Comment by @gugahoa on 2019-04-01 05:25_

I believe I'm facing this same issue, but I can't seem to work out any of the solutions so far.
This code is being run on Linux, 5.0.2:
https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=65f9f17d796fc8ed4c080109d577baf4

What could be the reason? 

---

_Comment by @BurntSushi on 2019-04-01 11:19_

@gugahoa You're not running into the same issue. Your command is incorrect. Try this instead: https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=0a8438a96ed5641f5be1c0161265689d

---
