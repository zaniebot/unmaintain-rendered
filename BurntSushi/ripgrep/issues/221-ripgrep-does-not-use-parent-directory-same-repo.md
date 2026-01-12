```yaml
number: 221
title: Ripgrep does not use parent-directory, same-repo .gitignore files
type: issue
state: closed
author: srcreigh
labels: []
assignees: []
created_at: 2016-11-04T03:28:23Z
updated_at: 2016-11-04T17:25:43Z
url: https://github.com/BurntSushi/ripgrep/issues/221
synced_at: 2026-01-12T18:23:11Z
```

# Ripgrep does not use parent-directory, same-repo .gitignore files

---

_@srcreigh_

Consider the following Git repo:

```
.gitignore
foo/
foo/message  # contains "hey"
foo/bar/
foo/bar/message  # contains "hey"
```

where `.gitignore` contains:

```
/foo/bar/
```

The issue is that when I run `rg hey` while in the `foo` directory, it returns 2 files (both `foo/message` and `foo/bar/message`). This is a bug because `foo/bar/message` is ignored by git, yet it's returned by `rg`, and the whole point of using `rg` over `grep` (imo) is that it doesn't include files that are ignored by Git.

## Reproduction steps

Run these commands:
```
mkdir -p rg-test rg-test/foo rg-test/foo/bar
echo "hey" > rg-test/foo/message
echo "hey" > rg-test/foo/bar/message
echo "/foo/bar/" > rg-test/.gitignore
cd rg-test/foo
rg hey
```

Ex:
```
λ /tmp/ mkdir -p rg-test rg-test/foo rg-test/foo/bar
λ /tmp/ echo "hey" > rg-test/foo/message
λ /tmp/ echo "hey" > rg-test/foo/bar/message
λ /tmp/ echo "/foo/bar/" > rg-test/.gitignore
λ /tmp/ cd rg-test
λ /tmp/rg-test/ cd foo
λ /tmp/rg-test/foo/ rg hey
bar/message
1:hey

message
1:hey
λ /tmp/rg-test/foo/ 
```

Expected output (from the `rg` command):
```
message
1:hey
```

Actual output:
```
bar/message
1:hey

message
1:hey
```

## Screenshot
<img width="446" alt="screen shot 2016-11-03 at 11 21 04 pm" src="https://cloud.githubusercontent.com/assets/2747249/19993466/5128b6c4-a21c-11e6-84f2-b5dafe2853d9.png">

---

_Comment by @BurntSushi on 2016-11-04 03:40_

What version of ripgrep are you using?

On Nov 3, 2016 11:28 PM, "Shane Creighton-Young" notifications@github.com
wrote:

> Consider the following Git repo:
> 
> .gitignore
> foo/
> foo/message  # contains "hey"
> foo/bar/
> foo/bar/message  # contains "hey"
> 
> where .gitignore contains:
> 
> /foo/bar/
> 
> The issue is that when I run rg hey while in the foo directory, it
> returns 2 files (both foo/message and foo/bar/message). This is a bug
> because foo/bar/message is ignored by git, yet it's returned by rg, and
> the whole point of using rg over grep (imo) is that it doesn't include
> files that are ignored by Git.
> Reproduction steps
> 
> Run these commands:
> 
> mkdir -p rg-test rg-test/foo rg-test/foo/bar
> echo "hey" > rg-test/foo/message
> echo "hey" > rg-test/foo/bar/message
> echo "/foo/bar/" > rg-test/.gitignore
> cd rg-test/foo
> rg hey
> 
> Ex:
> 
> λ /tmp/ mkdir -p rg-test rg-test/foo rg-test/foo/bar
> λ /tmp/ echo "hey" > rg-test/foo/message
> λ /tmp/ echo "hey" > rg-test/foo/bar/message
> λ /tmp/ echo "/foo/bar/" > rg-test/.gitignore
> λ /tmp/ cd rg-test
> λ /tmp/rg-test/ cd foo
> λ /tmp/rg-test/foo/ rg hey
> bar/message
> 1:hey
> 
> message
> 1:hey
> λ /tmp/rg-test/foo/
> 
> Expected output (from the rg command):
> 
> message
> 1:hey
> 
> Actual output:
> 
> bar/message
> 1:hey
> 
> message
> 1:hey
> 
> Screenshot [image: screen shot 2016-11-03 at 11 21 04 pm]
> https://cloud.githubusercontent.com/assets/2747249/19993466/5128b6c4-a21c-11e6-84f2-b5dafe2853d9.png
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/221, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34j9d89ypaOvjyI5MVhOelD05qYzEks5q6qZYgaJpZM4KpKHp
> .


---

_Comment by @srcreigh on 2016-11-04 03:43_

```
λ ~/ rg --version
0.2.3
λ ~/
```

Installed via `brew install ripgrep` earlier today.


---

_Comment by @BurntSushi on 2016-11-04 03:48_

Ah okay. 0.2.6 is out, but not on brew yet (there is a binary download here
on github though). I'm on mobile, but I'll check this out on the latest
version tomorrow. Thanks for the detailed report!

On Nov 3, 2016 11:43 PM, "Shane Creighton-Young" notifications@github.com
wrote:

> λ ~/ rg --version
> 0.2.3
> λ ~/
> 
> Installed via brew install ripgrep earlier today.
> 
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/221#issuecomment-258338373,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34gQyZBWtL8ctDdEdKhkFKVXoKvRvks5q6qntgaJpZM4KpKHp
> .


---

_Comment by @BurntSushi on 2016-11-04 13:22_

Yay, looks like this has already been fixed. :-)

```
[andrew@Cheetah ripgrep] mkdir 221
[andrew@Cheetah ripgrep] cd 221/
[andrew@Cheetah 221] mkdir -p foo/bar
[andrew@Cheetah 221] echo "hey" > foo/message
[andrew@Cheetah 221] echo "hey" > foo/bar/message
[andrew@Cheetah 221] echo /foo/bar/ > .gitignore
[andrew@Cheetah 221] rg hey
foo/message
1:hey
[andrew@Cheetah 221] cd foo/
[andrew@Cheetah foo] rg hey
message
1:hey
```

The gitignore logic recently went through a major overhaul, and I may have accidentally fixed a few bugs along the way.

I'll see about submitting a PR to homebrew to update.


---

_Closed by @BurntSushi on 2016-11-04 13:22_

---

_Comment by @lyuha on 2016-11-04 15:23_

I submitted PR to https://github.com/Homebrew/homebrew-core/pull/6590, and accepted. So, latest rg is 0.2.6 at homebrew.


---

_Comment by @BurntSushi on 2016-11-04 15:28_

@lyuha Thanks! ^_^


---

_Comment by @srcreigh on 2016-11-04 17:25_

Thanks everyone. :smile: 


---
