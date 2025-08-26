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



flowchart TD

subgraph A[Model Conversion]
    A1[Convert CLIP → IR]
    A2[Convert SigLIP → IR]
    A3[Convert BLIP → IR]
end

subgraph B[Servable Pipeline]
    B1[Preprocessing (resize, normalize)]
    B2[OpenVINO Model Server]
    B3[Postprocessing (embedding extraction)]
    B4[MediaPipe Graphs]
end

subgraph C[gRPC CLI]
    C1[Loop through folder of images]
    C2[Generate embeddings]
    C3[Store in Vector DB]
end

subgraph D[Search API]
    D1[Query Image]
    D2[Generate embedding]
    D3[Vector DB Lookup]
    D4[Return Similar Images]
end

A --> B
B --> C
C --> D





