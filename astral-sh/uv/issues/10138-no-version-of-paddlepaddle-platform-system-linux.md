```yaml
number: 10138
title: "no version of paddlepaddle{platform_system == 'Linux'}==3.0.0b1"
type: issue
state: closed
author: hongbo-miao
labels: []
assignees: []
created_at: 2024-12-24T06:02:09Z
updated_at: 2024-12-24T12:59:07Z
url: https://github.com/astral-sh/uv/issues/10138
synced_at: 2026-01-10T04:36:21Z
```

# no version of paddlepaddle{platform_system == 'Linux'}==3.0.0b1

---

_Issue opened by @hongbo-miao on 2024-12-24 06:02_

# Experiment 1

I use uv create a Python 3.10 env by `uv venv`. I activated this env.

```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = "~=3.10.0"

[tool.uv]
package = false
```

However,

```sh
uv pip install magic-pdf[full]==0.10.6 --extra-index-url https://wheels.myhloli.com
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of paddlepaddle{platform_system == 'Linux'}==3.0.0b1 and magic-pdf[full]==0.10.6 depends on paddlepaddle{platform_system == 'Linux'}==3.0.0b1, we can
      conclude that magic-pdf[full]==0.10.6 cannot be used.
      And because you require magic-pdf[full]==0.10.6, we can conclude that your requirements are unsatisfiable.
```

# Experiment 2

```sh
pip install magic-pdf[full]==0.10.6 --extra-index-url https://wheels.myhloli.com
```

You can see it installed the paddlepaddle 3.0.0b1 wheel for Python 3.10 in Linux.

```sh
Collecting paddlepaddle==3.0.0b1
  Downloading paddlepaddle-3.0.0b1-cp310-cp310-manylinux1_x86_64.whl (141.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 141.8/141.8 MB 9.3 MB/s eta 0:00:00
```

inside the full log.

<details>
  <summary>Click here to see the the full log.</summary>
  
