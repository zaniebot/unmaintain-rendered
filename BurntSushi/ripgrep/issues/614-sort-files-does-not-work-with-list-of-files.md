```yaml
number: 614
title: "--sort-files does not work with list of files provided as rg input"
type: issue
state: closed
author: isker
labels: []
assignees: []
created_at: 2017-09-27T23:58:29Z
updated_at: 2017-09-28T01:07:50Z
url: https://github.com/BurntSushi/ripgrep/issues/614
synced_at: 2026-01-12T16:13:22Z
```

# --sort-files does not work with list of files provided as rg input

---

_@isker_

I want to have rg print a sorted list of files containing two different strings anywhere in the file.  I do this:
```
$ rg -l 'foo' | xargs rg -l --sort-files 'bar'
```
The resulting list of files is not sorted.  Here's a way to repro:
```
$ rg --version
ripgrep 0.6.0
-AVX -SIMD
$ mkdir /tmp/test
$ echo 'foo\nblablabla\nbar' > /tmp/test/{000..100}.txt
$ rg -l foo /tmp/test | xargs rg -l --sort-files bar
/tmp/test/000.txt
/tmp/test/001.txt
/tmp/test/002.txt
/tmp/test/003.txt
/tmp/test/004.txt
/tmp/test/005.txt
/tmp/test/006.txt
/tmp/test/007.txt
/tmp/test/008.txt
/tmp/test/009.txt
/tmp/test/010.txt
/tmp/test/011.txt
/tmp/test/012.txt
/tmp/test/014.txt
/tmp/test/015.txt
/tmp/test/016.txt
/tmp/test/017.txt
/tmp/test/018.txt
/tmp/test/013.txt
/tmp/test/019.txt
/tmp/test/020.txt
/tmp/test/021.txt
/tmp/test/022.txt
/tmp/test/023.txt
/tmp/test/024.txt
/tmp/test/025.txt
/tmp/test/026.txt
/tmp/test/027.txt
/tmp/test/028.txt
/tmp/test/029.txt
/tmp/test/030.txt
/tmp/test/031.txt
/tmp/test/032.txt
/tmp/test/033.txt
/tmp/test/034.txt
/tmp/test/035.txt
/tmp/test/036.txt
/tmp/test/037.txt
/tmp/test/038.txt
/tmp/test/039.txt
/tmp/test/040.txt
/tmp/test/041.txt
/tmp/test/042.txt
/tmp/test/043.txt
/tmp/test/044.txt
/tmp/test/045.txt
/tmp/test/046.txt
/tmp/test/047.txt
/tmp/test/048.txt
/tmp/test/049.txt
/tmp/test/050.txt
/tmp/test/051.txt
/tmp/test/052.txt
/tmp/test/053.txt
/tmp/test/054.txt
/tmp/test/055.txt
/tmp/test/056.txt
/tmp/test/057.txt
/tmp/test/058.txt
/tmp/test/059.txt
/tmp/test/060.txt
/tmp/test/061.txt
/tmp/test/062.txt
/tmp/test/063.txt
/tmp/test/064.txt
/tmp/test/065.txt
/tmp/test/066.txt
/tmp/test/067.txt
/tmp/test/068.txt
/tmp/test/069.txt
/tmp/test/070.txt
/tmp/test/071.txt
/tmp/test/072.txt
/tmp/test/073.txt
/tmp/test/074.txt
/tmp/test/075.txt
/tmp/test/076.txt
/tmp/test/077.txt
/tmp/test/078.txt
/tmp/test/079.txt
/tmp/test/080.txt
/tmp/test/081.txt
/tmp/test/082.txt
/tmp/test/083.txt
/tmp/test/084.txt
/tmp/test/085.txt
/tmp/test/086.txt
/tmp/test/087.txt
/tmp/test/088.txt
/tmp/test/089.txt
/tmp/test/090.txt
/tmp/test/091.txt
/tmp/test/092.txt
/tmp/test/094.txt
/tmp/test/096.txt
/tmp/test/097.txt
/tmp/test/098.txt
/tmp/test/099.txt
/tmp/test/100.txt
/tmp/test/093.txt
/tmp/test/095.txt
```

