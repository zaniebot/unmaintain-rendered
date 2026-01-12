```yaml
number: 7589
title: pynmstar failure to compile sub package
type: issue
state: closed
author: varioustoxins
labels:
  - needs-mre
assignees: []
created_at: 2024-09-20T16:16:11Z
updated_at: 2024-12-20T12:20:12Z
url: https://github.com/astral-sh/uv/issues/7589
synced_at: 2026-01-12T15:59:15Z
```

# pynmstar failure to compile sub package

---

_@varioustoxins_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

HI 
if I do `uv tool install nef-pipelines` it all appears to work
However, when I run the command `nef` [the main entry point to my application in nef-pipelines], I get

`raise ImportError('Could not import cnmrstar sub-module! Your installation appears to be broken.')`

This appears to be because the dependant c file from the package pynmstar.cnmrstar doesn't appear to be compiled properly. I appear to have had similar but different problems  as the c component seems to require the latest setup tools to compile. So when using pip I have to do 

`pip install --upgrade wheel setuptools`

and sometimes

`pip install  --upgrade wheel setuptools backports.tarfile`

to get the installation to work, this is on a centos 7 system but I have had this problem elsewhere

this is with the latest uv at time of writing  0.4.13







---

_Comment by @varioustoxins on 2024-09-20 16:18_

this is also discussed here, binary wheels are provided so it shouldn't need to build the c

---

_Comment by @charliermarsh on 2024-09-20 16:30_

Can you share the output of `uv tool install` with `--verbose`?

---

_Comment by @varioustoxins on 2024-09-20 16:36_

