```yaml
number: 1999
title: is it possible to transfer the reconciliation of 2 bases to CUDA NVidia?
type: issue
state: closed
author: zh76internetru
labels:
  - invalid
assignees: []
created_at: 2021-09-21T20:35:47Z
updated_at: 2021-09-22T13:27:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1999
synced_at: 2026-01-12T16:13:24Z
```

# is it possible to transfer the reconciliation of 2 bases to CUDA NVidia?

---

_@zh76internetru_

Hello.
Comparing 2 key databases consisting of 2 text files. 1 file - a template with 1 million keys, in the second several million, in which you need to find similar ones.
Note: since the files are large, you have to divide the template and the checked one into several parts, and then check each checked part with each part of the template. For this I use the bat file. In principle, it is tolerable.
About processing speed: there are 65,000 lines in a template piece. There are also 65000 lines in the piece of the checked one. Checked by the central processor in 5 seconds.
Request for speeding up the process - is it possible to transfer the reconciliation of 2 bases to CUDA NVidia?
It would be even faster.
Thanks.

---

_Comment by @BurntSushi on 2021-09-21 21:50_

I don't know what you're asking. I don't know what "is it possible to transfer the reconciliation of 2 bases to CUDA NVidia" means. Why not build a proof of concept and test it?

---

_Comment by @zh76internetru on 2021-09-22 04:30_

I'm not a programmer.
Now the processing is carried out using the central processor. I am looking for 1 million keys from a template file in other files with millions of different keys.

rg -nf 1K1.txt 221.txt>>0.txt
rg -nf 1K2.txt 221.txt>>0.txt
rg -nf 1K3.txt 221.txt>>0.txt
rg -nf 1K4.txt 221.txt>>0.txt
rg -nf 1K5.txt 221.txt>>0.txt
rg -nf 1K6.txt 221.txt>>0.txt
rg -nf 1K7.txt 221.txt>>0.txt
rg -nf 1K8.txt 221.txt>>0.txt
rg -nf 1K9.txt 221.txt>>0.txt
rg -nf 1K10.txt 221.txt>>0.txt
rg -nf 1K11.txt 221.txt>>0.txt
rg -nf 1K12.txt 221.txt>>0.txt
rg -nf 1K13.txt 221.txt>>0.txt
rg -nf 1K14.txt 221.txt>>0.txt
rg -nf 1K15.txt 221.txt>>0.txt
rg -nf 1K16.txt 221.txt>>0.txt
rg -nf 1K17.txt 221.txt>>0.txt

============

rg -nf 1K1.txt 22101.txt>>0.txt
rg -nf 1K2.txt 22101.txt>>0.txt
rg -nf 1K3.txt 22101.txt>>0.txt
rg -nf 1K4.txt 22101.txt>>0.txt
rg -nf 1K5.txt 22101.txt>>0.txt
rg -nf 1K6.txt 22101.txt>>0.txt
rg -nf 1K7.txt 22101.txt>>0.txt
rg -nf 1K8.txt 22101.txt>>0.txt
rg -nf 1K9.txt 22101.txt>>0.txt
rg -nf 1K10.txt 22101.txt>>0.txt
rg -nf 1K11.txt 22101.txt>>0.txt
rg -nf 1K12.txt 22101.txt>>0.txt
rg -nf 1K13.txt 22101.txt>>0.txt
rg -nf 1K14.txt 22101.txt>>0.txt
rg-nf 1K15.txt 22101.txt>>0.txt
rg-nf 1K16.txt 22101.txt>>0.txt
rg-nf 1K17.txt 22101.txt>>0. TXT 

and this process, is it possible to do such processing using a video card, and not a CPU?

And how would I simplify my bat file? As I knew I did, but I'm not a programmer.
![Screenshot_5](https://user-images.githubusercontent.com/91119134/134285055-e15d8b50-4506-42b5-9899-5702ed4a1f89.jpg)




---

_Comment by @BurntSushi on 2021-09-22 11:27_

> and this process, is it possible to do such processing using a video card, and not a CPU?

I don't know. Maybe. I'm not the right person to ask. Sorry.

---

_Closed by @BurntSushi on 2021-09-22 11:27_

---

_Label `invalid` added by @BurntSushi on 2021-09-22 11:27_

---

_Comment by @zh76internetru on 2021-09-22 12:11_

Why did you close the question?
In the next versions, you may be able to work on a video card?

---

_Comment by @BurntSushi on 2021-09-22 12:26_

> Why did you close the question?

Because I answered the question.

> In the next versions, you may be able to work on a video card?

Nope. I don't do GPU programming. It's too much of a niche. I can pretty safely say I'll ~never work on GPU. Maybe something in that niche will change in the years to come that will draw me in, but there's no point in tracking something that is unlikely to ever happen.

Like I said, go out and build it. If you aren't a programmer, then you might have to pay someone to do it or find someone who is specifically interested in GPU programming and regex searching.

---

_Comment by @zh76internetru on 2021-09-22 13:11_

Sorry. The program turned out to be very good, but it could have become several tens or hundreds of times even better ))) Maybe I would have bought it )))
![Screenshot_1](https://user-images.githubusercontent.com/91119134/134349626-dd0c8b28-5798-41f5-907a-fa94abefb1f8.jpg)


---

_Comment by @BurntSushi on 2021-09-22 13:27_

> but it could have become several tens or hundreds of times even better

Citation needed.

---