The resulting list seems to be almost sorted, which makes it seem like there is parallelism afoot.

Interestingly, when I pass `--sort-files` to both invocations of rg, the output is sorted.  When I pass it only to the first invocation, the output is extremely not sorted (in contrast to the almost sorted output above).

---

_Comment by @BurntSushi on 2017-09-28 00:04_

I'm on mobile, but technically the command you are running will never be
guaranteed to produce sorted output, and ripgrep can't do a damn thing
about it. Namely, xargs is free to invoke multiple distinct rg processes,
and from there the output order is anyone's guess.

Now in this case the number of arguments is small, so I would expect xargs
to just invoke rg once with all of them. But I don't actually know.

I'd suggest trying to reproduce this issue with xargs. You might also be
able to observe interesting things via xargs's -P and -n flags.

On Sep 27, 2017 7:58 PM, "CannedYerins" <notifications@github.com> wrote:

> I want to have rg print all files containing two different strings
> anywhere in the file. I do this:
>
> $ rg -l 'foo' | xargs rg -l --sort-files 'bar'
>
> The resulting list of files is not sorted. Here's a way to repro:
>
> $ rg --version
> ripgrep 0.6.0
> -AVX -SIMD
> $ mkdir /tmp/test
> $ echo 'foo\nblablabla\nbar' > /tmp/test/{000..100}.txt
> $ rg -l foo /tmp/test | xargs rg -l --sort-files bar
> /tmp/test/000.txt
> /tmp/test/001.txt
> /tmp/test/002.txt
> /tmp/test/003.txt
> /tmp/test/004.txt
> /tmp/test/005.txt
> /tmp/test/006.txt
> /tmp/test/007.txt
> /tmp/test/008.txt
> /tmp/test/009.txt
> /tmp/test/010.txt
> /tmp/test/011.txt
> /tmp/test/012.txt
> /tmp/test/014.txt
> /tmp/test/015.txt
> /tmp/test/016.txt
> /tmp/test/017.txt
> /tmp/test/018.txt
> /tmp/test/013.txt
> /tmp/test/019.txt
> /tmp/test/020.txt
> /tmp/test/021.txt
> /tmp/test/022.txt
> /tmp/test/023.txt
> /tmp/test/024.txt
> /tmp/test/025.txt
> /tmp/test/026.txt
> /tmp/test/027.txt
> /tmp/test/028.txt
> /tmp/test/029.txt
> /tmp/test/030.txt
> /tmp/test/031.txt
> /tmp/test/032.txt
> /tmp/test/033.txt
> /tmp/test/034.txt
> /tmp/test/035.txt
> /tmp/test/036.txt
> /tmp/test/037.txt
> /tmp/test/038.txt
> /tmp/test/039.txt
> /tmp/test/040.txt
> /tmp/test/041.txt
> /tmp/test/042.txt
> /tmp/test/043.txt
> /tmp/test/044.txt
> /tmp/test/045.txt
> /tmp/test/046.txt
> /tmp/test/047.txt
> /tmp/test/048.txt
> /tmp/test/049.txt
> /tmp/test/050.txt
> /tmp/test/051.txt
> /tmp/test/052.txt
> /tmp/test/053.txt
> /tmp/test/054.txt
> /tmp/test/055.txt
> /tmp/test/056.txt
> /tmp/test/057.txt
> /tmp/test/058.txt
> /tmp/test/059.txt
> /tmp/test/060.txt
> /tmp/test/061.txt
> /tmp/test/062.txt
> /tmp/test/063.txt
> /tmp/test/064.txt
> /tmp/test/065.txt
> /tmp/test/066.txt
> /tmp/test/067.txt
> /tmp/test/068.txt
> /tmp/test/069.txt
> /tmp/test/070.txt
> /tmp/test/071.txt
> /tmp/test/072.txt
> /tmp/test/073.txt
> /tmp/test/074.txt
> /tmp/test/075.txt
> /tmp/test/076.txt
> /tmp/test/077.txt
> /tmp/test/078.txt
> /tmp/test/079.txt
> /tmp/test/080.txt
> /tmp/test/081.txt
> /tmp/test/082.txt
> /tmp/test/083.txt
> /tmp/test/084.txt
> /tmp/test/085.txt
> /tmp/test/086.txt
> /tmp/test/087.txt
> /tmp/test/088.txt
> /tmp/test/089.txt
> /tmp/test/090.txt
> /tmp/test/091.txt
> /tmp/test/092.txt
> /tmp/test/094.txt
> /tmp/test/096.txt
> /tmp/test/097.txt
> /tmp/test/098.txt
> /tmp/test/099.txt
> /tmp/test/100.txt
> /tmp/test/093.txt
> /tmp/test/095.txt
>
> The resulting list seems to be almost sorted, which makes it seem like
> there is parallelism afoot.
>
> Interestingly, when I pass --sort-files to both invocations of rg, the
> output is sorted. When I pass it only to the first invocation, the output
> is extremely not sorted (in contrast to the almost sorted output above).
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/614>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oxPUiJx-HnNylggeF8tKekxvYmCks5smuEmgaJpZM4PmjTh>
> .
>


