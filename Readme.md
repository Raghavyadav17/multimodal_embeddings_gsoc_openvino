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
python clip_conversion.py (for clip)
python laion_conversion.py (for laion)
python siglip_conversion.py (for siglip)



