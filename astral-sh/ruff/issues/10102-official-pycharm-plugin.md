```yaml
number: 10102
title: Official PyCharm Plugin
type: issue
state: closed
author: Pixel-Minions
labels:
  - wish
assignees: []
created_at: 2024-02-23T17:15:34Z
updated_at: 2025-12-04T00:16:38Z
url: https://github.com/astral-sh/ruff/issues/10102
synced_at: 2026-01-10T11:09:52Z
```

# Official PyCharm Plugin

---

_Issue opened by @Pixel-Minions on 2024-02-23 17:15_

Hi,

I was wondering if there are any plans for creating plugins for Pycharm for the formatter and linter Ruff.

Most of the people I have ever worked use Pycharm as the main driver for dev and we find ourselves using the terminal within to use Ruff.

best,

---

_Comment by @MichaReiser on 2024-02-23 17:21_

Hey. 

PyCharm support is something that's on our minds and we agree that it's important. For now, you can give the unofficial pycharm plugin a try https://docs.astral.sh/ruff/integrations/#pycharm-unofficial

---

_Closed by @MichaReiser on 2024-02-23 17:21_

---

_Comment by @Pixel-Minions on 2024-02-23 17:37_

Thank you for your insights, @MichaReiser. We've been utilizing the plugin you recommended, primarily due to the lack of alternatives. However, we've encountered some challenges with it. Specifically, the plugin doesn't seem to perform linting as effectively as needed. We've noticed that certain linting issues aren't identified during its use, leading to unexpected feedback from our CI/CD pipeline. To address this, we're considering the implementation of pre-commit hooks as a means to automate and enhance the linting process.

---

_Reopened by @MichaReiser on 2024-02-23 20:01_

---

_Renamed from "Support Pycharm via creating Astral Plugins " to "Official PyCharm Plugin" by @MichaReiser on 2024-02-23 20:01_

---

_Label `wish` added by @MichaReiser on 2024-02-23 20:01_

---

_Comment by @MichaReiser on 2024-02-24 17:40_

@Pixel-Minions, have you tried creating an issue in https://github.com/koxudaxi/ruff-pycharm-plugin to see if your problem can get fixed or is there already an existing issue? 

---

_Comment by @anmolratn-qure on 2024-04-10 20:30_

PyCharm is one of the most used Python IDEs, specially in a professional setting. A lot of people at my company use PyCharm but find ourselves running ruff from the command line. I believe this should be taken as a priority. The unofficial plugin has linting bugs and sometimes work unexpectedly.

---

_Comment by @MichaReiser on 2024-04-11 06:11_

@snowsignal plans to look into this once we've rewritten our LSP in rust.

---

_Comment by @raayu83 on 2024-05-24 18:38_

Now that rust based lsp server is beta, are there any updates on these plans?

---

_Comment by @zanieb on 2024-05-24 19:07_

We are continuing to consider the best path forward here and discussing with both the JetBrains team and the author of the existing plugin. We're very interested in providing a better experience in PyCharm and we'll definitely be dedicating resources to it. I think most of the remaining uncertainty is how to use our LSP to provide the same experience in both paid and free PyCharm versions.

---

_Comment by @InSyncWithFoo on 2024-07-09 03:52_

> I think most of the remaining uncertainty is how to use our LSP to provide the same experience in both paid and free PyCharm versions.

There's [LSP4IJ](https://github.com/redhat-developer/lsp4ij). [Its support range](https://github.com/redhat-developer/lsp4ij/blob/00fc081833001099e1ca1a0866f41ec408573ff9/docs/LSPSupport.md) is pretty good. One minor issue is that it is (also) unstable.


---

_Comment by @Kaelten on 2024-08-20 20:56_

Please consider prioritizing this. 

The plugin maintained by @koxudaxi routinely get's confused about what version of ruff it should be using in a given project and most recently has started just erroring all the time and trying to use a version of ruff from a project that's not even open.