---

_Comment by @BurntSushi on 2017-09-28 00:05_

If you are just printing the list of files, then I'd recommend piping the
output to `sort`.

On Sep 27, 2017 8:04 PM, "Andrew Gallant" <jamslam@gmail.com> wrote:

> I'm on mobile, but technically the command you are running will never be
> guaranteed to produce sorted output, and ripgrep can't do a damn thing
> about it. Namely, xargs is free to invoke multiple distinct rg processes,
> and from there the output order is anyone's guess.
>
> Now in this case the number of arguments is small, so I would expect xargs
> to just invoke rg once with all of them. But I don't actually know.
>
> I'd suggest trying to reproduce this issue with xargs. You might also be
> able to observe interesting things via xargs's -P and -n flags.
>
> On Sep 27, 2017 7:58 PM, "CannedYerins" <notifications@github.com> wrote:
>
>> I want to have rg print all files containing two different strings
>> anywhere in the file. I do this:
>>
>> $ rg -l 'foo' | xargs rg -l --sort-files 'bar'
>>
>> The resulting list of files is not sorted. Here's a way to repro:
>>
>> $ rg --version
>> ripgrep 0.6.0
>> -AVX -SIMD
>> $ mkdir /tmp/test
>> $ echo 'foo\nblablabla\nbar' > /tmp/test/{000..100}.txt
>> $ rg -l foo /tmp/test | xargs rg -l --sort-files bar
>> /tmp/test/000.txt
>> /tmp/test/001.txt
>> /tmp/test/002.txt
>> /tmp/test/003.txt
>> /tmp/test/004.txt
>> /tmp/test/005.txt
>> /tmp/test/006.txt
>> /tmp/test/007.txt
>> /tmp/test/008.txt
>> /tmp/test/009.txt
>> /tmp/test/010.txt
>> /tmp/test/011.txt
>> /tmp/test/012.txt
>> /tmp/test/014.txt
>> /tmp/test/015.txt
>> /tmp/test/016.txt
>> /tmp/test/017.txt
>> /tmp/test/018.txt
>> /tmp/test/013.txt
>> /tmp/test/019.txt
>> /tmp/test/020.txt
>> /tmp/test/021.txt
>> /tmp/test/022.txt
>> /tmp/test/023.txt
>> /tmp/test/024.txt
>> /tmp/test/025.txt
>> /tmp/test/026.txt
>> /tmp/test/027.txt
>> /tmp/test/028.txt
>> /tmp/test/029.txt
>> /tmp/test/030.txt
>> /tmp/test/031.txt
>> /tmp/test/032.txt
>> /tmp/test/033.txt
>> /tmp/test/034.txt
>> /tmp/test/035.txt
>> /tmp/test/036.txt
>> /tmp/test/037.txt
>> /tmp/test/038.txt
>> /tmp/test/039.txt
>> /tmp/test/040.txt
>> /tmp/test/041.txt
>> /tmp/test/042.txt
>> /tmp/test/043.txt
>> /tmp/test/044.txt
>> /tmp/test/045.txt
>> /tmp/test/046.txt
>> /tmp/test/047.txt
>> /tmp/test/048.txt
>> /tmp/test/049.txt
>> /tmp/test/050.txt
>> /tmp/test/051.txt
>> /tmp/test/052.txt
>> /tmp/test/053.txt
>> /tmp/test/054.txt
>> /tmp/test/055.txt
>> /tmp/test/056.txt
>> /tmp/test/057.txt
>> /tmp/test/058.txt
>> /tmp/test/059.txt
>> /tmp/test/060.txt
>> /tmp/test/061.txt
>> /tmp/test/062.txt
>> /tmp/test/063.txt
>> /tmp/test/064.txt
>> /tmp/test/065.txt
>> /tmp/test/066.txt
>> /tmp/test/067.txt
>> /tmp/test/068.txt
>> /tmp/test/069.txt
>> /tmp/test/070.txt
>> /tmp/test/071.txt
>> /tmp/test/072.txt
>> /tmp/test/073.txt
>> /tmp/test/074.txt
>> /tmp/test/075.txt
>> /tmp/test/076.txt
>> /tmp/test/077.txt
>> /tmp/test/078.txt
>> /tmp/test/079.txt
>> /tmp/test/080.txt
>> /tmp/test/081.txt
>> /tmp/test/082.txt
>> /tmp/test/083.txt
>> /tmp/test/084.txt
>> /tmp/test/085.txt
>> /tmp/test/086.txt
>> /tmp/test/087.txt
>> /tmp/test/088.txt
>> /tmp/test/089.txt
>> /tmp/test/090.txt
>> /tmp/test/091.txt
>> /tmp/test/092.txt
>> /tmp/test/094.txt
>> /tmp/test/096.txt
>> /tmp/test/097.txt
>> /tmp/test/098.txt
>> /tmp/test/099.txt
>> /tmp/test/100.txt
>> /tmp/test/093.txt
>> /tmp/test/095.txt
>>
>> The resulting list seems to be almost sorted, which makes it seem like
>> there is parallelism afoot.
>>
>> Interestingly, when I pass --sort-files to both invocations of rg, the
>> output is sorted. When I pass it only to the first invocation, the output
>> is extremely not sorted (in contrast to the almost sorted output above).
>>
>> —
>> You are receiving this because you are subscribed to this thread.
>> Reply to this email directly, view it on GitHub
>> <https://github.com/BurntSushi/ripgrep/issues/614>, or mute the thread
>> <https://github.com/notifications/unsubscribe-auth/AAb34oxPUiJx-HnNylggeF8tKekxvYmCks5smuEmgaJpZM4PmjTh>
>> .
>>
>


---

_Comment by @isker on 2017-09-28 01:06_

Okay.  I had at face value assumed that xargs would only invoke more than one rg process (in non-parallel mode) if the number of arguments exceeded the argument limit on my system (which I'm pretty sure is much higher than this).  But it appears that I'm wrong, because just running `rg -l --sort-files bar /tmp/test/{000..100}.txt` results in correctly sorted output.

So like you say, it seems like this is a non-issue.  Thanks!

---

_Closed by @isker on 2017-09-28 01:06_

---

_Comment by @isker on 2017-09-28 01:07_

Right, piping to sort is a fine thing to do, but I wasn't aware that xargs would behave that way and so thought there was an actual problem with rg.

Clearly it's more than fine.  It's correct :).

---