```sh
Defaulting to user installation because normal site-packages is not writeable
Looking in indexes: https://pypi.org/simple, https://wheels.myhloli.com
Collecting magic-pdf[full]==0.10.6
  Using cached magic_pdf-0.10.6-py3-none-any.whl (1.0 MB)
Collecting fast-langdetect==0.2.0
  Downloading fast_langdetect-0.2.0-py3-none-any.whl (6.4 kB)
Requirement already satisfied: numpy<2.0.0,>=1.21.6 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from magic-pdf[full]==0.10.6) (1.26.3)
Collecting pydantic<2.8.0,>=2.7.2
  Using cached pydantic-2.7.4-py3-none-any.whl (409 kB)
Collecting transformers
  Downloading transformers-4.47.1-py3-none-any.whl (10.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.1/10.1 MB 8.8 MB/s eta 0:00:00
Collecting PyMuPDF>=1.24.9
  Downloading pymupdf-1.25.1-cp39-abi3-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (20.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 20.0/20.0 MB 10.9 MB/s eta 0:00:00
Collecting click>=8.1.7
  Downloading click-8.1.8-py3-none-any.whl (98 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 98.2/98.2 KB 9.2 MB/s eta 0:00:00
Collecting loguru>=0.6.0
  Downloading loguru-0.7.3-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.6/61.6 KB 5.5 MB/s eta 0:00:00
Collecting scikit-learn>=1.0.2
  Downloading scikit_learn-1.6.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (13.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.5/13.5 MB 11.0 MB/s eta 0:00:00
Collecting boto3>=1.28.43
  Downloading boto3-1.35.87-py3-none-any.whl (139 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 139.2/139.2 KB 15.0 MB/s eta 0:00:00
Collecting Brotli>=1.1.0
  Downloading Brotli-1.1.0-cp310-cp310-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (3.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.0/3.0 MB 11.0 MB/s eta 0:00:00
Collecting pdfminer.six==20231228
  Downloading pdfminer.six-20231228-py3-none-any.whl (5.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.6/5.6 MB 10.9 MB/s eta 0:00:00
Collecting torch>=2.2.2
  Downloading torch-2.5.1-cp310-cp310-manylinux1_x86_64.whl (906.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 906.4/906.4 MB 1.8 MB/s eta 0:00:00
Requirement already satisfied: matplotlib in /home/hongbo-miao/.local/lib/python3.10/site-packages (from magic-pdf[full]==0.10.6) (3.8.2)
Collecting accelerate
  Downloading accelerate-1.2.1-py3-none-any.whl (336 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 336.4/336.4 KB 11.0 MB/s eta 0:00:00
Collecting paddlepaddle==3.0.0b1
  Downloading paddlepaddle-3.0.0b1-cp310-cp310-manylinux1_x86_64.whl (141.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 141.8/141.8 MB 9.3 MB/s eta 0:00:00
Collecting rapidocr-paddle
  Downloading rapidocr_paddle-1.4.4-py3-none-any.whl (15.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 15.0/15.0 MB 11.0 MB/s eta 0:00:00
Collecting paddleocr==2.7.3
  Downloading paddleocr-2.7.3-py3-none-any.whl (780 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 780.0/780.0 KB 8.8 MB/s eta 0:00:00
Collecting struct-eqtable==0.3.2
  Downloading struct_eqtable-0.3.2-py3-none-any.whl (26 kB)
Requirement already satisfied: PyYAML in /usr/lib/python3/dist-packages (from magic-pdf[full]==0.10.6) (5.4.1)
Collecting doclayout-yolo==0.0.2
  Downloading doclayout_yolo-0.0.2-py3-none-any.whl (708 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 708.2/708.2 KB 9.8 MB/s eta 0:00:00
Collecting unimernet==0.2.2
  Downloading unimernet-0.2.2-py3-none-any.whl (2.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 MB 10.7 MB/s eta 0:00:00
Collecting ultralytics>=8.3.48
  Downloading ultralytics-8.3.53-py3-none-any.whl (902 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 902.2/902.2 KB 11.3 MB/s eta 0:00:00
DEPRECATION: The HTML index page being used (https://wheels.myhloli.com/detectron2/) is not a proper HTML 5 document. This is in violation of PEP 503 which requires these pages to be well-formed HTML 5 documents. Please reach out to the owners of this index page, and ask them to update this index page to a valid HTML 5 document. pip 22.2 will enforce this behaviour change. Discussion can be found at https://github.com/pypa/pip/issues/10825
Collecting detectron2
  Downloading https://gcore.jsdelivr.net/gh/myhloli/wheels@main/assets/whl/detectron2/detectron2-0.6-cp310-cp310-linux_x86_64.whl (902 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 902.1/902.1 KB 2.8 MB/s eta 0:00:00
Collecting rapid-table
  Downloading rapid_table-0.3.0-py3-none-any.whl (7.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.2/7.2 MB 10.7 MB/s eta 0:00:00
Collecting torchvision<=0.18.1,>=0.17.2
  Downloading torchvision-0.18.1-cp310-cp310-manylinux1_x86_64.whl (7.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.0/7.0 MB 10.9 MB/s eta 0:00:00
Collecting einops
  Downloading einops-0.8.0-py3-none-any.whl (43 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.2/43.2 KB 4.5 MB/s eta 0:00:00
Collecting torch>=2.2.2
  Downloading torch-2.3.1-cp310-cp310-manylinux1_x86_64.whl (779.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 779.1/779.1 MB 1.9 MB/s eta 0:00:00
Requirement already satisfied: pandas>=1.1.4 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (2.1.4)
Requirement already satisfied: psutil in /home/hongbo-miao/.local/lib/python3.10/site-packages (from doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (5.9.7)
Requirement already satisfied: pillow>=7.1.2 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (10.2.0)
Collecting opencv-python>=4.6.0
  Downloading opencv_python-4.10.0.84-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (62.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 62.5/62.5 MB 10.2 MB/s eta 0:00:00
Requirement already satisfied: requests>=2.23.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (2.31.0)
Collecting py-cpuinfo
  Downloading py_cpuinfo-9.0.0-py3-none-any.whl (22 kB)
Collecting seaborn>=0.11.0
  Downloading seaborn-0.13.2-py3-none-any.whl (294 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 294.9/294.9 KB 13.5 MB/s eta 0:00:00
Collecting albumentations>=1.4.11
  Downloading albumentations-1.4.23-py3-none-any.whl (269 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 269.9/269.9 KB 9.7 MB/s eta 0:00:00
Collecting scipy>=1.4.1
  Downloading scipy-1.14.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (41.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 41.2/41.2 MB 10.7 MB/s eta 0:00:00
Requirement already satisfied: tqdm>=4.64.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (4.67.1)
Collecting thop>=0.1.1
  Downloading thop-0.1.1.post2209072238-py3-none-any.whl (15 kB)
Collecting fasttext-wheel>=0.9.2
  Downloading fasttext_wheel-0.9.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.4/4.4 MB 11.1 MB/s eta 0:00:00
Collecting langdetect>=1.0.9
  Downloading langdetect-1.0.9.tar.gz (981 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 981.5/981.5 KB 11.1 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting robust-downloader>=0.0.2
  Downloading robust_downloader-0.0.2-py3-none-any.whl (15 kB)
Collecting fire>=0.3.0
  Downloading fire-0.7.0.tar.gz (87 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 87.2/87.2 KB 6.3 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting imgaug
  Downloading imgaug-0.4.0-py2.py3-none-any.whl (948 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 948.0/948.0 KB 10.8 MB/s eta 0:00:00
Collecting attrdict
  Downloading attrdict-2.0.1-py2.py3-none-any.whl (9.9 kB)
Collecting shapely
  Downloading shapely-2.0.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.5/2.5 MB 11.0 MB/s eta 0:00:00
Collecting lmdb
  Downloading lmdb-1.5.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (294 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 294.9/294.9 KB 13.2 MB/s eta 0:00:00
Collecting python-docx
  Downloading python_docx-1.1.2-py3-none-any.whl (244 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 244.3/244.3 KB 12.2 MB/s eta 0:00:00
Collecting beautifulsoup4
  Downloading beautifulsoup4-4.12.3-py3-none-any.whl (147 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 147.9/147.9 KB 10.5 MB/s eta 0:00:00
Collecting scikit-image
  Downloading scikit_image-0.25.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (14.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.8/14.8 MB 11.0 MB/s eta 0:00:00
Requirement already satisfied: fonttools>=4.24.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from paddleocr==2.7.3->magic-pdf[full]==0.10.6) (4.47.0)
Collecting cython
  Downloading Cython-3.0.11-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.6/3.6 MB 11.1 MB/s eta 0:00:00
Collecting visualdl
  Downloading visualdl-2.5.3-py3-none-any.whl (6.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.3/6.3 MB 11.1 MB/s eta 0:00:00
Collecting premailer
  Downloading premailer-3.10.0-py2.py3-none-any.whl (19 kB)
Collecting opencv-python>=4.6.0
  Downloading opencv_python-4.6.0.66-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (60.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 60.9/60.9 MB 10.2 MB/s eta 0:00:00
Collecting pyclipper
  Downloading pyclipper-1.3.0.post6-cp310-cp310-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (912 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 912.2/912.2 KB 10.9 MB/s eta 0:00:00
Requirement already satisfied: lxml in /home/hongbo-miao/.local/lib/python3.10/site-packages (from paddleocr==2.7.3->magic-pdf[full]==0.10.6) (5.1.0)
Collecting pdf2docx
  Downloading pdf2docx-0.5.8-py3-none-any.whl (132 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 132.0/132.0 KB 12.0 MB/s eta 0:00:00
Collecting openpyxl
  Downloading openpyxl-3.1.5-py2.py3-none-any.whl (250 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 250.9/250.9 KB 9.0 MB/s eta 0:00:00
Collecting opencv-contrib-python<=4.6.0.66
  Downloading opencv_contrib_python-4.6.0.66-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (67.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 67.1/67.1 MB 10.2 MB/s eta 0:00:00
Collecting rapidfuzz
  Downloading rapidfuzz-3.11.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.1/3.1 MB 10.8 MB/s eta 0:00:00
Collecting networkx
  Downloading networkx-3.4.2-py3-none-any.whl (1.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 10.8 MB/s eta 0:00:00
Collecting astor
  Downloading astor-0.8.1-py2.py3-none-any.whl (27 kB)
Requirement already satisfied: typing-extensions in /home/hongbo-miao/.local/lib/python3.10/site-packages (from paddlepaddle==3.0.0b1->magic-pdf[full]==0.10.6) (4.12.2)
Collecting httpx
  Downloading httpx-0.28.1-py3-none-any.whl (73 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 73.5/73.5 KB 9.9 MB/s eta 0:00:00
Collecting opt-einsum==3.3.0
  Downloading opt_einsum-3.3.0-py3-none-any.whl (65 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 65.5/65.5 KB 9.6 MB/s eta 0:00:00
Collecting protobuf>=3.20.2
  Downloading protobuf-5.29.2-cp38-abi3-manylinux2014_x86_64.whl (319 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 319.7/319.7 KB 9.6 MB/s eta 0:00:00
Collecting decorator
  Downloading decorator-5.1.1-py3-none-any.whl (9.1 kB)
Collecting cryptography>=36.0.0
  Downloading cryptography-44.0.0-cp39-abi3-manylinux_2_28_x86_64.whl (4.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.2/4.2 MB 11.1 MB/s eta 0:00:00
Requirement already satisfied: charset-normalizer>=2.0.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from pdfminer.six==20231228->magic-pdf[full]==0.10.6) (3.3.2)
Collecting eva-decord<0.7.0,>=0.6.1
  Downloading eva_decord-0.6.1-py3-none-manylinux2010_x86_64.whl (13.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.6/13.6 MB 10.9 MB/s eta 0:00:00
Collecting timm<0.10.0,>=0.9.16
  Downloading timm-0.9.16-py3-none-any.whl (2.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.2/2.2 MB 11.1 MB/s eta 0:00:00
Collecting matplotlib
  Downloading matplotlib-3.10.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (8.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.6/8.6 MB 11.0 MB/s eta 0:00:00
Collecting webdataset<0.3.0,>=0.2.86
  Downloading webdataset-0.2.100-py3-none-any.whl (74 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 74.8/74.8 KB 4.4 MB/s eta 0:00:00
Collecting wand<0.7.0,>=0.6.13
  Downloading Wand-0.6.13-py2.py3-none-any.whl (143 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 143.8/143.8 KB 13.2 MB/s eta 0:00:00
Collecting ftfy<7.0.0,>=6.2.0
  Downloading ftfy-6.3.1-py3-none-any.whl (44 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 44.8/44.8 KB 3.8 MB/s eta 0:00:00
Collecting iopath<0.2.0,>=0.1.9
  Downloading iopath-0.1.10.tar.gz (42 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 42.2/42.2 KB 5.1 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting fairscale<0.5.0,>=0.4.13
  Downloading fairscale-0.4.13.tar.gz (266 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 266.3/266.3 KB 12.3 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
  Preparing metadata (pyproject.toml) ... done
Collecting evaluate<0.5.0,>=0.4.1
  Downloading evaluate-0.4.3-py3-none-any.whl (84 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 84.0/84.0 KB 9.2 MB/s eta 0:00:00
Collecting omegaconf<3.0.0,>=2.3.0
  Downloading omegaconf-2.3.0-py3-none-any.whl (79 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 79.5/79.5 KB 7.3 MB/s eta 0:00:00
Collecting transformers
  Downloading transformers-4.42.4-py3-none-any.whl (9.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 9.3/9.3 MB 11.0 MB/s eta 0:00:00
Collecting regex!=2019.12.17
  Downloading regex-2024.11.6-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (781 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 781.7/781.7 KB 11.9 MB/s eta 0:00:00
Requirement already satisfied: packaging>=20.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from transformers->magic-pdf[full]==0.10.6) (23.2)
Collecting tokenizers<0.20,>=0.19
  Downloading tokenizers-0.19.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.6/3.6 MB 11.0 MB/s eta 0:00:00
Requirement already satisfied: huggingface-hub<1.0,>=0.23.2 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from transformers->magic-pdf[full]==0.10.6) (0.27.0)
Requirement already satisfied: filelock in /home/hongbo-miao/.local/lib/python3.10/site-packages (from transformers->magic-pdf[full]==0.10.6) (3.16.1)
Collecting safetensors>=0.4.1
  Downloading safetensors-0.4.5-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (435 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 435.0/435.0 KB 8.8 MB/s eta 0:00:00
Collecting jmespath<2.0.0,>=0.7.1
  Downloading jmespath-1.0.1-py3-none-any.whl (20 kB)
Collecting s3transfer<0.11.0,>=0.10.0
  Downloading s3transfer-0.10.4-py3-none-any.whl (83 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 83.2/83.2 KB 12.6 MB/s eta 0:00:00
Collecting botocore<1.36.0,>=1.35.87
  Downloading botocore-1.35.87-py3-none-any.whl (13.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.3/13.3 MB 11.1 MB/s eta 0:00:00
Requirement already satisfied: cycler>=0.10 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from matplotlib->magic-pdf[full]==0.10.6) (0.12.1)
Requirement already satisfied: pyparsing>=2.3.1 in /usr/lib/python3/dist-packages (from matplotlib->magic-pdf[full]==0.10.6) (2.4.7)
Requirement already satisfied: python-dateutil>=2.7 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from matplotlib->magic-pdf[full]==0.10.6) (2.8.2)
Requirement already satisfied: contourpy>=1.0.1 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from matplotlib->magic-pdf[full]==0.10.6) (1.2.0)
Requirement already satisfied: kiwisolver>=1.3.1 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from matplotlib->magic-pdf[full]==0.10.6) (1.4.5)
Collecting annotated-types>=0.4.0
  Downloading annotated_types-0.7.0-py3-none-any.whl (13 kB)
Collecting pydantic-core==2.18.4
  Downloading pydantic_core-2.18.4-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.0/2.0 MB 11.0 MB/s eta 0:00:00
Collecting threadpoolctl>=3.1.0
  Downloading threadpoolctl-3.5.0-py3-none-any.whl (18 kB)
Collecting joblib>=1.2.0
  Downloading joblib-1.4.2-py3-none-any.whl (301 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 301.8/301.8 KB 10.6 MB/s eta 0:00:00
Requirement already satisfied: jinja2 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from torch>=2.2.2->magic-pdf[full]==0.10.6) (3.1.3)
Collecting triton==2.3.1
  Downloading triton-2.3.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (168.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 168.1/168.1 MB 9.4 MB/s eta 0:00:00
Collecting nvidia-cuda-runtime-cu12==12.1.105
  Downloading nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (823 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 823.6/823.6 KB 10.8 MB/s eta 0:00:00
Collecting nvidia-cudnn-cu12==8.9.2.26
  Downloading nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl (731.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 731.7/731.7 MB 4.6 MB/s eta 0:00:00
Collecting nvidia-cufft-cu12==11.0.2.54
  Downloading nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl (121.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 121.6/121.6 MB 7.6 MB/s eta 0:00:00
Collecting nvidia-cublas-cu12==12.1.3.1
  Downloading nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl (410.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 410.6/410.6 MB 7.1 MB/s eta 0:00:00
Collecting nvidia-cusolver-cu12==11.4.5.107
  Downloading nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl (124.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 124.2/124.2 MB 9.5 MB/s eta 0:00:00
Requirement already satisfied: fsspec in /home/hongbo-miao/.local/lib/python3.10/site-packages (from torch>=2.2.2->magic-pdf[full]==0.10.6) (2024.12.0)
Collecting nvidia-cuda-cupti-cu12==12.1.105
  Downloading nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (14.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.1/14.1 MB 11.0 MB/s eta 0:00:00
Collecting nvidia-cusparse-cu12==12.1.0.106
  Downloading nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl (196.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 196.0/196.0 MB 7.3 MB/s eta 0:00:00
Collecting nvidia-nccl-cu12==2.20.5
  Downloading nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_x86_64.whl (176.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 176.2/176.2 MB 8.9 MB/s eta 0:00:00
Collecting nvidia-nvtx-cu12==12.1.105
  Downloading nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (99 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 99.1/99.1 KB 7.8 MB/s eta 0:00:00
Collecting nvidia-curand-cu12==10.3.2.106
  Downloading nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl (56.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 56.5/56.5 MB 10.4 MB/s eta 0:00:00
Requirement already satisfied: sympy in /home/hongbo-miao/.local/lib/python3.10/site-packages (from torch>=2.2.2->magic-pdf[full]==0.10.6) (1.12)
Collecting nvidia-cuda-nvrtc-cu12==12.1.105
  Downloading nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (23.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 23.7/23.7 MB 10.9 MB/s eta 0:00:00
Collecting nvidia-nvjitlink-cu12
  Downloading nvidia_nvjitlink_cu12-12.6.85-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl (19.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 19.7/19.7 MB 10.9 MB/s eta 0:00:00
Collecting ultralytics-thop>=2.0.0
  Downloading ultralytics_thop-2.0.13-py3-none-any.whl (26 kB)
Collecting iopath<0.2.0,>=0.1.9
  Downloading iopath-0.1.9-py3-none-any.whl (27 kB)
Collecting hydra-core>=1.1
  Downloading hydra_core-1.3.2-py3-none-any.whl (154 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 154.5/154.5 KB 7.9 MB/s eta 0:00:00
Collecting fvcore<0.1.6,>=0.1.5
  Downloading fvcore-0.1.5.post20221221.tar.gz (50 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 50.2/50.2 KB 4.6 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting tensorboard
  Downloading tensorboard-2.18.0-py3-none-any.whl (5.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.5/5.5 MB 11.1 MB/s eta 0:00:00
Collecting yacs>=0.1.8
  Downloading yacs-0.1.8-py3-none-any.whl (14 kB)
Collecting black
  Downloading black-24.10.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_28_x86_64.whl (1.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.8/1.8 MB 10.6 MB/s eta 0:00:00
Collecting termcolor>=1.1
  Downloading termcolor-2.5.0-py3-none-any.whl (7.8 kB)
Collecting cloudpickle
  Downloading cloudpickle-3.1.0-py3-none-any.whl (22 kB)
Collecting pycocotools>=2.0.2
  Downloading pycocotools-2.0.8-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (427 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 427.8/427.8 KB 9.8 MB/s eta 0:00:00
Collecting tabulate
  Downloading tabulate-0.9.0-py3-none-any.whl (35 kB)
Collecting PyYAML
  Downloading PyYAML-6.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (751 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 751.2/751.2 KB 11.3 MB/s eta 0:00:00
Collecting onnxruntime>=1.7.0
  Downloading onnxruntime-1.20.1-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (13.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.3/13.3 MB 11.0 MB/s eta 0:00:00
Requirement already satisfied: six>=1.15.0 in /usr/lib/python3/dist-packages (from rapidocr-paddle->magic-pdf[full]==0.10.6) (1.16.0)
Collecting albucore==0.0.21
  Downloading albucore-0.0.21-py3-none-any.whl (12 kB)
Collecting eval-type-backport
  Downloading eval_type_backport-0.2.2-py3-none-any.whl (5.8 kB)
Collecting albumentations>=1.4.11
  Downloading albumentations-1.4.22-py3-none-any.whl (258 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 258.1/258.1 KB 10.2 MB/s eta 0:00:00
  Downloading albumentations-1.4.21-py3-none-any.whl (227 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 227.9/227.9 KB 9.2 MB/s eta 0:00:00
Collecting albucore==0.0.20
  Downloading albucore-0.0.20-py3-none-any.whl (12 kB)
Collecting opencv-python-headless>=4.9.0.80
  Downloading opencv_python_headless-4.10.0.84-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (49.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 49.9/49.9 MB 10.4 MB/s eta 0:00:00
Collecting stringzilla>=3.10.4
  Downloading stringzilla-3.11.2-cp310-cp310-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_28_x86_64.whl (303 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 303.5/303.5 KB 10.1 MB/s eta 0:00:00
Collecting simsimd>=5.9.2
  Downloading simsimd-6.2.1-cp310-cp310-manylinux_2_28_x86_64.whl (632 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 632.7/632.7 KB 12.0 MB/s eta 0:00:00
Requirement already satisfied: urllib3!=2.2.0,<3,>=1.25.4 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from botocore<1.36.0,>=1.35.87->boto3>=1.28.43->magic-pdf[full]==0.10.6) (2.1.0)
Collecting cffi>=1.12
  Downloading cffi-1.17.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (446 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 446.2/446.2 KB 10.5 MB/s eta 0:00:00
Collecting multiprocess
  Downloading multiprocess-0.70.17-py310-none-any.whl (134 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 134.8/134.8 KB 9.2 MB/s eta 0:00:00
Collecting dill
  Downloading dill-0.3.9-py3-none-any.whl (119 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 119.4/119.4 KB 10.6 MB/s eta 0:00:00
Collecting datasets>=2.0.0
  Downloading datasets-3.2.0-py3-none-any.whl (480 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 480.6/480.6 KB 12.3 MB/s eta 0:00:00
Collecting xxhash
  Downloading xxhash-3.5.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (194 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 194.1/194.1 KB 10.1 MB/s eta 0:00:00
Requirement already satisfied: pybind11>=2.2 in /usr/lib/python3/dist-packages (from fasttext-wheel>=0.9.2->fast-langdetect==0.2.0->magic-pdf[full]==0.10.6) (2.9.1)
Requirement already satisfied: setuptools>=0.7.0 in /usr/lib/python3/dist-packages (from fasttext-wheel>=0.9.2->fast-langdetect==0.2.0->magic-pdf[full]==0.10.6) (59.6.0)
Collecting wcwidth
  Downloading wcwidth-0.2.13-py2.py3-none-any.whl (34 kB)
Collecting antlr4-python3-runtime==4.9.*
  Downloading antlr4-python3-runtime-4.9.3.tar.gz (117 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 117.0/117.0 KB 9.6 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting portalocker
  Downloading portalocker-3.0.0-py3-none-any.whl (19 kB)
Collecting flatbuffers
  Downloading flatbuffers-24.12.23-py2.py3-none-any.whl (30 kB)
Collecting coloredlogs
  Downloading coloredlogs-15.0.1-py2.py3-none-any.whl (46 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 46.0/46.0 KB 9.7 MB/s eta 0:00:00
Requirement already satisfied: pytz>=2020.1 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from pandas>=1.1.4->doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (2023.3.post1)
Requirement already satisfied: tzdata>=2022.1 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from pandas>=1.1.4->doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (2023.4)
Requirement already satisfied: certifi>=2017.4.17 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from requests>=2.23.0->doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (2023.11.17)
Requirement already satisfied: idna<4,>=2.5 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from requests>=2.23.0->doclayout-yolo==0.0.2->magic-pdf[full]==0.10.6) (3.6)
Collecting colorlog
  Downloading colorlog-6.9.0-py3-none-any.whl (11 kB)
Collecting braceexpand
  Downloading braceexpand-0.1.7-py2.py3-none-any.whl (5.9 kB)
Collecting soupsieve>1.2
  Downloading soupsieve-2.6-py3-none-any.whl (36 kB)
Collecting tomli>=1.1.0
  Downloading tomli-2.2.1-py3-none-any.whl (14 kB)
Collecting platformdirs>=2
  Downloading platformdirs-4.3.6-py3-none-any.whl (18 kB)
Collecting pathspec>=0.9.0
  Downloading pathspec-0.12.1-py3-none-any.whl (31 kB)
Collecting mypy-extensions>=0.4.3
  Downloading mypy_extensions-1.0.0-py3-none-any.whl (4.7 kB)
Collecting anyio
  Downloading anyio-4.7.0-py3-none-any.whl (93 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 93.1/93.1 KB 6.6 MB/s eta 0:00:00
Collecting httpcore==1.*
  Downloading httpcore-1.0.7-py3-none-any.whl (78 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 78.6/78.6 KB 6.1 MB/s eta 0:00:00
Collecting h11<0.15,>=0.13
  Downloading h11-0.14.0-py3-none-any.whl (58 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 58.3/58.3 KB 12.9 MB/s eta 0:00:00
Collecting imageio
  Downloading imageio-2.36.1-py3-none-any.whl (315 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 315.4/315.4 KB 8.9 MB/s eta 0:00:00
Collecting lazy-loader>=0.4
  Downloading lazy_loader-0.4-py3-none-any.whl (12 kB)
Collecting tifffile>=2022.8.12
  Downloading tifffile-2024.12.12-py3-none-any.whl (227 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 227.5/227.5 KB 8.7 MB/s eta 0:00:00
Requirement already satisfied: MarkupSafe>=2.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from jinja2->torch>=2.2.2->magic-pdf[full]==0.10.6) (2.1.3)
Collecting et-xmlfile
  Downloading et_xmlfile-2.0.0-py3-none-any.whl (18 kB)
Collecting cachetools
  Downloading cachetools-5.5.0-py3-none-any.whl (9.5 kB)
Collecting cssutils
  Downloading cssutils-2.11.1-py3-none-any.whl (385 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 385.7/385.7 KB 9.5 MB/s eta 0:00:00
Collecting cssselect
  Downloading cssselect-1.2.0-py2.py3-none-any.whl (18 kB)
Requirement already satisfied: mpmath>=0.19 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from sympy->torch>=2.2.2->magic-pdf[full]==0.10.6) (1.3.0)
Collecting markdown>=2.6.8
  Downloading Markdown-3.7-py3-none-any.whl (106 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 106.3/106.3 KB 8.8 MB/s eta 0:00:00
Collecting grpcio>=1.48.2
  Downloading grpcio-1.68.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (5.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.9/5.9 MB 11.3 MB/s eta 0:00:00
Collecting tensorboard-data-server<0.8.0,>=0.7.0
  Downloading tensorboard_data_server-0.7.2-py3-none-manylinux_2_31_x86_64.whl (6.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.6/6.6 MB 11.0 MB/s eta 0:00:00
Collecting werkzeug>=1.0.1
  Downloading werkzeug-3.1.3-py3-none-any.whl (224 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 224.5/224.5 KB 11.1 MB/s eta 0:00:00
Collecting absl-py>=0.4
  Downloading absl_py-2.1.0-py3-none-any.whl (133 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.7/133.7 KB 10.2 MB/s eta 0:00:00
Collecting bce-python-sdk
  Downloading bce_python_sdk-0.9.25-py3-none-any.whl (337 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 337.0/337.0 KB 11.3 MB/s eta 0:00:00
Collecting flask>=1.1.1
  Downloading flask-3.1.0-py3-none-any.whl (102 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 103.0/103.0 KB 9.5 MB/s eta 0:00:00
Collecting Flask-Babel>=3.0.0
  Downloading flask_babel-4.0.0-py3-none-any.whl (9.6 kB)
Collecting rarfile
  Downloading rarfile-4.2-py3-none-any.whl (29 kB)
Collecting pycparser
  Downloading pycparser-2.22-py3-none-any.whl (117 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 117.6/117.6 KB 7.3 MB/s eta 0:00:00
Collecting requests>=2.23.0
  Downloading requests-2.32.3-py3-none-any.whl (64 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 64.9/64.9 KB 5.2 MB/s eta 0:00:00
Collecting dill
  Downloading dill-0.3.8-py3-none-any.whl (116 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 116.3/116.3 KB 9.6 MB/s eta 0:00:00
Collecting fsspec[http]>=2021.05.0
  Downloading fsspec-2024.9.0-py3-none-any.whl (179 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 179.3/179.3 KB 9.6 MB/s eta 0:00:00
Collecting aiohttp
  Downloading aiohttp-3.11.11-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.6/1.6 MB 10.9 MB/s eta 0:00:00
Collecting multiprocess
  Downloading multiprocess-0.70.16-py310-none-any.whl (134 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 134.8/134.8 KB 9.9 MB/s eta 0:00:00
Collecting pyarrow>=15.0.0
  Downloading pyarrow-18.1.0-cp310-cp310-manylinux_2_28_x86_64.whl (40.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 40.1/40.1 MB 10.4 MB/s eta 0:00:00
Collecting itsdangerous>=2.2
  Downloading itsdangerous-2.2.0-py3-none-any.whl (16 kB)
Collecting blinker>=1.9
  Downloading blinker-1.9.0-py3-none-any.whl (8.5 kB)
Collecting Babel>=2.12
  Downloading babel-2.16.0-py3-none-any.whl (9.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 9.6/9.6 MB 11.1 MB/s eta 0:00:00
Collecting sniffio>=1.1
  Downloading sniffio-1.3.1-py3-none-any.whl (10 kB)
Collecting exceptiongroup>=1.0.2
  Downloading exceptiongroup-1.2.2-py3-none-any.whl (16 kB)
Requirement already satisfied: future>=0.6.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from bce-python-sdk->visualdl->paddleocr==2.7.3->magic-pdf[full]==0.10.6) (0.18.3)
Collecting pycryptodome>=3.8.0
  Downloading pycryptodome-3.21.0-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 MB 11.3 MB/s eta 0:00:00
Collecting humanfriendly>=9.1
  Downloading humanfriendly-10.0-py2.py3-none-any.whl (86 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 86.8/86.8 KB 7.2 MB/s eta 0:00:00
Requirement already satisfied: more-itertools in /usr/lib/python3/dist-packages (from cssutils->premailer->paddleocr==2.7.3->magic-pdf[full]==0.10.6) (8.10.0)
Requirement already satisfied: attrs>=17.3.0 in /home/hongbo-miao/.local/lib/python3.10/site-packages (from aiohttp->datasets>=2.0.0->evaluate<0.5.0,>=0.4.1->unimernet==0.2.2->magic-pdf[full]==0.10.6) (23.2.0)
Collecting yarl<2.0,>=1.17.0
  Downloading yarl-1.18.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (319 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 319.7/319.7 KB 10.8 MB/s eta 0:00:00
Collecting multidict<7.0,>=4.5
  Downloading multidict-6.1.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (124 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 124.6/124.6 KB 23.7 MB/s eta 0:00:00
Collecting aiohappyeyeballs>=2.3.0
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl (14 kB)
Collecting async-timeout<6.0,>=4.0
  Downloading async_timeout-5.0.1-py3-none-any.whl (6.2 kB)
Collecting frozenlist>=1.1.1
  Downloading frozenlist-1.5.0-cp310-cp310-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (241 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 241.9/241.9 KB 17.2 MB/s eta 0:00:00
Collecting propcache>=0.2.0
  Downloading propcache-0.2.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (205 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 205.1/205.1 KB 8.9 MB/s eta 0:00:00
Collecting aiosignal>=1.1.2
  Downloading aiosignal-1.3.2-py2.py3-none-any.whl (7.6 kB)
Building wheels for collected packages: fairscale, fire, fvcore, antlr4-python3-runtime, langdetect
  Building wheel for fairscale (pyproject.toml) ... done
  Created wheel for fairscale: filename=fairscale-0.4.13-py3-none-any.whl size=332138 sha256=40c20dad43d4e450771b88f1445aaf5ac9ff851f73bdb3ebd8535859a0437b5f
  Stored in directory: /home/hongbo-miao/.cache/pip/wheels/78/a4/c0/fb0a7ef03cff161611c3fa40c6cf898f76e58ec421b88e8cb3
  Building wheel for fire (setup.py) ... done
  Created wheel for fire: filename=fire-0.7.0-py3-none-any.whl size=114262 sha256=fc53f693efdc7cf94cbc844512e5ed7105d7fda0ccbb94f7ab0e490af875c1cd
  Stored in directory: /home/hongbo-miao/.cache/pip/wheels/19/39/2f/2d3cadc408a8804103f1c34ddd4b9f6a93497b11fa96fe738e
  Building wheel for fvcore (setup.py) ... done
  Created wheel for fvcore: filename=fvcore-0.1.5.post20221221-py3-none-any.whl size=61431 sha256=3a9a42caca60579662de42e42ea0fb3869adb0a0c733f4ec25dfa3aaa940833c
  Stored in directory: /home/hongbo-miao/.cache/pip/wheels/01/c0/af/77c1cf53a1be9e42a52b48e5af2169d40ec2e89f7362489dd0
  Building wheel for antlr4-python3-runtime (setup.py) ... done
  Created wheel for antlr4-python3-runtime: filename=antlr4_python3_runtime-4.9.3-py3-none-any.whl size=144575 sha256=bcc58ddb01e0ca1b68d8a30f8095e680ffd0d7896f7138467a9ea49d700fd760
  Stored in directory: /home/hongbo-miao/.cache/pip/wheels/12/93/dd/1f6a127edc45659556564c5730f6d4e300888f4bca2d4c5a88
  Building wheel for langdetect (setup.py) ... done
  Created wheel for langdetect: filename=langdetect-1.0.9-py3-none-any.whl size=993242 sha256=e60bf9d500fd0dfb4b7b9ddbaf9f612cafcd14dbb468de7292cab834901f7429
  Stored in directory: /home/hongbo-miao/.cache/pip/wheels/95/03/7d/59ea870c70ce4e5a370638b5462a7711ab78fba2f655d05106
Successfully built fairscale fire fvcore antlr4-python3-runtime langdetect
Installing collected packages: wcwidth, wand, stringzilla, simsimd, pyclipper, py-cpuinfo, lmdb, flatbuffers, Brotli, braceexpand, antlr4-python3-runtime, xxhash, werkzeug, triton, tomli, tifffile, threadpoolctl, termcolor, tensorboard-data-server, tabulate, soupsieve, sniffio, shapely, scipy, safetensors, requests, regex, rarfile, rapidfuzz, PyYAML, python-docx, PyMuPDF, pydantic-core, pycryptodome, pycparser, pyarrow, protobuf, propcache, portalocker, platformdirs, pathspec, opt-einsum, opencv-python-headless, opencv-python, opencv-contrib-python, nvidia-nvtx-cu12, nvidia-nvjitlink-cu12, nvidia-nccl-cu12, nvidia-curand-cu12, nvidia-cufft-cu12, nvidia-cuda-runtime-cu12, nvidia-cuda-nvrtc-cu12, nvidia-cuda-cupti-cu12, nvidia-cublas-cu12, networkx, mypy-extensions, multidict, markdown, loguru, lazy-loader, langdetect, joblib, jmespath, itsdangerous, imageio, humanfriendly, h11, grpcio, ftfy, fsspec, frozenlist, fasttext-wheel, exceptiongroup, eval-type-backport, eva-decord, et-xmlfile, einops, dill, decorator, cython, cssutils, cssselect, colorlog, cloudpickle, click, cachetools, blinker, Babel, attrdict, async-timeout, astor, annotated-types, aiohappyeyeballs, absl-py, yarl, yacs, webdataset, tensorboard, scikit-learn, scikit-image, robust-downloader, rapidocr-paddle, pydantic, premailer, openpyxl, omegaconf, nvidia-cusparse-cu12, nvidia-cudnn-cu12, multiprocess, matplotlib, iopath, httpcore, flask, fire, coloredlogs, cffi, botocore, black, beautifulsoup4, bce-python-sdk, anyio, albucore, aiosignal, tokenizers, seaborn, s3transfer, pycocotools, pdf2docx, onnxruntime, nvidia-cusolver-cu12, imgaug, hydra-core, httpx, fvcore, Flask-Babel, fast-langdetect, cryptography, albumentations, aiohttp, visualdl, transformers, torch, rapid-table, pdfminer.six, paddlepaddle, detectron2, boto3, ultralytics-thop, torchvision, thop, struct-eqtable, paddleocr, magic-pdf, fairscale, datasets, accelerate, ultralytics, timm, evaluate, doclayout-yolo, unimernet
  Attempting uninstall: requests
    Found existing installation: requests 2.31.0
    Uninstalling requests-2.31.0:
      Successfully uninstalled requests-2.31.0
  Attempting uninstall: fsspec
    Found existing installation: fsspec 2024.12.0
    Uninstalling fsspec-2024.12.0:
      Successfully uninstalled fsspec-2024.12.0
  Attempting uninstall: matplotlib
    Found existing installation: matplotlib 3.8.2
    Uninstalling matplotlib-3.8.2:
      Successfully uninstalled matplotlib-3.8.2
Successfully installed Babel-2.16.0 Brotli-1.1.0 Flask-Babel-4.0.0 PyMuPDF-1.25.1 PyYAML-6.0.2 absl-py-2.1.0 accelerate-1.2.1 aiohappyeyeballs-2.4.4 aiohttp-3.11.11 aiosignal-1.3.2 albucore-0.0.20 albumentations-1.4.21 annotated-types-0.7.0 antlr4-python3-runtime-4.9.3 anyio-4.7.0 astor-0.8.1 async-timeout-5.0.1 attrdict-2.0.1 bce-python-sdk-0.9.25 beautifulsoup4-4.12.3 black-24.10.0 blinker-1.9.0 boto3-1.35.87 botocore-1.35.87 braceexpand-0.1.7 cachetools-5.5.0 cffi-1.17.1 click-8.1.8 cloudpickle-3.1.0 coloredlogs-15.0.1 colorlog-6.9.0 cryptography-44.0.0 cssselect-1.2.0 cssutils-2.11.1 cython-3.0.11 datasets-3.2.0 decorator-5.1.1 detectron2-0.6 dill-0.3.8 doclayout-yolo-0.0.2 einops-0.8.0 et-xmlfile-2.0.0 eva-decord-0.6.1 eval-type-backport-0.2.2 evaluate-0.4.3 exceptiongroup-1.2.2 fairscale-0.4.13 fast-langdetect-0.2.0 fasttext-wheel-0.9.2 fire-0.7.0 flask-3.1.0 flatbuffers-24.12.23 frozenlist-1.5.0 fsspec-2024.9.0 ftfy-6.3.1 fvcore-0.1.5.post20221221 grpcio-1.68.1 h11-0.14.0 httpcore-1.0.7 httpx-0.28.1 humanfriendly-10.0 hydra-core-1.3.2 imageio-2.36.1 imgaug-0.4.0 iopath-0.1.9 itsdangerous-2.2.0 jmespath-1.0.1 joblib-1.4.2 langdetect-1.0.9 lazy-loader-0.4 lmdb-1.5.1 loguru-0.7.3 magic-pdf-0.10.6 markdown-3.7 matplotlib-3.10.0 multidict-6.1.0 multiprocess-0.70.16 mypy-extensions-1.0.0 networkx-3.4.2 nvidia-cublas-cu12-12.1.3.1 nvidia-cuda-cupti-cu12-12.1.105 nvidia-cuda-nvrtc-cu12-12.1.105 nvidia-cuda-runtime-cu12-12.1.105 nvidia-cudnn-cu12-8.9.2.26 nvidia-cufft-cu12-11.0.2.54 nvidia-curand-cu12-10.3.2.106 nvidia-cusolver-cu12-11.4.5.107 nvidia-cusparse-cu12-12.1.0.106 nvidia-nccl-cu12-2.20.5 nvidia-nvjitlink-cu12-12.6.85 nvidia-nvtx-cu12-12.1.105 omegaconf-2.3.0 onnxruntime-1.20.1 opencv-contrib-python-4.6.0.66 opencv-python-4.6.0.66 opencv-python-headless-4.10.0.84 openpyxl-3.1.5 opt-einsum-3.3.0 paddleocr-2.7.3 paddlepaddle-3.0.0b1 pathspec-0.12.1 pdf2docx-0.5.8 pdfminer.six-20231228 platformdirs-4.3.6 portalocker-3.0.0 premailer-3.10.0 propcache-0.2.1 protobuf-5.29.2 py-cpuinfo-9.0.0 pyarrow-18.1.0 pyclipper-1.3.0.post6 pycocotools-2.0.8 pycparser-2.22 pycryptodome-3.21.0 pydantic-2.7.4 pydantic-core-2.18.4 python-docx-1.1.2 rapid-table-0.3.0 rapidfuzz-3.11.0 rapidocr-paddle-1.4.4 rarfile-4.2 regex-2024.11.6 requests-2.32.3 robust-downloader-0.0.2 s3transfer-0.10.4 safetensors-0.4.5 scikit-image-0.25.0 scikit-learn-1.6.0 scipy-1.14.1 seaborn-0.13.2 shapely-2.0.6 simsimd-6.2.1 sniffio-1.3.1 soupsieve-2.6 stringzilla-3.11.2 struct-eqtable-0.3.2 tabulate-0.9.0 tensorboard-2.18.0 tensorboard-data-server-0.7.2 termcolor-2.5.0 thop-0.1.1.post2209072238 threadpoolctl-3.5.0 tifffile-2024.12.12 timm-0.9.16 tokenizers-0.19.1 tomli-2.2.1 torch-2.3.1 torchvision-0.18.1 transformers-4.42.4 triton-2.3.1 ultralytics-8.3.53 ultralytics-thop-2.0.13 unimernet-0.2.2 visualdl-2.5.3 wand-0.6.13 wcwidth-0.2.13 webdataset-0.2.100 werkzeug-3.1.3 xxhash-3.5.0 yacs-0.1.8 yarl-1.18.3
```
</details>

