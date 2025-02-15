## h2oGPT integration with LangChain and Chroma/FAISS/Qdrant/Weaviate for Vector DB

Our goal is to make it easy to have private offline document question-answer using LLMs.

## Get Started

Follow the [get started steps](../README.md#get-started) in the main README.  In this readme, we focus on other optional aspects.

To support GPU FAISS database, run:
```bash
pip install -r reqs_optional/requirements_optional_gpu_only.txt
```
or for CPU FAISS database, run:
```bash
pip install -r reqs_optional/requirements_optional_cpu_only.txt
```

or for Qdrant/Weaviate, run:
```bash
pip install -r reqs_optional/requirements_optional_langchain.txt
```
## Supported Data types

Open-source data types are supported, .msg is not supported due to GPL-3 requirement.  Other meta types support other types inside them.  Special support for some behaviors is provided by the UI itself.

### Supported Native Data types

   - `.pdf`: Portable Document Format (PDF),
   - `.txt`: Text file (UTF-8),
   - `.csv`: CSV,
   - `.toml`: TOML,
   - `.py`: Python,
   - `.rst`: reStructuredText,
   - `.rtf`: Rich Text Format,
   - `.md`: Markdown,
   - `.html`: HTML File,
   - `.mhtml`: MHTML File,
   - `.htm`: HTML File,
   - `.docx`: Word Document (optional),
   - `.doc`: Word Document (optional),
   - `.xlsx`: Excel Document (optional),
   - `.xls`: Excel Document (optional),
   - `.enex`: EverNote,
   - `.eml`: Email,
   - `.epub`: EPub,
   - `.odt`: Open Document Text,
   - `.pptx` : PowerPoint Document,
   - `.ppt` : PowerPoint Document,
   - `.xml`: XML,

   - `.apng` : APNG Image (optional),
   - `.blp` : BLP Image (optional),
   - `.bmp` : BMP Image (optional),
   - `.bufr` : BUFR Image (optional),
   - `.bw` : BW Image (optional),
   - `.cur` : CUR Image (optional),
   - `.dcx` : DCX Image (optional),
   - `.dds` : DDS Image (optional),
   - `.dib` : DIB Image (optional),
   - `.emf` : EMF Image (optional),
   - `.eps` : EPS Image (optional),
   - `.fit` : FIT Image (optional),
   - `.fits` : FITS Image (optional),
   - `.flc` : FLC Image (optional),
   - `.fli` : FLI Image (optional),
   - `.fpx` : FPX Image (optional),
   - `.ftc` : FTC Image (optional),
   - `.ftu` : FTU Image (optional),
   - `.gbr` : GBR Image (optional),
   - `.gif` : GIF Image (optional),
   - `.grib` : GRIB Image (optional),
   - `.h5` : H5 Image (optional),
   - `.hdf` : HDF Image (optional),
   - `.icb` : ICB Image (optional),
   - `.icns` : ICNS Image (optional),
   - `.ico` : ICO Image (optional),
   - `.iim` : IIM Image (optional),
   - `.im` : IM Image (optional),
   - `.j2c` : J2C Image (optional),
   - `.j2k` : J2K Image (optional),
   - `.jfif` : JFIF Image (optional),
   - `.jp2` : JP2 Image (optional),
   - `.jpc` : JPC Image (optional),
   - `.jpe` : JPE Image (optional),
   - `.jpeg` : JPEG Image (optional),
   - `.jpf` : JPF Image (optional),
   - `.jpg` : JPG Image (optional),
   - `.jpx` : JPX Image (optional),
   - `.mic` : MIC Image (optional),
   - `.mpeg` : MPEG Image (optional),
   - `.mpg` : MPG Image (optional),
   - `.msp` : MSP Image (optional),
   - `.pbm` : PBM Image (optional),
   - `.pcd` : PCD Image (optional),
   - `.pcx` : PCX Image (optional),
   - `.pgm` : PGM Image (optional),
   - `.png` : PNG Image (optional),
   - `.pnm` : PNM Image (optional),
   - `.ppm` : PPM Image (optional),
   - `.ps` : PS Image (optional),
   - `.psd` : PSD Image (optional),
   - `.pxr` : PXR Image (optional),
   - `.qoi` : QOI Image (optional),
   - `.ras` : RAS Image (optional),
   - `.rgb` : RGB Image (optional),
   - `.rgba` : RGBA Image (optional),
   - `.sgi` : SGI Image (optional),
   - `.tga` : TGA Image (optional),
   - `.tif` : TIF Image (optional),
   - `.tiff` : TIFF Image (optional),
   - `.vda` : VDA Image (optional),
   - `.vst` : VST Image (optional),
   - `.webp` : WEBP Image (optional),
   - `.wmf` : WMF Image (optional),
   - `.xbm` : XBM Image (optional),
   - `.xpm` : XPM Image (optional).

   - `.mp4` : MP4 Audio (optional).
   - `.mpeg` : MP4-based MPEG Audio (optional).
   - `.mpg` : MP4-based MPG Audio (optional).
   - `.mp3` : MP3 Audio (optional).
   - `.ogg` : OGG Audio (optional).
   - `.flac` : FLAC Audio (optional).
   - `.aac` : AAC Audio (optional).
   - `.au` : AU Audio (optional).


### Supported Meta Data types

   - `.zip` : Zip File containing any native datatype.
   - `.urls` : Text file containing new-line separated URLs (to be consumed via download).

Note: If you upload files and one of the files is a zip that contains images to be read by Florence-2/DocTR or PDFs to be read by DocTR, this will currently fail with:
```text
Cannot re-initialize CUDA in forked subprocess. To use CUDA with multiprocessing, you must use the 'spawn' start method
```
Please upload the zip separately for now.

### Supported Data Types in UI

   - `Files` : All Native and Meta Data Types as file(s),
   - `URL` : Any URL (i.e. `http://` or `https://`),
   - `ArXiv` : Any ArXiv name (e.g. `arXiv:1706.03762`),
   - `Text` : Paste Text into UI.

### Supported Meta Tasks

   - `ScrapeWithPlayWRight` : Async Web Scraping using headless Chromium via PlayWright
   - `ScrapeWithHttp` : Async Web Scraping using aiohttp (slower than PlayWright)

* Timing
  * Typical page like passing `https://github.com/h2oai/h2ogpt` takes about 300 seconds to process at a default depth of 1 with about 140 pages.
  * No good progress indicators from these packages, so just have to wait.
* Depth:
  * Set env `CRAWL_DEPTH=<depth>` to control depth for some integer `<depth>`, where 0 means only the actual page, 1 means that page + all links on that page, etc.  `CRAWL_DEPTH=1` by default to avoid excessive crawling.
  * Set env `ALL_CRAWL_DEPTH=<depth>` to force all url loaders to crawl at some depth (will be slower than async ones)
* BS4:
  * Set env `HTML_TRANS=BS4` to use `BS4` to transform instead of `Html2TextTransformer`.  Set `BS4_TAGS` env to some string of list to set [tags](https://python.langchain.com/docs/use_cases/web_scraping#quickstart).
    * e.g. `export BS4_TAGS="['span']"`
  * Scrape text content tags such as `<p>`, `<li>`, `<div>`, and `<a>` tags from the HTML content:
    * `<p>`: The paragraph tag. It defines a paragraph in HTML and is used to group related sentences and/or phrases.
    * `<li>`: The list item tag. It is used within ordered (`<ol>`) and unordered (`<ul>`) lists to define individual items within the list.
    * `<div>`: The division tag. It is a block-level element used to group other inline or block-level elements.
    * `<a>`: The anchor tag. It is used to define hyperlinks.
    * `<span>`: an inline container used to mark up a part of a text, or a part of a document.
  For many news websites (e.g., WSJ, CNN), headlines and summaries are all in `<span>` tags.
* ScrapeWithHttp:
  * Can change code in src/gpt_langchain.py to change `requests_per_second=10` to some other value.

### Adding new file types

The function `file_to_doc` controls the ingestion, with [allowed ones listed](https://github.com/h2oai/h2ogpt/blob/1184f057088743599e2d5241329551b8f7f5320d/src/gpt_langchain.py#L1021-L1035).   If one wants to add a new file type, add it to the list `file_types`, and then add an entry in `file_to_doc()` function.

Metadata is added using `add_meta` function, and other metadata, like chunk_id, is added after chunking.  One could add a new step to add metadata to `page_content` to each langchain `Document`.

## Database creation

To use some example databases (will overwrite UserData make above unless change options) and run generate after, do:
```bash
python src/make_db.py --download_some=True
python generate.py --base_model=HuggingFaceH4/zephyr-7b-beta --langchain_mode=UserData --langchain_modes="['UserData', 'wiki', 'MyData', 'github h2oGPT', 'DriverlessAI docs']"
```
which downloads example databases.  This obtains files from some [pre-generated databases](https://huggingface.co/datasets/h2oai/db_dirs).  A large Wikipedia database is also available.

To build the database first outside chatbot, then run generate after, do:
```bash
python src/make_db.py
python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b --langchain_mode=UserData
```

To add data to the existing database, then run generate after, do:
```bash
python src/make_db.py --add_if_exists=True
python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b --langchain_mode=UserData
```

By default, `generate.py` will load an existing UserData database and add any documents added to user_path or change any files that have changed.  To avoid detecting any new files, just avoid passing --user_path=user_path, which sets it to None, i.e.:
```bash
python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b --langchain_mode=UserData
```
which will avoid using `user_path` since it is no longer passed.  Otherwise, when passed, any new files will be added or changed (by hash) files will be updated (delete old sources and add new sources).

If you have enough GPU memory for embedding, but not the LLM as well, then a less private mode is to use the OpenAI model.
```bash
python generate.py  --inference_server=openai_chat --base_model=gpt-3.5-turbo --score_model=None --langchain_mode=LLM --langchain_modes="['LLM', 'UserData', 'MyData']"
```
and if you want to push the image caption model to get better captions, this can be done if have enough GPU memory or if use OpenAI:
```bash
python generate.py  --inference_server=openai_chat --base_model=gpt-3.5-turbo --score_model=None --langchain_mode=LLM --langchain_modes="['LLM', 'UserData', 'MyData']" --captions_model=microsoft/Florence-2-large
```
Similar commands can be used for Azure OpenAI, e.g.
```bash
OPENAI_API_KEY=<key> python generate.py --inference_server="openai_azure_chat:<deployment_name>:<base_url>:<api_version>" --base_model=gpt-3.5-turbo --h2ocolors=False --langchain_mode=UserData
```

To speed-up ingestion of PDFs (skip complex PDFs that fail with pymupdf) and to use faster embedding model, can run differently.  Can also use docker to avoid installing dependencies:
```bash
mkdir -p ~/.cache
mkdir -p ~/save
mkdir -p ~/user_path
mkdir -p ~/db_dir_UserData
docker run \
       --gpus all \
       --runtime=nvidia \
       --shm-size=2g \
       --rm --init \
       --network host \
       -v /etc/passwd:/etc/passwd:ro \
       -v /etc/group:/etc/group:ro \
       -u `id -u`:`id -g` \
       -v "${HOME}"/.cache:/workspace/.cache \
       -v "${HOME}"/save:/workspace/save \
       -v "${HOME}"/user_path:/workspace/user_path \
       -v "${HOME}"/db_dir_UserData:/workspace/db_dir_UserData \
       gcr.io/vorvan/h2oai/h2ogpt-runtime:0.2.1 /workspace/src/make_db.py --verbose --use_unstructured_pdf=False --enable_pdf_ocr=False --hf_embedding_model=BAAI/bge-small-en-v1.5 --cut_distance=10000
```
This will consume about 100 PDFs per minute on average, and embedding part takes about 5 minutes for 300 PDFs.  For multilingual, use `BAAI/bge-m3` that uses more memory, so you may need to set ENV `CHROMA_MAX_BATCH_SIZE=1` or similar values to avoid GPU OOM.


### Multiple embeddings and sources

We only support one embedding at a time for each database.

So you could use src/make_db.py to make the DB for different embeddings (`--hf_embedding_model` like gen.py, any HF model) for each collection (e.g. UserData, UserData2) for each source folders (e.g. user_path, user_path2), and then at generate.py time you can specify those different collection names in `--langchain_modes` and `--langchain_modes` and `--langchain_mode_paths`.  For example:
```bash
python src/make_db.py --user_path=user_path --collection_name=UserData --langchain_type=shared --hf_embedding_model=hkunlp/instructor-large
python src/make_db.py --user_path=user_path2 --collection_name=UserData2 --langchain_type=shared --hf_embedding_model=sentence-transformers/all-MiniLM-L6-v2
```
Note that `shared` is the default type already, but we show above to show what options are relevant if want to change them.
Then run:
```bash
python generate.py --base_model='llama' --prompt_type=llama2 --score_model=None --langchain_mode='UserData' --langchain_modes=['UserData','UserData2'] --langchain_mode_paths={'UserData':'user_path','UserData2':'user_path2'} --langchain_mode_types={'UserData':'shared','UserData2':'shared'} --model_path_llama=https://huggingface.co/TheBloke/Llama-2-7b-Chat-GGUF/resolve/main/llama-2-7b-chat.Q6_K.gguf --max_seq_len=4096
```
or choose 13B.  And watch out for the use of whitespace.  For `langchain_mode_paths` you can pass surrounded by "'s and have spaces.

### Per-User DataBase

See discussion [here](https://github.com/h2oai/h2ogpt/issues/1550#issuecomment-2059793978).

E.g. a folder might already have some databases, like for user *jon* be:
```text
(h2ogpt) jon@pseudotensor:~/h2ogpt$ ls -alrt users/jon/
total 84
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_yuppy/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_xxx/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_testsum1/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_feefef/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_dudedata/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_dogdata1/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_dogdata/
drwx------   2 jon jon  4096 Apr  8 01:49 db_dir_aaaaa/
drwx------  12 jon jon  4096 Apr  8 02:11 ./
drwx------   3 jon jon  4096 Apr  8 02:12 db_dir_asdfasdf/
drwx------   3 jon jon  4096 Apr  9 08:44 db_dir_MyData/
drwx------ 431 jon jon 36864 Apr 16 11:20 ../
```
for personal collections.

To make a new one for the user, fill `user_path_jon` with documents (can be soft or hard linked to avoid dups across multiple users), do:
```bash
python src/make_db.py --user_path=user_path_jon --collection_name=JonData --langchain_type=personal --hf_embedding_model=hkunlp/instructor-large --persist_directory=users/jon/db_dir_JonData
```

Then you'll have:
```text
(h2ogpt) jon@pseudotensor:~/h2ogpt$ ls -alrt users/jon/db_dir_JonData/
total 264
drwx------ 13 jon jon   4096 Apr 16 12:28 ../
drwx------  2 jon jon   4096 Apr 16 12:28 d7ccacb6-93fe-4380-9340-b7f5edffb655/
-rw-------  1 jon jon 249856 Apr 16 12:28 chroma.sqlite3
-rw-------  1 jon jon     41 Apr 16 12:28 embed_info
drwx------  3 jon jon   4096 Apr 16 12:28 ./
```

You can add that database to the `auth.json` for their entry if using `auth.json` type file, and they will see when they login.

Or you can have the user add that collection by name (JonData).  i.e. In *Document Selection* they would go to *Add Collection* and enter `JonData, personal`.  A path could be added if you want them to be able to add to the path, else avoid.  After hitting enter they will see the collection and it will become the default with the documents you added tot he database.

### Choosing document types

```python
import sys
sys.path.append('src')
from src.gpt_langchain import get_supported_types
non_image_types, image_types, video_types = get_supported_types()
print(non_image_types)
print(image_types)
```
Select types, and pass to `make_db` like:
```bash
python src/make_db.py --user_path="/home/jon/Downloads/demo_data" --collection_name=VAData --enable_pdf_ocr='off' --selected_file_types="['pdf', 'html', 'htm']"
python generate.py  --base_model='llama' --prompt_type=llama2 --score_model=None --langchain_mode=VAData --langchain_modes=['VAData'] --model_path_llama=https://huggingface.co/TheBloke/Llama-2-7b-Chat-GGUF/resolve/main/llama-2-7b-chat.Q6_K.gguf --max_seq_len=4096
```
or choose 13B.

To ensure a collection is persisted even when not using any authentication, be sure it is shared type, e.g.:
```bash
python generate.py --base_model='llama' --prompt_type=llama2 --score_model=None --max_max_new_tokens=2048 --max_new_tokens=1024 \
       --visible_tos_tab=False --visible_hosts_tab=False --visible_models_tab=False \
       --langchain_modes="['LLM','PersistData']" --langchain_mode=PersistData \
       --langchain_mode_types="{'PersistData':'shared'}" \
       --top_k_docs=-1 --max_time=360 --save_dir=save \
       --model_path_llama=https://huggingface.co/TheBloke/Llama-2-7b-Chat-GGUF/resolve/main/llama-2-7b-chat.Q6_K.gguf \
       --max_seq_len=4096
```
or choose 13B.


### Personal collections with make_db

* --collection_mame must match --persist_directory if both provided
* Temporary users cannot have a personal databases craeted by make_db since those all uses hashes, so one must at least login or use auth etc.
* So, ensure you at least login so your personal directories look like `users/<username>/db_dir_<collection_name>`.

Example sequence:

1. Run make_db ensuring collection name matches persist directory and `users/<user>` path matches the expected persistent user name.
```
python src/make_db.py --collection_name=duck --user_path=user_path_test --langchain_type=personal --persist_directory=users/tomer/db_dir_duck/
```

2. Run without "tomer" in langchain_mode, because personal collections are for a single user, not specified at CLI time but stored in the auth database.
```
python generate.py --base_model=https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF/resolve/main/zephyr-7b-beta.Q2_K.gguf --use_safetensors=True --prompt_type=zephyr --save_dir='save2' --use_gpu_id=False --user_path=user_path_test --langchain_mode="LLM" --langchain_modes="['UserData', 'LLM']" --score_model=None --add_disk_models_to_ui=False
```

3. Login as user "tomer"

![image](https://github.com/user-attachments/assets/51241c90-f262-421c-87f9-c7f8c09d48e3)

4. Add the collection:

![image](https://github.com/user-attachments/assets/8b78fc2e-6375-47d6-8836-143a8f3b907e)

5. Then you'll see the "Directory" be correct:

![image](https://github.com/user-attachments/assets/f36281cd-6237-4027-a250-362ecb7ef59f)

6. You'll see your docs when choosing the duck collection:

![image](https://github.com/user-attachments/assets/f1720238-ec2c-4db8-971b-2e1b4ef03195)

### Note about Embeddings

The default embedding for GPU is `instructor-large` since most accurate, however, it leads to excessively high scores for references due to its flat score distribution.  For CPU the default embedding is `all-MiniLM-L6-v2`, and it has a sharp distribution of scores, so references make sense, but it is less accurate.

### Note about FAISS

FAISS filtering is not supported in h2oGPT yet, ask if this is desired to be added.  So subset by document does not function for FAISS.

### Using Weaviate

#### About

[Weaviate](https://weaviate.io/) is an open-source vector database designed to scale seamlessly into billions of data objects. This implementation supports hybrid search out-of-the-box (meaning it will perform better for keyword searches).

You can run Weaviate in 5 ways:

- **SaaS** – with [Weaviate Cloud Services (WCS)](https://weaviate.io/pricing).

  WCS is a fully managed service that takes care of hosting, scaling, and updating your Weaviate instance. You can try it out for free with a sandbox that lasts for 14 days.

  To set up a SaaS Weaviate instance with WCS:

  1.  Navigate to [Weaviate Cloud Console](https://console.weaviate.cloud/).
  2.  Register or sign in to your WCS account.
  3.  Create a new cluster with the following settings:
      - `Subscription Tier` – Free sandbox for a free trial, or contact [hello@weaviate.io](mailto:hello@weaviate.io) for other options.
      - `Cluster name` – a unique name for your cluster. The name will become part of the URL used to access this instance.
      - `Enable Authentication?` – Enabled by default. This will generate a static API key that you can use to authenticate.
  4.  Wait for a few minutes until your cluster is ready. You will see a green tick ✔️ when it's done. Copy your cluster URL.

- **Hybrid SaaS**

  > If you need to keep your data on-premise for security or compliance reasons, Weaviate also offers a Hybrid SaaS option: Weaviate runs within your cloud instances, but the cluster is managed remotely by Weaviate. This gives you the benefits of a managed service without sending data to an external party.

  The Weaviate Hybrid SaaS is a custom solution. If you are interested in this option, please reach out to [hello@weaviate.io](mailto:hello@weaviate.io).

- **Self-hosted** – with a Docker container

  To set up a Weaviate instance with Docker:

  1. [Install Docker](https://docs.docker.com/engine/install/) on your local machine if it is not already installed.
  2. [Install the Docker Compose Plugin](https://docs.docker.com/compose/install/)
  3. Download a `docker-compose.yml` file with this `curl` command:

```bash
curl -o docker-compose.yml "https://configuration.weaviate.io/v2/docker-compose/docker-compose.yml?modules=standalone&runtime=docker-compose&weaviate_version=v1.19.6"
```

     Alternatively, you can use Weaviate's docker compose [configuration tool](https://weaviate.io/developers/weaviate/installation/docker-compose) to generate your own `docker-compose.yml` file.

4. Run `docker compose up -d` to spin up a Weaviate instance.

     > To shut it down, run `docker compose down`.

- **Self-hosted** – with a Kubernetes cluster

  To configure a self-hosted instance with Kubernetes, follow Weaviate's [documentation](https://weaviate.io/developers/weaviate/installation/kubernetes).|

- **Embedded** - start a Weaviate instance right from your application code using the client library
   
  This code snippet shows how to instantiate an embedded Weaviate instance and upload a document:

```python
  import weaviate
  from weaviate.embedded import EmbeddedOptions

  client = weaviate.Client(
    embedded_options=EmbeddedOptions()
  )

  data_obj = {
    "name": "Chardonnay",
    "description": "Goes with fish"
  }

  client.data_object.create(data_obj, "Wine")
```
  
  Refer to the [documentation](https://weaviate.io/developers/weaviate/installation/embedded) for more details about this deployment method.
## How To Use
Simply pass the `--db_type=weaviate` argument. For example:
```bash
python src/make_db.py --db_type=weaviate
python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b \
   --langchain_mode=UserData \
   --db_type=weaviate
```
will use an embedded Weaviate instance.

If you have a Weaviate instance hosted at say http://localhost:8080, then you need to define the `WEAVIATE_URL` environment variable before running the scripts:
```
WEAVIATE_URL=http://localhost:8080 python src/make_db.py --db_type=weaviate
WEAVIATE_URL=http://localhost:8080 python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b \
   --langchain_mode=UserData \
   --db_type=weaviate
```

Similarly, if you had set up your Weaviate instance with a username and password using the [OIDC Resource Owner Password flow](https://weaviate.io/developers/weaviate/configuration/authentication#oidc---a-client-side-perspective), you will need to define the following additional environment variables:
* WEAVIATE_USERNAME: the username used for authentication
* WEAVIATE_PASSWORD: the password used for authentication
* WEAVIATE_SCOPE: optional, defaults to "offline_access"

Notes:

* Since h2oGPT is focused on privacy, connecting to Weaviate via WCS is not supported as that will expose your data to a 3rd party
* Weaviate doesn't know about persistent directories throughout code and maintains locations based on the collection name
* Weaviate doesn't support query of all metadata except via similarity search up to 10k documents, so a full list of sources is not possible in h2oGPT UI for `Update UI with Document(s) from DB` or `Show Sources from DB`

### Using Qdrant

#### About
[Qdrant](https://qdrant.tech/) is an open-source, high-performance vector search engine/database. It is built with Rust for large data on a billion scale.

You can find installation instructions in the Qdrant [documentation](https://qdrant.tech/documentation/guides/installation/).

#### Usage

Set the `db_type` option value to `qdrant`:

```bash
python src/make_db.py --db_type=qdrant
python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b \
   --langchain_mode=UserData \
   --db_type=qdrant
```

Qdrant's Python client also supports in-memory instances for prototyping, which is the default in H2OGPT.

You can use environment variables to configure your Qdrant connection. For example:

```
QDRANT_URL=http://localhost:8080 QDRANT_API_KEY="<YOUR_KEY>" python src/make_db.py --db_type=qdrant
QDRANT_URL=http://localhost:8080 QDRANT_API_KEY="<YOUR_KEY>" python generate.py --base_model=h2oai/h2ogpt-oig-oasst1-512-6_9b \
   --langchain_mode=UserData \
   --db_type=qdrant
```

The available configurations are:

| ENV name           | Description                                                                                                                                        |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| QDRANT_URL         | Either host or a fully qualified URL. Eg. `http://localhost:6333`                                                                                  |
| QDRANT_PORT        | Port of the REST API interface. Default: `6333`                                                                                                    |
| QDRANT_GRPC_PORT   | Port of the gRPC interface. Default: `6334`                                                                                                        |
| QDRANT_PREFER_GPRC | If `true` - use the gRPC interface whenever possible in custom methods.                                                                                |
| QDRANT_HTTPS       | If `true` - use HTTPS(SSL) protocol.                                                                                                               |
| QDRANT_API_KEY     | API key for authentication in Qdrant Cloud.                                                                                                        |
| QDRANT_PREFIX      | If set, add `prefix` to the REST URL path. Example: `service/v1` will result in `http://localhost:6333/service/v1/{qdrant-endpoint}` for REST API. |
| QDRANT_TIMEOUT     | Timeout for REST and gRPC API requests. Default: 5.0 seconds for REST and unlimited for gRPC                                                       |
| QDRANT_HOST        | Host name of Qdrant service. If url and host are not set, defaults to 'localhost'.                                                                 |
| QDRANT_PATH        | Persistence path for QdrantLocal. Eg. `h2o_data/qdrant`                                                                                            |


## Document Question-Answer FAQ

### What is UserData and MyData?

UserData: Shared with anyone who is on your server. Persisted across sessions in a single location for the entire server. Control upload via allow_upload_to_user_data option.  Useful for collaboration.

MyData: Personal space inaccessible if one goes into a new browser session. Useful for public demonstrations so that every instance is independent. It is useful if the user cannot upload to shared UserData and wants to do Q&A.

It's a work in progress to add other persistent databases and to have MyData persisted across browser sessions via cookie or other authentication.

#### Why does the source link not work?

For links to direct to the document and download to your local machine, the source documents must still be present on the host system where the database was created, e.g. `user_path` for `UserData` by default.  If the database alone is copied somewhere else, that host won't have access to the documents.  URL links like Wikipedia will still work normally on any host.


#### What is h2oGPT's LangChain integration like?

* [PrivateGPT](https://github.com/imartinez/privateGPT) .  By comparison, h2oGPT has:
  * UI with chats export, import, selection, regeneration, and undo
  * UI and document Q/A, upload, download, and list
  * Parallel ingest of documents, using GPUs if present for vector embeddings, with progress bar in stdout
  * Choose which specific collection
  * Choose to get a response regarding all documents or specifically selected document(s) out of a collection
  * Choose to chat with LLM, get a one-off LLM response to a query, or talk to a collection
  * GPU support from any hugging face model for the highest performance
  * Upload many types of docs, from PDFs to images (caption or OCR), URLs, ArXiv queries, or just plain text inputs
  * Server-Client API through Gradio client
  * RLHF score evaluation for every response
  * UI with side-by-side model comparisons against two models at a time with independent chat streams
  * Fine-tuning framework with QLORA 4-bit, 8-bit, 16-bit GPU fine-tuning or CPU fine-tuning

* [localGPT](https://github.com/PromtEngineer/localGPT).  By comparison, h2oGPT has similar benefits as compared to localGPT.  Both h2oGPT and localGPT can use GPUs for LLMs and embeddings, including the latest Vicuna or WizardLM models.

* [Quiver](https://github.com/StanGirard/quivr). By comparison, Quiver requires docker but also supports audio and video and currently only supports OpenAI models and embeddings.

* [LM Studio](https://github.com/lmstudio-ai). Nice control over models and llama settings, good Windows installer.

* [DocsGPT](https://github.com/arc53/DocsGPT).  More limited document support.

* [GPT4-PDF-Chatbot-LangChain](https://github.com/mayooear/gpt4-pdf-chatbot-langchain).  Uses OpenAI, pinecone, etc. No longer maintained.

* [Vault-AI](https://github.com/pashpashpash/vault-ai) but h2oGPT is fully private and open-source by not using OpenAI or [pinecone](https://www.pinecone.io/).

* [DB-GPT](https://github.com/csunny/DB-GPT) but h2oGPT is fully commercially viable by not using [Vicuna](https://lmsys.org/blog/2023-03-30-vicuna/) (LLaMa based with GPT3.5 training data).

* [ChatBox](https://github.com/Bin-Huang/chatbox) has ability to collaborate.

* [Chat2DB](https://github.com/alibaba/Chat2DB) like DB-GPT by Alibaba.

* [pdfGPT](https://github.com/bhaskatripathi/pdfGPT) like PrivateGPT but no longer maintained.

* [docquery](https://github.com/impira/docquery) like PrivateGPT but uses LayoutLM.

* [KhoJ](https://github.com/khoj-ai/khoj) but also access from emacs or Obsidian.

* [ChatPDF](https://www.chatpdf.com/) but h2oGPT is open-source and private and many more data types.

* [TryGloo](https://www.trygloo.com/) Semantic Search and Classification.

* [Cube](https://cube.dev/blog/introducing-the-langchain-integration).

* [RFPBot](https://www.datarobot.com/platform/generative-ai/).  Confidence score, slack integration.

* [Sharly](https://www.sharly.ai/) but h2oGPT is open-source and private and many more data types.  Sharly and h2oGPT both allow sharing work through UserData shared collection.

* [ChatDoc](https://chatdoc.com/) but h2oGPT is open-source and private. ChatDoc shows a nice side-by-side view with the doc on one side and chat on the other.  Select a specific doc or text in the doc for question/summary.

* [Casalioy](https://github.com/su77ungr/casalioy) with a focus on air-gap with docker, otherwise like older privateGPT.

* [Perplexity](https://www.perplexity.ai/) but h2oGPT is open-source and private, with similar control over sources.

* [HayStack](https://github.com/deepset-ai/haystack) but h2oGPT is open-source and private.  Haystack is pivoted to LLMs from NLP tasks, so well-developed documentation etc.  But mostly LangChain clones.

* [Empler](https://www.empler.ai/) but h2oGPT is open-source and private.  Empler has nice AI and content control and focuses on use cases like marketing.

* [Writesonic](https://writesonic.com/) but h2oGPT is open-source and private.  Writesonic has better image/video control.

* [HuggingChat](https://huggingface.co/chat/) Not for commercial use, uses LLaMa and GPT3.5 training data, so violates ToS.

* [Bard](https://bard.google.com/) but h2oGPT is open-source and private.  Bard has better automatic link and image use.

* [ChatGPT](https://chat.openai.com/) but h2oGPT is open-source and private.  ChatGPT code interpreter has better image, video, etc. handling.

* [ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web) like local ChatGPT.

* [Bing](https://www.bing.com/) but h2oGPT is open-source and private.  Bing has excellent search queries and handling of results.

* [Bearly](https://bearly.ai/) but h2oGPT is open-source and private.  Bearly focuses on creative content creation.

* [Poe](https://poe.com/) but h2oGPT is open-source and private.  Poe also has an immediate info wall requiring a phone number.

* [WiseOne](https://wiseone.io/) but h2oGPT is open-source and private.  WiseOne is a reading helper.

* [Poet.ly or Aify](https://aify.co/) but h2oGPT is open-source and private.  Poet.ly focuses on writing articles.

* [PDFGPT.ai](https://pdfgpt.io/) but h2oGPT is open-source and private.  Only PDF and on the expensive side.

* [BratGPT](https://bratgpt.com/) but h2oGPT is open-source and private.  Focuses on uncensored chat.

* [Halist](https://halist.ai/) but h2oGPT is open-source and private.  Uses ChatGPT but does not store chats, but can already do that now with ChatGPT.

* [UltimateGPT Toolkit](https://play.google.com/store/apps/details?id=com.neuralminds.ultimategptoolkit&ref=producthunt&pli=1) Android plugin for ChatGPT.

* [Intellibar](https://intellibar.app/) ChatGPT on iPhone.

* [GPTMana](https://play.google.com/store/apps/details?id=com.chatgpt.gptmana) Android Plugin.

* [Genie](https://www.genieai.co/) but h2oGPT is open-source and private.  Focuses on legal assistant.

* [ResearchAI](https://research-ai.io/) but h2oGPT is open-source and private.  Focuses on research helper with tools.

* [ChatOn](https://apps.apple.com/us/app/chaton) but h2oGPT is open-source and private.  ChatOn focuses on mobile, iPhone app.

* [Ask](https://iask.ai/) but h2oGPT is open-source and private.  Similar content control.

* [Petey](https://apps.apple.com/us/app/petey-ai-assistant/id6446047813) but h2oGPT is open-source and private.  Apple Watch.

* [QuickGPT](https://www.quickgpt.io/) but h2oGPT is open-source and private.  QuickGPT is ChatGPT for Whatsapp.

* [Raitoai](https://www.raitoai.com/) but h2oGPT is open-source and private.  Raito.ai focuses on helping writers.

* [AIChat](https://deepai.org/chat) but h2oGPT is open-source and private.  Heavy on ads, avoid.

* [AnonChatGPT](https://anonchatgpt.com/) but h2oGPT is open-source and private.  Anonymous use of ChatGPT, i.e. no account required.

* [GPTPro](https://play.google.com/store/apps/details?id=com.dfmv.gptpro&hl=en_US&gl=US) but h2oGPT is open-source and private.  GPTPro focuses on Android.

* [Rio](https://www.oziku.tech/rio-openai-chatgpt-assistant) but h2oGPT is open-source and private.  Browser-based assistant.

* [CommanderGPT](https://www.commandergpt.app/) but h2oGPT is open-source and private.  CommanderGPT focuses on MAC with a few tasks like image generation, translation, YouTube query, etc.

* [ThreeSigma](https://www.threesigma.ai/) but h2oGPT is open-source and private.  Focuses on research tools, and nice page linking.

* [LocalAI](https://github.com/go-skynet/LocalAI) but h2oGPT has document question/answer.  LocalAI has audio transcription, image generation, and a variety of models.

* [LocalLLaMa](https://github.com/jlonge4/local_llama) but h2oGPT has UI and GPU support. LocalLLaMa is command-line focused.  Like privateGPT.

* [ChartGPT](https://www.chartgpt.dev/) Focus on drawing charts.
