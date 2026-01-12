```yaml
number: 362
title: Compiled regex exceeds size limit of 10485760 bytes.
type: issue
state: closed
author: GenomicaMicrob
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-02-13T19:43:51Z
updated_at: 2024-08-07T12:48:07Z
url: https://github.com/BurntSushi/ripgrep/issues/362
synced_at: 2026-01-12T16:13:21Z
```

# Compiled regex exceeds size limit of 10485760 bytes.

---

_@GenomicaMicrob_

Hi, I am trying to use a file with more then 8,000 entries (10-20 letter words, one per line. 132K) and get their corresponding lines in a big file (645,151lines, 76M). I use:
`rg -w -f query_file target_file`

I get the error: `Compiled regex exceeds size limit of 10485760 bytes.`
How can I configure it to allow rg to run without the limit?

---

_Comment by @BurntSushi on 2017-02-13 19:47_

You can't, as of now, without re-compiling and hard-coding a new limit.

I would be interested to hear how well 8,000 entries performs. As of now, it is fed into a single regex as a single alternation. If they are all plain text strings and not patterns, then it might be better to use Aho-Corasick instead (which ripgrep should do, it just doesn't yet).

---

_Label `enhancement` added by @BurntSushi on 2017-02-13 19:48_

---

_Comment by @BurntSushi on 2017-02-13 19:49_

If you were so inclined, the limit is here: https://github.com/BurntSushi/ripgrep/blob/master/grep/src/search.rs#L67

FYI, you can't "remove" the limit (but you can set it arbitrarily high).

---

_Comment by @GenomicaMicrob on 2017-02-13 20:17_

Thanks, I have run it with a 640-line file (11K) without problems and very fast. The lines of the file are like these:
```
AACY020304192.1.1216
AACY020311045.179.1696
AACY020524983.300.1803
AACY020558197.3281.4799
AACY023705044.289.1515
AACY023835985.1.1221
```

I am fairly new to coding, so excuse me but I need a "for dummies" howto set the limit very high. Basically, what should I do and where?
I have a server with 64 cores, 128 Mb RAM.
sorry

---

_Comment by @BurntSushi on 2017-02-13 20:21_

The limit probably *should* be exposed as a flag so that it's a knob you can turn easily. That feature is not currently available, so the only way for you to do it is change the source code of ripgrep and recompile it. Briefly:

```
$ git clone git://github.com/BurntSushi/ripgrep
$ cd ripgrep
... try to compile it to make sure your env is setup right
$ cargo build --release
... change the limit in the source code
$ $EDITOR grep/src/search.rs
... compile again
$ cargo build --release
... the ripgrep binary is in target/release
$ ./target/release/rg blah blah blah
```

In order for the above to work, you will need to install Rust. See: https://rustup.rs/

---

_Comment by @GenomicaMicrob on 2017-02-16 19:58_

OK, solved, thanks.
I ran a file with 11,724 lines in under 1:20 h, not bad compared to several hours with grep.

I changed the "size_limit" from 10 to 1000:

```
 impl Default for Options {
     fn default() -> Options {
         Options {
             case_insensitive: false,
             case_smart: false,
             line_terminator: b'\n',
             size_limit: 1000 * (1 << 20),
             dfa_size_limit: 10 * (1 << 20),
         }
     }
 }
```

---

_Closed by @GenomicaMicrob on 2017-02-16 19:58_

---

_Comment by @GenomicaMicrob on 2017-02-16 19:58_

Cheers

---

_Comment by @BurntSushi on 2017-02-16 20:03_

Can you try increasing the dfa limit as well? Might speed things up too

On Feb 16, 2017 2:58 PM, "Microbial Genomics Lab" <notifications@github.com>
wrote:

> Cheers
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/362#issuecomment-280441247>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34qqW670PqgAxZQNhO7lrXV_VA0b6ks5rdKpggaJpZM4L_ose>
> .
>


---

_Comment by @BurntSushi on 2017-02-17 12:14_

I'm going to re-open this because I think this limit should be configurable without re-compiling ripgrep.

---

_Reopened by @BurntSushi on 2017-02-17 12:14_

---

_Comment by @GenomicaMicrob on 2017-02-17 14:08_

Nice! Down to seconds now!

---

_Comment by @BurntSushi on 2017-02-17 14:30_

Wow. My guess is that you were previously exhausting the cache space of the DFA, so it probably did a lot of thrashing or dropped down to one of the (much) slower NFA engines.

---

_Label `help wanted` added by @BurntSushi on 2017-03-12 23:52_

---

_Closed by @BurntSushi on 2017-04-12 22:14_

---

_Comment by @victoriastuart on 2018-01-06 20:22_

Just a comment (I have not tried resetting the limit, as detailed above): I ran into this exact issue today, comparing a ~15K list (individual words on separate lines) to another file (~272K; ditto).  Large, I know, but grep trounced that task, whereas ripgrep failed:

```
time rg -Fxv -f my_stopwords.txt my_list | sort | uniq > \
~/projects/nlp/tfidf/output/kw_not_sw

  Compiled regex exceeds size limit of 10485760 bytes.                            
  Command exited with non-zero status 1                                           
  0:01.58                                                                         
 
time grep -Fxv -f my_stopwords.txt my_list | sort | uniq > \
~/projects/nlp/tfidf/output/kw_not_sw

  0:00.14
```
Arch Linux x84_64; 32MB RAM + swap + tmpfs; ripgrep v.0.7.1; grep (GNU grep) v.3.1
  

---

_Comment by @BurntSushi on 2018-01-06 20:35_

@victoriastuart Increase the size limit using `--regex-size-limit`.

---

_Comment by @victoriastuart on 2018-01-07 19:08_

Hi Andrew; thank you for the project/code, comment -- appreciated.  Love ripgrep (v. fast, generally)!  :-D

Some observations (just a FYI; I'm happy with using grep, here):

```
$ time rg --regex-size-limit 100M -Fxv -f  \
/mnt/Vancouver/Programming/data/nlp/stopwords/my_stopwords.txt  \
/mnt/Vancouver/untitled | sort | uniq > ~/projects/nlp/tfidf/output/kw_not_sw

Compiled regex exceeds size limit of 104857600 bytes.
Command exited with non-zero status 1
0:01.55

$ time rg --regex-size-limit 200M -Fxv -f  \
/mnt/Vancouver/Programming/data/nlp/stopwords/my_stopwords.txt  \
/mnt/Vancouver/untitled | sort | uniq > ~/projects/nlp/tfidf/output/kw_not_sw

^C          ## manually terminated
Command terminated by signal 2
21:07.17    ## 21 min

$ time grep -Fxv -f  \
/mnt/Vancouver/Programming/data/nlp/stopwords/my_stopwords.txt  \
/mnt/Vancouver/untitled | sort | uniq > ~/projects/nlp/tfidf/output/kw_not_sw

0:00.13    ## ~0.1 sec

Regarding the ripgrep experiments:

* Intel Core i7-4790 CPU @ 3.60 GHz x 4 cores (hyper-threaded to 8 threads):
  * only 1 of 4 CPU/threads used @100% at any one time (rest ~@10-20%;
    includes other background processes, I presume).
* Memory (RAM; swap) usage unchanged throughout.
* Noted:
  * https://github.com/tiehuis/ripgrep/commit/593895566512f45b4dbdec78de2c46a7a91e8e18
  * The default limit is 10M. This should only be changed on very large regex
    inputs where the (slower) fallback regex engine may otherwise be used
```
  
  

---

_Comment by @BurntSushi on 2018-01-07 19:15_

You need to increase --dfa-size-limit too.

---

_Comment by @victoriastuart on 2018-01-07 20:02_

Noting https://github.com/BurntSushi/ripgrep/issues/497 ,

```
time rg --regex-size-limit 200M --dfa-size-limit 10G -Fxv -f  \
/mnt/Vancouver/Programming/data/nlp/stopwords/my_stopwords.txt  \
/mnt/Vancouver/untitled > ~/projects/nlp/tfidf/output/kw_not_sw

^C Command terminated by signal 2
33:58.19    ## manually-terminated at t ~34 min
```

---

_Comment by @BurntSushi on 2018-01-07 20:09_

@victoriastuart thanks! Is possible can you share the data you are searching and your regex queries as well? Or tell me how to get it? It reproduce it publicly available data?

---

_Comment by @victoriastuart on 2018-01-07 20:28_

Hi ... it's my own data (private), but simply lists of words; e.g.:

* my_stopwords.txt (currently 272K entries):
```
#wed
'd
'll
'm
're
's
't
'tis
'twas
've
**
+summary
--
-141
-2
-200c
-a
-casts
-determination
-immunocompatible
-induced
-researchers
-resistant
-sensitive
-β
...
.b
/diaphanous
/pax7
0-10
0-μm
0.02
0.2
0.5
000
000-fold
00000002-0030
00000004-0010
[ ... SNIP!! ... ]
zzzxpster
zörnig
zünd
zürich
~130
~130,000
²-actin
µm
ß
ángel
école
òscar
öaw
ûlo
α
α-helical
αβ
β
β-sandwich
β-sheets
β-subunit
β1
β2
β3
γ-carboxyl
μm
−141
```

* untitled (tmp file, generated from a custom tf-idf search script that I wrote; one of several methods I'm using -- along with RAKE; TextRank; others; ...):

```
`['cell', 'gene', 'rna', 'cancer', 'protein', 'disease', 'dna', 'mouse', 'human', 'tumor', 'expression', 'genetic', 'function', 'mrna', 'tissue', 'brain', 'memory', 'genome', 'mirna', 'mechanism', 'molecular', 'stem', 'rnai', 'mutation', 'pathway', 'breast', 'blood', 'micrornas', 'cellular', 'lncrna', ...]
````

formatted for processing (~15K lines, this particular output):

```
cell
gene
rna
cancer
protein
disease
dna
mouse
human
tumor
expression
genetic
function
mrna
tissue
brain
memory
genome
mirna
mechanism
molecular
stem
rnai
mutation
pathway
breast
blood
micrornas
cellular
lncrna
[ ... SNIP!! ... ]
free-energy
bldg
edn
br
lookup
sciencemag
453-466
ac9erisnzlnjed
aczdv
csb28z6ji
g7bq
q8uv1gph4msxsoqw8ar
xad
zooew
tt
phylogenet
```
  

---

_Comment by @garry-ut99 on 2024-08-07 09:49_

When trying to do what described in:
- https://github.com/Genivia/ugrep/issues/153#issuecomment-2272449057

It throws: `rg: compiled regex exceeds size limit of 104857600`

When trying to incerase limits as per https://github.com/BurntSushi/ripgrep/issues/362#issuecomment-355848324, using the both options, where `--regex-size-limit` is 200M, then 300M and so on, it still throws the same:
`rg: compiled regex exceeds size limit of 209715200`
`rg: compiled regex exceeds size limit of 314572800`
`rg: compiled regex exceeds size limit of 524288000`
`rg: compiled regex exceeds size limit of 943718400`

Then finally when I used: `--regex-size-limit 10G` it went out of RAM:
`memory allocation of 4294967296 bytes failed`

I have 16GB RAM, it just crashed the same way like ug, eating almost all RAM.

Concluding: all greps: grep, rg, ug eat incredibly insane ammount of RAM to incerase speed.
But rg and ug are even more RAM hungry compared to old classic grep, that they get out of RAM quickly and fail to finish their job. 
I do understand that rg and ug's algorithms are faster than classic grep's, but what is the point of faster algorithms if they just fail? In the end, ug and rg are useless compared to old classic grep on my PC, in terms of comparing big files for duplicates.

It would be good if the flawed rg and ug's RAM hungry algorithms would be fixed, I don't mean to sacrifice speed and to remove the current algorithm, the point is to simply have some alternative options to be able to finish the task without insane RAM usage, even if it will take some more time, some alternative option like "slower but less ram hungry and being able to finish the task" or maybe automatic splitting files into smaller chunks, whatever you invent to fix it.

---

_Comment by @BurntSushi on 2024-08-07 11:22_

@garry-ut99 Please open a new issue and provide an MRE. I can't help you if you can't provide a way for me to reproduce the behavior you're seeing. You don't even share the actual `rg` you're using!

---

_Comment by @garry-ut99 on 2024-08-07 12:39_

> Please open a new issue

Strange request, as you didn't tell the same to the previous person who posted similiar issue in the current thread after one year: https://github.com/BurntSushi/ripgrep/issues/362#issuecomment-355773426, hence I assumed it's ok for you to post similiar issues in the same thread, but it seems you strangely suddenly changed your mind,  I prefer to continue in the current thread until you explain/convince me: why do you want me to create a separate thread given a fact you didn't want the previous person with a similair issue to create a separate thread.

> and provide an MRE. I can't help you if you can't provide a way for me to reproduce the behavior you're seeing. 

Another strange statement, as I provided much useful informations, including description of the issue, ammount of my RAM, example content of my files, their sizes, and an exact command line used to compare the both files, I expected you to at least try to reproduce issue on your side, by using files with similair size and similiar content, using any binary, and then eventually to ask me for "MRE", if you still can't reproduce, but you strangely jumped directly into requiring me to provide MRE for you.

> You don't even share the actual rg you're using!

Strange, as when no version is provided, the assumption is: the latest, which means `14.1.0` currently, but I agree that I forgot to specify the binary, as I have forgotten that there is so many different binaries, so the one I used is `-x86_64-pc-windows-gnu.zip`

Many strange statements from your side.

---

_Comment by @BurntSushi on 2024-08-07 12:47_

I'll repeat one last time: please open a new issue with a [minimal reproducible example](https://stackoverflow.com/help/minimal-reproducible-example). If you want my help, that's what I require.

---

_Locked by @ghost on 2024-08-07 12:48_

---