here you go
[output.txt](https://github.com/user-attachments/files/17078117/output.txt)


---

_Comment by @charliermarsh on 2024-09-20 17:29_

So for one: `pynmrstar` only ships wheels for Python 3.9 and earlier. You're on Python 3.12, so I think it's correct that it's building from source.

---

_Comment by @charliermarsh on 2024-09-20 17:29_

See [here](https://pypi.org/project/pynmrstar/3.3.2/#files) (look at `cp39` etc.).

---

_Comment by @charliermarsh on 2024-09-20 17:30_

It seems to install ok for me:

```
❯ nef
Usage: nef [OPTIONS] COMMAND [ARGS]...

Options:
  --install-completion [bash|zsh|fish|powershell|pwsh]
                                  Install completion for the specified shell.
  --show-completion [bash|zsh|fish|powershell|pwsh]
                                  Show completion for the specified shell, to
                                  copy it or customize the installation.
  --help                          Show this message and exit.

Commands:
  chains     - carry out operations on chains
  csv        - read [rdcs]
  deep       - read deep [peaks]
  echidna    - read echidna data [peaks]
  entry      - carry out operations on the nef file entry
  fasta      - read and write fasta sequences
  fit        - carry out fitting operations [alpha]
  frames     - carry out operations on frames in nef files
  globals    - add global options to the pipeline [use save as your last...
  header     - add a header to the stream
  help       - help on the nef pipelines tools and their usage
  loops      - carry out operations on loops in nef frames
  mars       - read and write mars [shifts and sequences]
  modelfree  - write modelfree [relaxation data]
  nmrpipe    - read nmrpipe [peaks shifts & sequencess]
  nmrstar    - read NMR-STAR [sequences & shifts]
  nmrview    - read and write nmrview [peaks, sequences & shifts]
  pales      - read and write pales/dc [rdcs]
  pdbx       - read pdb [sequences]
  peaks      - carry out operations on nef peaks
  rpf        - write rpf shifts
  save       - save the entries in the stream to a file / files or stdout...
  series     - carry out operations on a data series
  shifts     - carry out operations on shifts [average]
  shiftx2    - read shiftx2 data [shifts]
  shifty     - write shifty [shifts]
  simulate   - simulate data
  sink       - read the current stream and don't write anything
  sparky     - read sparky files [shifts]
  stream     - stream a nef file
  talos      - read and write talos files [shifts & restraints]
  test       - run the test suite
  xcamshift  - write xcamshift for xplor [shifts]
  xeasy      - read xeasy files [flya dialect: sequence]
  xplor      - read xplor [sequences, dihedral & distance restraints]
```

Do you see a failure when running just `nef`? Or when you run `nef` within a project, some specific subcommand, etc.?

---

_Label `question` added by @charliermarsh on 2024-09-20 17:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 17:30_

---

_Comment by @varioustoxins on 2024-09-20 18:03_

just running it as a command as you did... it maybe a centos 7 oddity it is quite old, I have tried it on a couple of centos 7 installations, so I assume that its not just a wrinkle on a particular install

---

_Comment by @charliermarsh on 2024-09-25 21:52_

Are you able to put together a reproduction, e.g., with Docker?

---

_Label `question` removed by @charliermarsh on 2024-09-25 21:52_

---

_Label `needs-mre` added by @charliermarsh on 2024-09-25 21:52_

---

_Comment by @varioustoxins on 2024-10-08 08:48_

Hi Charlie

I should be able to, but it will take a little while as I am currently on holiday till the 24th and I will need an hour or two to sort something out.

gary

---

_Comment by @charliermarsh on 2024-10-08 21:52_

No problem, thanks for following up. Enjoy your holiday.

---

_Comment by @varioustoxins on 2024-11-26 23:25_

hi Charlie so back from holidays illness and family emergencies

here is a docker build file to make a working centos 7 system

------------------8<------------------8<------------------8<-----------------

FROM centos:7

RUN useradd -u 1000 -p test test

RUN cat /etc/yum.repos.d/CentOS-Base.repo

RUN sed -i -e 's/mirrorlist.centos.org/vault.centos.org/' /etc/yum.repos.d/CentOS-Base.repo
RUN sed -i -e 's/mirror.centos.org/vault.centos.org/' /etc/yum.repos.d/CentOS-Base.repo
RUN sed -i -e 's/#baseurl/baseurl/' /etc/yum.repos.d/CentOS-Base.repo

RUN cat /etc/yum.repos.d/CentOS-Base.repo

RUN yum -y groupinstall 'Development Tools'

RUN su test

RUN echo pwd

USER test

WORKDIR /home/test

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y

RUN curl -LsSf https://astral.sh/uv/install.sh | sh

RUN source $HOME/.local/bin/env


CMD ["/bin/bash"]

------------------8<------------------8<------------------8<-----------------

I put it in the file uv_nef_test.docker 

once run  with

docker build  -f uv_nef_test.docker --progress=plain -t uv_nef_test  .

login with 

docker run  -it uv_nef_test

and do 

uv tool install --python 3.12 nef-pipelines

this does a complete installation of nef-pipelines but when nef is run it reproduces the error with installing pynmrstar described above 

have fun
gary






---

_Comment by @varioustoxins on 2024-11-27 10:05_

I did some more digging...

with the same image if I do

```
uv python install 3.12
uv venv test
source test/bin/activate
uv pip install pynmrstar
python
```

and then 

```
import pynmrstar
import cnmrstar
```

it all seems to be fine



---

_Comment by @charliermarsh on 2024-12-19 23:42_

Ack, thanks.

---

_Comment by @charliermarsh on 2024-12-20 00:47_

If you look at the build logs:

```
DEBUG /usr/bin/aarch64-linux-gnu-gcc -fno-strict-overflow -Wsign-compare -DNDEBUG -g -O3 -Wall -fPIC -fPIC -I/home/test/.cache/uv/builds-v0/.tmpYyg39I/include -I/home/test/.local/share/uv/python/cpython-3.12.8-linux-aarch64-gnu/include/python3.12 -c c/cnmrstarmodule.c -o build/temp.linux-aarch64-cpython-312/c/cnmrstarmodule.o -funroll-loops -O3
DEBUG warning: build_ext: building extension "cnmrstar" failed: command '/usr/bin/aarch64-linux-gnu-gcc' failed: No such file or directory
```


---

_Comment by @charliermarsh on 2024-12-20 00:48_

Oh sorry, I need to emulate x86

---

_Comment by @charliermarsh on 2024-12-20 02:30_

Ok, so it looks like the issue is that `nef` ultimately installs `pynmrstar==3.3.2` due to its dependency graph. And that version is reported as failing on that operating system: https://github.com/bmrb-io/PyNMRSTAR/issues/126.

If you look at the build logs:

```
DEBUG c/cnmrstarmodule.c: In function 'str_to_lowercase':
DEBUG c/cnmrstarmodule.c:149:5: error: 'for' loop initial declarations are only allowed in C99 mode
DEBUG      for(int i = 0; orig[i]; i++){
DEBUG      ^
DEBUG c/cnmrstarmodule.c:149:5: note: use option -std=c99 or -std=gnu99 to compile your code
DEBUG warning: build_ext: building extension "cnmrstar" failed: command '/usr/bin/cc' failed with exit code 1
```

So, it seems like a somewhat irresolvable problem.

What you _can_ do is add an `overrides.txt` file:

```
pynmrstar>=3.3.3
```

And then run `uv tool install nef-pipelines --overrides overrides.txt`.

That would instruct uv to ignore any dependency declarations and use a newer version of `pynmrstar`. But other than that, I think you need to find a way for `nef` to raise its `pynmrstar` constraint.


---

_Closed by @charliermarsh on 2024-12-20 02:30_

---

_Comment by @varioustoxins on 2024-12-20 12:20_

Hi Charlie

Thanks you so much for looking at this, the explanation makes it clear whats going on. Since I develop nef-pipelines I can fix that by upgrading the dependency [In actual fact I though I had upgraded it enough to avoid this but I will dig further].

regards
Gary


Dr Gary S Thompson NMR Facility Manager
CCPN CoI & Working Group Member
Wellcome Trust Biomolecular NMR Facility
School of Biosciences,  Division of Natural Sciences
University of Kent,  Canterbury,  Kent,  England,  CT2 7NZ

☎:01227 82 7117
✉️: ***@***.***
orchid: orcid.org/0000-0001-9399-7636

On 20 Dec 2024, at 02:31, Charlie Marsh ***@***.***> wrote:


CAUTION: This email originated from outside of the organisation. Do not click links or open attachments unless you recognise the sender and know the content is safe.




Ok, so it looks like the issue is that nef ultimately installs pynmrstar==3.3.2 due to its dependency graph. And that version is reported as failing on that operating system: bmrb-io/PyNMRSTAR#126<https://github.com/bmrb-io/PyNMRSTAR/issues/126>.

If you look at the build logs:

DEBUG c/cnmrstarmodule.c: In function 'str_to_lowercase':
DEBUG c/cnmrstarmodule.c:149:5: error: 'for' loop initial declarations are only allowed in C99 mode
DEBUG      for(int i = 0; orig[i]; i++){
DEBUG      ^
DEBUG c/cnmrstarmodule.c:149:5: note: use option -std=c99 or -std=gnu99 to compile your code
DEBUG warning: build_ext: building extension "cnmrstar" failed: command '/usr/bin/cc' failed with exit code 1


So, it seems like a somewhat irresolvable problem.

What you can do is add an overrides.txt file:

pynmrstar>=3.3.3


And then run uv tool install nef-pipelines --overrides overrides.txt.

That would instruct uv to ignore any dependency declarations and use a newer version of pynmrstar. But other than that, I think you need to find a way for nef to raise its pynmrstar constraint.

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/7589#issuecomment-2556140893>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AA3UD6I4AJQW2W2GFMKFI332GN6O5AVCNFSM6AAAAABOSLNOO2VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDKNJWGE2DAOBZGM>.
You are receiving this because you authored the thread.Message ID: ***@***.***>



---
