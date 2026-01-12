```yaml
number: 1965
title: Cannot be used with pipe character at the same time
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2021-08-05T09:49:57Z
updated_at: 2021-08-05T14:31:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1965
synced_at: 2026-01-12T16:13:24Z
```

# Cannot be used with pipe character at the same time

---

_@ghost_

#### What version of ripgrep are you using?

Ripgrep Version：12.1.1-1+b1

#### How did you install ripgrep?

apt install ripgrep -y

#### What operating system are you using 

Kali Linux 5.4.0-faked

#### Describe your bug.

I used the combined command of the pipe character

Like this：apt list | rg 'rust'

But the output of the command is not executed at the same time as Ripgrep, but the pipe character and grep can

#### What are the steps to reproduce the behavior?

apt list | rg'rust'

#### What is the actual behavior?


DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/grep-regex-0.1.8/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(rust)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.bash_profile: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.cache: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.bashrc: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.profile: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.viminfo: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.config: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.selected_editor: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.hashcat: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./.gitconfig: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/ignore-0.4.16/src/walk.rs:1730: ignoring ./Blasting_dictionary/.git: Ignore(IgnoreMatch(Hidden))

illustrate：
![Screenshot_20210805_174744](https://user-images.githubusercontent.com/83487691/128330324-7bb498e8-cc95-48e1-87f7-c4ea6103b347.jpg)
![Screenshot_20210805_174731](https://user-images.githubusercontent.com/83487691/128330343-7ef19e0b-8452-4c61-91e6-bfc07b0f6da8.jpg)



---

_Comment by @BurntSushi on 2021-08-05 11:59_

Please try ripgrep 13. As a work-around, you may use `apt list | rg 'rust' -` to explicitly search stdin.

From what I can tell in your screenshots, the issue is that ripgrep is not detecting stdin as readable. grep doesn't have this problem because grep requires you to provide a file path or directory to search, and it will otherwise always search stdin. This is the only way to make something like `rg foo` do what you expect in the current directory.

There was a fix related to this included in ripgrep 13, so it's important that you try that. If ripgrep 13 exhibits the same behavior, then the only way forward is to either find a way for me to reproduce it, or for you to report more details about your environment. e.g., Are you using anything "weird"? Which shell are you using?

---

_Comment by @ghost on 2021-08-05 14:31_

> 请尝试 ripgrep 13。作为一种变通方法，您可以使用`apt list | rg 'rust' -`显式搜索标准输入。
> 
> 从我在你的截图中可以看出，问题是 ripgrep 没有检测到标准输入为可读的。grep 没有这个问题，因为 grep 要求您提供要搜索的文件路径或目录，否则它将始终搜索 stdin。这是`rg foo`在当前目录中执行您期望的操作的唯一方法。
> 
> ripgrep 13 中包含了一个与此相关的修复程序，因此尝试该修复程序很重要。如果 ripgrep 13 表现出相同的行为，那么前进的唯一方法是找到一种方法让我重现它，或者让您报告有关您的环境的更多详细信息。例如，您使用“奇怪”的东西吗？你用的是哪个壳？

Thank you very much, I fixed this problem in compiling Ripgrep 13, and I have to complain, Kali Linux package update speed is too slow 😅

---

_Closed by @ghost on 2021-08-05 14:31_

---
