<h1 align="center">
<img src="docs/assets/openvideo-logo2.png" width="600">
</h1><br>


[![HuggingFace](https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-OpenVideo-yellow)](https://huggingface.co/OpenVideo)
[![ModelScope](https://img.shields.io/badge/ModelScope-OpenVideo-blue)](https://www.modelscope.cn/organization/OpenVideo)
[![PyPi version](https://badgen.net/pypi/v/openvideo/)](https://pypi.org/pypi/openvideo/)
[![PyPI pyversions](https://img.shields.io/badge/dynamic/json?color=blue&label=python&query=info.requires_python&url=https%3A%2F%2Fpypi.org%2Fpypi%2Fdata4co%2Fjson)](https://pypi.python.org/pypi/openvideo/) 
[![Downloads](https://static.pepy.tech/badge/openvideo)](https://pepy.tech/project/openvideo)
[![GitHub stars](https://img.shields.io/github/stars/UmiMarch/OpenVideo.svg?style=social&label=Star&maxAge=8640)](https://GitHub.com/UmiMarch/OpenVideo/stargazers/)

OpenVideo focuses on the field of cultural videos, aiming to provide high-quality and diverse video data to AI researchers around the world, and to provide corresponding data collection, cleaning, and annotation tools to help the development of the artificial intelligence industry.


## 📚Dataset


| Source| Specifications| Duration| Items |
| :--------: | :--: | :--: | :---------: |
| Pexels-Raw | 720p | 672h | 106k+ clips |

### Download method:

从[ModelScope](https://www.modelscope.cn/datasets/OpenVideo/pexel-0808-complete-final-test)下载：

```
bash git clone https://user_id:access_token@www.modelscope.cn/datasets/OpenVideo/pexel-0808-complete-final-test.git
```

Download from [huggingface](https://huggingface.co/datasets/OpenVideo/pexel-0808-complete-final-test):

```
bash git clone https://user_id:access_token@huggingface.co/datasets/OpenVideo/pexel-0808-complete-final-test
```

(user_id is the username, access_token needs to be generated in the settings)

### Unzip the script:

```
python ./openvideo/video/preprocess/utils/decode_parquet_file.py --parquet_dir your_parquet_path --save_dir your_save_path
```



## ⚡Tool Description


You can install the stable version using PyPI by typing the following command in the command line:

```bash
$ pip install openvideo
```

Or get the latest version from Github:

```bash
$ pip install -U https://github.com/UmiMarch/OpenVideo/archive/master.zip # with --user for user install (no root)
```

The packages that ``OpenVideo`` depends on are as follows:

```
huggingface_hub>=0.22.2
tqdm>=4.66.1
wget>=3.2
requests>=2.31.0
aiohttp>=3.9.3
async_timeout>=4.0.3
moviepy>=1.0.3
opencv-python>=4.9.0.80
selenium>=4.19.0
scenedetect>=0.6.3
texttable>=1.7.0
bs4>=0.0.2
```

### <u>Video data download</u>

* Mixkit [https://mixkit.co/free-stock-video/](https://mixkit.co/free-stock-video/)
```python
from openvideo.video.fetch import MixkitVideoFetch
 
mixkit_fetch = MixkitVideoFetch(root_dir="your/video/save/path")
mixkit_fetch.download_with_category_page_idx(
    category="sky", # Video type
    page_idx=1, # Which page should I start downloading from?
    start_idx=22, # which video to start downloading from
    platform="linux" # running platform
)
```

* Pixabay [https://pixabay.com/zh](https://pixabay.com/zh)
```python
from openvideo.video.fetch import PixabayVideoFetch

pixabay = PixabayVideoFetch("your/video/save/path")
pixabay.download(
    chrome_exe_path=r"your/chrome/exe/path",
    username="your/pixabay/username",
    password="your/pixabay/password",
    headless=False,
    platform="windows" # Currently only supports windows
)
```

* Pexels [https://www.pexels.com/](https://www.pexels.com/)
```python
from openvideo.video.fetch import PexelsVdieoFetch, PexelsAPI

# The first step is to call the API to get the video link
pexels_api = PexelsAPI(
    api="your/pexels/api", 
    save_path="pexels_api.npy"
)
pexels_api.fetch_api(
    start_page=1, # start page
    end_page=2, # Final page
    save_api_dict_every_pages=1 # Save every few pages
)

# Step 2: Download the video
pexels = PexelsVdieoFetch("pexels")
pexels.download(
    api_npy_save_path="pexels_api.npy", 
    chrome_exe_path=r"your/chrome/exe/path",
    headless=False
)
```

### <u>Video Annotation Platform</u>

We have developed a Rust-based [video annotation platform](https://huggingface.co/spaces/OpenVideo/GPT4o-Azure-Caption-Pixel) that is designed to efficiently generate labels for multiple media such as images and videos. The platform supports calling the most advanced AI models such as GPT-4o, Gemini, Claude3, etc., supports multi-prompt input and flexible configuration options. It is designed with a focus on high performance, capable of processing 100 queries per second and scalable task processing capabilities up to 200 million times. With 100 API accounts, the tool can synthesize a dataset of 200,000 videos in 8 hours. All output content is categorized and organized by model and prompt to ensure a clear structure and facilitate integration with subsequent research and applications.

![image-20250123193428569](./docs/assets/caption_platform.png)

(If you encounter display problems, you can switch to Edge browser to view)



### <u>Annotation Verification Platform</u>

We provide a [video annotation and verification platform](https://huggingface.co/spaces/OpenVideo/AIL-Caption-lalala-Dup), which allows you to view, verify, and modify the annotations of the annotated video dataset on the page.

**How ​​to use:**

1. Open the HuggingFace link (if you encounter display problems, you can switch to Edge browser to view), enter [personal token](https://huggingface.co/settings/tokens)

   
2. Play the video through the annotation platform, view the corresponding annotation text, modify the annotation text and switch to the next video

   ![openvideo_tagger](./docs/assets/openvideo_tagger.png)


**User-defined datasets must meet the following requirements:**

1. The dataset and code are on the same platform (for example, the dataset is hosted on huggingface);

2. Modify the [dataset path]([run.py · OpenVideo/AIL-Caption-lalala-Dup at main](https://huggingface.co/spaces/OpenVideo/AIL-Caption-lalala-Dup/blob/main/run.py#L7)) in the code.



### <u>Data Migration</u>

We provide a general [data migration platform](https://huggingface.co/spaces/OpenVideo/HF_To_MS) for migrating HuggingFace datasets to ModelScope, making it easier to access and use the datasets in networks in different regions.

**How ​​to use:**

Enter the personal token of HuggingFace, the dataset path of HuggingFace, the personal token of ModelScope, and the warehouse directory corresponding to ModelScope. Click Submit to copy the dataset from HuggingFace to the warehouse corresponding to ModelScope in the background.

![data_transfer](./docs/assets/data_transfer.png)



## 👨‍💻 Contributors

Crawler algorithm: @yangming @heatingma @ZZY @晚来风雪

Data source: @yangming @晚来风雪 @杰杰杰

Data cleaning: @一马平川 @zjukop @伊小布

Prompt: @Tiger.C @dpyneo @Chocolate

Model labeling: @YUE @zjukop

Verification platform: @YUE @晚来风雪

Data reflux: @晚来风雪@heatingma

Manual verification: @一马平川@dpyneo @杨嘉昊@flipped @yi @believe @思恩

Project research: @dingby @believe

Aesthetic guidance: @图拉@杨嘉昊

Document: @ZZY @枪枪

Project Coordinator: @Chocolate

## 🙏 Acknowledgements

Server/Funding Support: Li Bai Artificial Intelligence Laboratory

Storage/Overseas Dedicated Line: HuggingFace, ModelScope, OPENDataLab

Share and exchange: @shoulder @王铁震 @杨欢 @新年京

Join the discussion: @Fadeaway Jump Shot @Floating Feather @MYX @Winniy @GUI @Planet

## ✨ Share and exchange

![connect](./docs/assets/connect.png)

## ©️ License Agreement

The project complies with the [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/deed.zh-hans) open source agreement.


