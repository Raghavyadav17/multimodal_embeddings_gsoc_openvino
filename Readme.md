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