This is creating a horrible development experience and there's many of us that have over a decade of experience with Pycharm and moving to VSCode is a huge ask. 

---

_Comment by @koxudaxi on 2024-08-21 18:25_

@InSyncWithFoo @Kaelten Thank you for your feedback. I want to provide an update on our progress.

Currently, I'm focusing on LSP4IJ compatibility, which we're working on in this PR:
https://github.com/koxudaxi/ruff-pycharm-plugin/pull/437

By implementing LSP support, I aim to provide a PyCharm experience equivalent to VS Code.
I expect to release it by the end of this week.



---

_Comment by @Kaelten on 2024-08-21 23:57_

> @InSyncWithFoo @Kaelten Thank you for your feedback. I want to provide an update on our progress.
> 
> Currently, I'm focusing on LSP4IJ compatibility, which we're working on in this PR: [koxudaxi/ruff-pycharm-plugin#437](https://github.com/koxudaxi/ruff-pycharm-plugin/pull/437)
> 
> By implementing LSP support, I aim to provide a PyCharm experience equivalent to VS Code. I expect to release it by the end of this week.

It's good to see movement, but I opened a [bug report months ago](https://github.com/koxudaxi/ruff-pycharm-plugin/issues/376) about how the plugin continuously switches which install of ruff is in use and always swaps this globally across all instances of the IDE.  This leads to the wrong version and/or the wrong configuration being used and it's a bit of a nightmare.  It's been going on 7 months and there's been zero action against a pretty significant issue. 

---

_Comment by @koxudaxi on 2024-08-22 00:00_

@Kaelten 
Sorry for not responding.
Some kind people have added their comments, which I hope will help to resolve the issue.
I will pick up after the LSP response.

---

_Comment by @dhruvmanila on 2024-08-22 02:35_

@koxudaxi Thank you for chiming in! Please feel free to ping me about any questions that you might have about the VS Code extension / language server either here or on Discord. I'm more than happy to help in whatever way I can.

---

_Comment by @PinkOatmeal on 2025-02-05 10:07_

Hi! I really appreciate all the work being done on Ruff. I was wondering if there has been any progress on PyCharm support. Thanks!

---

_Comment by @olofahlen on 2025-02-21 08:17_

Adding my voice of support for this. This week I decided to try PyCharm with ruff but I wasn't able to get it working out of the box using the unofficial plugin (probably related to https://github.com/koxudaxi/ruff-pycharm-plugin/issues/531) . In VSCode, the ruff integration is great and offers features such as formatting the selected lines rather than the full file which I find very useful in my current work. Official PyCharm support would be great. Now I'm left considering switching back to Black so I can use PyCharm.

---

_Comment by @alanwilter on 2025-10-21 08:47_

Where are we on this issue? https://docs.astral.sh/ruff/integrations/#pycharm-unofficial does not exist anymore.

---

_Comment by @MichaReiser on 2025-10-21 08:49_

The "unofficial" plugin still exists, see https://plugins.jetbrains.com/plugin/20574-ruff

---

_Comment by @Otto-AA on 2025-10-21 15:17_

You can subscribe to the issue at the pycharm tracker and wait for news: https://youtrack.jetbrains.com/issue/PY-83733/ruff-support

Looks like ruff will be built into pycharm in a future release. 

---

_Comment by @koxudaxi on 2025-10-22 03:34_

I'm the author of the Ruff PyCharm Plugin, and I just learned yesterday that official support is actually moving forward. I'm truly delighted to hear this news 🎉 

In fact, I met with the PyCharm team at PyCon US in May to discuss official Ruff support. To put it simply, I requested that they add this support. I don't know if my request contributed to this decision, but I'm very happy to see that support was actually added yesterday.

I developed this plugin because I needed to use Ruff in PyCharm for my daily work. I still use this plugin every day to work with Ruff.
I was aware of the issues where the plugin doesn't work in certain environments. However, since I can only dedicate time to OSS during my personal time, I've been occupied with family commitments, maintaining other OSS tools I've created, and activities related to being a PEP 750 co-author. I apologize for not being able to resolve some of these issues.

That said, I've continued to make necessary updates many times to keep up with PyCharm version upgrades, along with bug fixes. Nevertheless, I didn't want my plugin to negatively impact the reputation of Ruff or PyCharm, which is why I made the request to the PyCharm team. Of course, I'm grateful to both Astral and JetBrains for sponsoring me over the years through GitHub Sponsors. Thanks to their support, I was able to purchase a Mac for OSS development.

It appears that the changes were already merged, but there will likely be an official announcement when it's released.

@KotlinIsland 
It would be greatly appreciated if you could leave a comment here once it's ready to be officially announced.

---

_Comment by @KotlinIsland on 2025-10-22 05:37_

i had no idea this issue existed haha

thank you @koxudaxi for maintaining the plugin for so long, i've definitely appreciated it

yes, ruff support will be available as beta soon. we will have an announcement as well

you can follow [this issue](https://youtrack.jetbrains.com/issue/PY-83733/ruff-support) and provide any feedback before the release of PyCharm 2025.3 when it will be generally available


---

_Comment by @KotlinIsland on 2025-11-05 00:28_

Ruff support in PyCharm is now available in preview: https://www.jetbrains.com/pycharm/nextversion/

if you choose to try it out, please let us know of any issues you encounter

---

_Comment by @wenmingchao on 2025-11-11 07:37_

@KotlinIsland does this native ruff support require the RyeCharm plugin?

---

_Comment by @KotlinIsland on 2025-11-11 07:38_

@wenmingchao that wouldn't be very native if it did 🙂

---

_Comment by @bryanadams-claren on 2025-11-13 16:15_

I have followed all the instructions to download the preview version and enable Ruff, and yet I'm not able to get any of the issues to appear in the editor.  It's entirely possible I'm doing something stupid, are there steps I should follow in order to see these issues in the editor?

---

_Comment by @KotlinIsland on 2025-11-13 16:45_

> I have followed all the instructions to download the preview version and enable Ruff, and yet I'm not able to get any of the issues to appear in the editor. It's entirely possible I'm doing something stupid, are there steps I should follow in order to see these issues in the editor?

@bryanadams-ph  there are no other steps, could you please raise an issue on YouTrack: https://youtrack.jetbrains.com/PY

---

_Comment by @MichaReiser on 2025-11-19 08:25_

I consider this as complete, now that PyCharm has official Ruff support.

---

_Closed by @MichaReiser on 2025-11-19 08:25_

---

_Comment by @estan on 2025-12-03 16:02_

Sorry for necroposting: Is this supposed to be available in the current 2025.3 RC? I'm using 

```
PyCharm 2025.3 RC
Build #PY-253.28294.256, built on November 28, 2025
Source revision: 5a940fc427269
Runtime version: 21.0.8+9-b1163.69 amd64 (JCEF 137.0.17)
VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
Toolkit: sun.awt.X11.XToolkit
Linux 6.8.0-88-generic
Ubuntu 24.04.3 LTS; glibc: 2.39
GC: G1 Young Generation, G1 Concurrent GC, G1 Old Generation
Memory: 2048M
Cores: 8
Registry:
  ide.experimental.ui=true
Current Desktop: KDE
```

installed via Snap, but I'm not seeing any Python | Tools | Ruff in the Settings dialog.

---

_Comment by @KotlinIsland on 2025-12-04 00:16_

@estan it's best if you comment on YouTrack. we are working on an open issue where Ruff support is not showing up, it will be addressed before the final release

https://youtrack.jetbrains.com/issue/PY-86003/New-LSP-tools-are-not-visible-in-2025.3-RC-settings-with-Python-plugin-disabled

---