I know uv is strict for Python dependencies. However, in this case,  paddlepaddle 3.0.0b1 does have a wheel for Linux. I am not sure where should I open this issue. But maybe I can get some hints here so I can open a proper ticket on paddlepaddle side. Thanks! ☺️

---

_Comment by @dimbleby on 2024-12-24 07:28_

uv's handling of pre-releases is... unusual.  I expect that even though magic-pdf explicitly requests the pre-release, the uv default is to disallow that.

There is a flag somewhere with some options you should try around pre-release behaviour.

---

_Comment by @FishAlchemist on 2024-12-24 07:43_

@dimbleby I think you're referring to this. The command below works, but I'm wondering if there's a better way.
```
uv pip install magic-pdf[full]==0.10.6 --extra-index-url https://wheels.myhloli.com --prerelease allow
```
https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility
https://docs.astral.sh/uv/reference/settings/#prerelease

```
--prerelease prerelease
The strategy to use when considering pre-release versions.

By default, uv will accept pre-releases for packages that only publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (if-necessary-or-explicit).

May also be set with the UV_PRERELEASE environment variable.

Possible values:

disallow: Disallow all pre-release versions
allow: Allow all pre-release versions
if-necessary: Allow pre-release versions if all versions of a package are pre-release
explicit: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
if-necessary-or-explicit: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements
````

---

_Comment by @dimbleby on 2024-12-24 07:57_

imo a better way would be for uv to treat pre-releases more as everyone else does, and more like the specs require, but you will have to petition the maintainers - I am just a passer-by

---

_Comment by @hongbo-miao on 2024-12-24 08:24_

Thank you @dimbleby @FishAlchemist I totally agree, `magic-pdf` should avoid using pre-release dependency in first place as it is not good for production. I will propose in the magic-pdf repo side. On the other side, I guess we are not living in a perfect world.

I also found `uv pip install magic-pdf[full]==0.10.6 --extra-index-url https://wheels.myhloli.com --prerelease allow` works.

