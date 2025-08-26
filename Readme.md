# Image Embeddings with OpenVINO Model Server {ovms_image_embeddings}

Image-to-image search system using vision models (CLIP, LAION, SigLIP) for generating semantic embeddings with OpenVINO Model Server. The client uploads query images and receives similar images from a pre-indexed dataset based on visual content similarity. This enables applications to find visually and semantically related images without requiring text descriptions or manual tagging. The system uses Python code for preprocessing and MediaPipe graphs for optimized inference execution.




## Build image

```bash
git clone https://github.com/openvinotoolkit/model_server.git
cd model_server
```

If you want the docker image that supports CPU, run:

```bash
make release_image
```

Else if you want the docker image that supports iGPU's, run:
```bash
make release_image GPU=1
```

# Project Architecture

1. **Model Conversion (`model_conversion/`)**
   - Convert supported multimodal models (e.g., CLIP, Laion, SigLIP) into **OpenVINO IR format**.
   - Ensures models are optimized for inference on Intel hardware.

2. **Servable Pipeline (`servable/`)**
   - **Preprocessing**: Handles image resizing, normalization(if required).
   - **Postprocessing**: Handles embedding extraction, vector normalization, and formatting.
   - **Config File**: `config_model.json` defines model parameters and pipeline configurations.
   - **MediaPipe Graphs**: Graph definitions for processing inputs/outputs across the 3 models.

3. **gRPC CLI (`grpc_cli.py`)**
   - Iterates over a folder of images.
   - Extracts embeddings using the OpenVINO-served models.
   - Stores embeddings in a **Vector Database** (Qdrant)

4. **Search API (`search_images.py`)**
   - Accepts an input query image.
   - Generates its embedding and queries the Vector DB.
   - Returns the most similar images based on cosine similarity or other distance metrics.

5. **Search App (`streamlit_app.py`)**
   - Provided a frontend for the users to interact with the project and test it
   - Allows users to upload images and perform semantic search


## Installaion and setup

```bash
cd demos/python_demos/image_embeddings
python3 -m venv venv
pip install -r requirements.txt
```


## Model conversion
```bash
python3 -m venv venv
cd demos/python_demos/image_embeddings/model_conversion
```

For clip
```bash
python clip_conversion.py
```

For laion
```bash
python laion_conversion.py
```

For siglip
```bash
python siglip_conversion.py
```

For Dino
```bash
python dino_conversion.py
```

## Deploying OpenVINO Model Server with the CLIP graph
Prerequisites:
-  image of OVMS with Python support and Optimum installed
Mount the `./servable` which contains:
- `post.py` and `pre.py` - python scripts which are required for execution.
- `config_model.json` - which defines which servables should be loaded.
- `graph_clip.pbtxt`, `graph_laion.pbtxt`, `graph_siglip.pbtxt`, `graph_dino.pbtxt` - which defines MediaPipe graph containing python nodes.


```bash
cd demos/python_demos/image_embeddings
```


To use CPU
```bash
docker run -it --rm \
-p 9000:9000 -p 8000:8000 \
-v ${PWD}/servable:/workspace \
-v ${PWD}/model_conversion/saved_mod/siglip:/saved_mod/siglip \
-v ${PWD}/model_conversion/saved_mod/clip:/saved_mod/clip \
-v ${PWD}/model_conversion/saved_mod/laion:/saved_mod/laion \
openvino/model_server:py \
--config_path /workspace/config_model.json \
--port 9000 --rest_port 8000
```

To use GPU
```bash
docker run -it --rm --device=/dev/dxg --volume /usr/lib/wsl:/usr/lib/wsl -p 9000:9000 -p 8000:8000 -v ${PWD}/servable:/workspace -v ${PWD}/model_conversion/saved_mod/siglip:/saved_mod/siglip -v ${PWD}/model_conversion/saved_mod/clip:/saved_mod/clip -v ${PWD}/model_conversion/saved_mod/laion:/saved_mod/laion ovms-gpu-custom --config_path /workspace/config_model.json --port 9000 --rest_port 8000
```

## Deploying the Vector Database

The next step is to start the vector database. In this project, we are using **Qdrant**, an open-source vector database optimized for efficient semantic search.

Run the following command to start Qdrant with Docker:

```bash
cd demos/python_demos/image_embeddings
docker run -p 6333:6333 qdrant/qdrant
```


