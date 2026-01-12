```yaml
number: 3887
title: "Uv progress bars: long output in jupyterlab"
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2024-05-28T19:15:36Z
updated_at: 2024-05-29T18:17:46Z
url: https://github.com/astral-sh/uv/issues/3887
synced_at: 2026-01-12T15:58:46Z
```

# Uv progress bars: long output in jupyterlab

---

_@bluss_

Hi team, sorry for not doing this test before release, but I'm not normally working with a dev version of Uv.

Goal: Uv should work well in jupyterlab's notebook interface. It's a place where some people like to use Uv (me).

**The issue:** Uv 0.2.5's progress output is too verbose for jupyterlab, with many duplicates of lines like `⠙ Downloading packages... (0/47)` in the output, lines that would be replaced in the terminal.


Pip progress bars generally work well - they look the same way in jupyterlab as in the terminal. Uv 0.2.4's output generally works well.


Command `UV_NO_CACHE=true uv pip install --refresh -r ./requirements.lock --reinstall`  has the following outputs

Screenshot: progress bar in Uv 0.2.4 being nice inside jupyterlab.

![bild](https://github.com/astral-sh/uv/assets/3209739/d160b62c-b279-4fc0-91bc-e7ee15ed9e70)


<details>

<summary>Uv 0.2.4 text output</summary>

<pre>

Building file:///home/user/proj/py/examples/stats        
   Built file:///home/user/proj/py/examples/stats
   Built file:///home/user/proj/py/examples/stats
   Built file:///home/user/proj/py/examples/stats
   Built file:///home/user/proj/py/examples/stats                              Built 1 editable in 374ms
Resolved 47 packages in 111ms                                                
Downloaded 46 packages in 1ms.4.5                                   
Uninstalled 47 packages in 92ms
Installed 47 packages in 29ms13                                     
 - asttokens==2.4.1
 + asttokens==2.4.1
 - comm==0.2.2
 + comm==0.2.2
 - contourpy==1.2.1
 + contourpy==1.2.1
 - cycler==0.12.1
 + cycler==0.12.1
 - debugpy==1.8.1
 + debugpy==1.8.1
 - decorator==5.1.1
 + decorator==5.1.1
 - executing==2.0.1
 + executing==2.0.1
 - fonttools==4.52.1
 + fonttools==4.52.1
 - ipykernel==6.29.4
 + ipykernel==6.29.4
 - ipython==8.24.0
 + ipython==8.24.0
 - jedi==0.19.1
 + jedi==0.19.1
 - joblib==1.4.2
 + joblib==1.4.2
 - jupyter-client==8.6.2
 + jupyter-client==8.6.2
 - jupyter-core==5.7.2
 + jupyter-core==5.7.2
 - kiwisolver==1.4.5
 + kiwisolver==1.4.5
 - matplotlib==3.9.0
 + matplotlib==3.9.0
 - matplotlib-inline==0.1.7
 + matplotlib-inline==0.1.7
 - nest-asyncio==1.6.0
 + nest-asyncio==1.6.0
 - numpy==1.26.4
 + numpy==1.26.4
 - packaging==24.0
 + packaging==24.0
 - pandas==2.2.2
 + pandas==2.2.2
 - parso==0.8.4
 + parso==0.8.4
 - patsy==0.5.6
 + patsy==0.5.6
 - pexpect==4.9.0
 + pexpect==4.9.0
 - pillow==10.3.0
 + pillow==10.3.0
 - platformdirs==4.2.2
 + platformdirs==4.2.2
 - prompt-toolkit==3.0.43
 + prompt-toolkit==3.0.43
 - psutil==5.9.8
 + psutil==5.9.8
 - ptyprocess==0.7.0
 + ptyprocess==0.7.0
 - pure-eval==0.2.2
 + pure-eval==0.2.2
 - pygments==2.18.0
 + pygments==2.18.0
 - pyparsing==3.1.2
 + pyparsing==3.1.2
 - python-dateutil==2.9.0.post0
 + python-dateutil==2.9.0.post0
 - pytz==2024.1
 + pytz==2024.1
 - pyzmq==26.0.3
 + pyzmq==26.0.3
 - scikit-learn==1.5.0
 + scikit-learn==1.5.0
 - scipy==1.13.1
 + scipy==1.13.1
 - seaborn==0.13.2
 + seaborn==0.13.2
 - six==1.16.0
 + six==1.16.0
 - stack-data==0.6.3
 + stack-data==0.6.3
 - stats-notebooks==0.1.0 (from file:///home/user/proj/py/examples/stats)
 + stats-notebooks==0.1.0 (from file:///home/user/proj/py/examples/stats)
 - statsmodels==0.14.2
 + statsmodels==0.14.2
 - threadpoolctl==3.5.0
 + threadpoolctl==3.5.0
 - tornado==6.4
 + tornado==6.4
 - traitlets==5.14.3
 + traitlets==5.14.3
 - tzdata==2024.1
 + tzdata==2024.1
 - wcwidth==0.2.13
 + wcwidth==0.2.13



</pre>

</details>


<details>
<summary>Uv 0.2.5 text output (1100 lines)</summary>

<pre>

Resolved 47 packages in 440ms                                                
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
Building stats-notebooks @ file:///home/user/proj/py/examples/stats
⠙ Downloading packages... (0/47)
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 2.76 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 2.76 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 6.89 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 11.02 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 15.16 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 15.16 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 15.16 kB/12.73 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 15.16 kB/12.73 MB
scikit-learn ------------------------------     0 B/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 15.16 kB/12.73 MB
scikit-learn ------------------------------ 2.76 kB/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 16.38 kB/12.73 MB
scikit-learn ------------------------------ 2.76 kB/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 16.38 kB/12.73 MB
scikit-learn ------------------------------ 2.76 kB/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
pandas     ------------------------------ 16.38 kB/12.73 MB
scikit-learn ------------------------------ 2.76 kB/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
⠙ Downloading packages... (0/47)
tzdata     ------------------------------     0 B/345.37 kB
pandas     ------------------------------ 16.38 kB/12.73 MB
scikit-learn ------------------------------ 2.76 kB/13.07 MB
Building stats-notebooks @ file:///home/user/proj/py/examples/stats   
...
</pre>

Too long to include. Full here https://gist.github.com/bluss/ca29f5310e451ab1be9033531daf111c


</details>


It kind of doesn't matter what's in the requirements file, so it's not included.

---

_Comment by @charliermarsh on 2024-05-28 19:18_

Thanks @bluss, that's helpful. Do you see this repetition in the terminal, or just inside Jupyter? Wonder what we can do about this.

---

_Comment by @charliermarsh on 2024-05-28 19:19_

\cc @ibraheemdev 

---

_Label `bug` added by @charliermarsh on 2024-05-28 19:20_

---

_Comment by @charliermarsh on 2024-05-28 19:20_

Yeah, something about the placement is going wrong:

![Screenshot 2024-05-28 at 3 20 33 PM](https://github.com/astral-sh/uv/assets/1309177/76fef954-d9f8-49bb-8a3d-1f5a46a0b7d9)


---

_Comment by @charliermarsh on 2024-05-28 19:26_

It looks like pip uses Rich for this which supports Jupyter as a backend via a mixin or something? https://github.com/pypa/pip/blob/1904270dfedd4acfd82344b828734c4ac58cf71f/src/pip/_vendor/rich/jupyter.py#L36

---

_Comment by @charliermarsh on 2024-05-28 19:26_

I would settle for reliably disabling this in Jupyter for now.

---

_Comment by @bluss on 2024-05-28 19:27_

Refer over to this PR and their purported solution https://github.com/pypa/pip/pull/12404  (not much, but some explanations and previous experience in the same topic!)

---

_Comment by @charliermarsh on 2024-05-28 19:30_

Rich has a bunch of code to detect Jupyter: https://github.com/Textualize/rich/blob/349042fd8912ab5f0714ff9a46a70ef8a4be4700/rich/console.py#L518

---

_Comment by @ibraheemdev on 2024-05-28 19:40_

Jupyter seems to set `JPY_SESSION_NAME` and `JPY_PARENT_PID`, we might be able to use that.

---

_Closed by @ibraheemdev on 2024-05-28 21:05_

---

_Comment by @charliermarsh on 2024-05-28 21:07_

Appreciate you @bluss! And thanks @ibraheemdev for resolving so quickly.

---

_Comment by @bluss on 2024-05-29 18:17_

That was quick fix, thank you! And I'm happy to help Uv

---