Here is my pyproject.toml file:

```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = "~=3.10.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==0.10.6"
]

[tool.uv]
package = false

[[tool.uv.index]]
name = "myhloli"
url = "https://wheels.myhloli.com"
explicit = true

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git" }
magic-pdf = { index = "myhloli" }
```

I have to install manually by

```
uv venv
uv pip install torch --index-url https://download.pytorch.org/whl/cu124
uv pip install --no-build-isolation git+https://github.com/facebookresearch/detectron2.git
uv pip install magic-pdf[full]==0.10.6 --prerelease=allow
```
The way to install `detectron2` refers to https://github.com/astral-sh/uv/issues/1715#issuecomment-2288734915
However, there is no proper lock file got generated, I guess currently there is no way to generate lock file after `uv pip install`, right?

I tried `uv lock --prerelease=allow` and it does not work.

Thanks!

---

_Comment by @hongbo-miao on 2024-12-24 08:32_

(Proposed the change at magic-pdf repo side for avoid using pre-release dependency in the final version: https://github.com/opendatalab/MinerU/issues/1354 ☺️)

---

_Comment by @FishAlchemist on 2024-12-24 09:23_

@hongbo-miao  According to your settings, you need to remove ``magic-pdf = { index = "myhloli" }`` unless you expect magic-pdf to search only from the index ``myhloli``
e.g. (include ``prerelease="allow"`` (https://docs.astral.sh/uv/reference/settings/#prerelease))
```toml
[tool.uv]
package = false
prerelease = "allow"
[[tool.uv.index]]
name = "myhloli"
url = "https://wheels.myhloli.com"
explicit = true

[tool.uv.sources]
detectron2 = { index = "myhloli"} # This is just me being lazy about dealing with Git issues, so I'm using this instead.
#magic-pdf = { index = "myhloli" }
```
https://docs.astral.sh/uv/configuration/indexes/#pinning-a-package-to-an-index
> If an index is marked as both default = true and explicit = true, it will be treated as an explicit index (i.e., only usable via tool.uv.sources) while also removing PyPI as the default index.

---

_Comment by @hongbo-miao on 2024-12-24 09:47_

Wow, thank you so much, @FishAlchemist ! I finally made it. 🎉 Here is the final pyproject.toml

```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==0.10.6"
]

[tool.uv]
package = false
prerelease = "allow"

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git" }
```

```sh
uv venv
uv pip install torch
uv pip install --no-build-isolation git+https://github.com/facebookresearch/detectron2.git
uv sync --dev
```

BTW, index  `https://wheels.myhloli.com` is not necessary. 😃


---

_Closed by @hongbo-miao on 2024-12-24 09:47_

---
